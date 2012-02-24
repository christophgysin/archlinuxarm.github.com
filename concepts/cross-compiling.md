# Arch Linux ARM #

<div class="meta subtitle">Developer Documentation</div>

## Cross-Compiling with DistCC ##
This is the official cross-compiling method used at Arch Linux ARM. The following guide will turn an Arch Linux or Ubuntu/Debian Linux box into an ARM cross-compiler.

## Vocabulary ##
* Client: The x86/x86_64 computer that will get work from ALARM, cross-compile it, and send it back.
* Crosstool-NG: This will build a cross-compiler on your client (x86/x86_64) computer.
* DistCC: Runs on both client and server, used for sending code and binaries back and forth easily.
* Master: The ARM device running Arch Linux ARM, with DistCC installed and running.

## Configuring the Master Device ##
Just point distcc to your client's (x86/x86_64 computer) IP address and adjust the `-j` flag in /etc/makepkg.conf.

## Installation on the Client ##

### Ubuntu/Debian as a Client ###
If you're planning to use Ubuntu or Debian as a client, note the following. The guide below uses Arch Linux directories and file names.

* "/etc/conf.d/distccd" is "/etc/default/distcc"
* Start distcc with "/etc/init.d/distcc start"
* Run the following to install everything you need:
`sudo apt-get install texinfo libncurses5-dev automake gawk build-essential bison flex libtool cvs curl`

### Install crosstool-ng ###
This process is very automated, courtesy of [crosstool-ng](http://crosstool-ng.org). As a normal user (**not root!**), download the latest version to a directory called "cross" in your home directory and extract it. Enter the source directory and configure with a prefix for the "cross" directory, make, and make install:

    mkdir -p cross/src
    cd cross
    wget http://crosstool-ng.org/download/crosstool-ng/crosstool-ng-1.13.0.tar.bz2
    tar -xjf crosstool-ng-1.13.0.tar.bz2
    cd crosstool-ng-1.13.0/
    ./configure --prefix=/home/your_user/cross
    make
    make install

At this point crosstool-ng is ready to be configured. "ct-ng" in the "bin" directory is where the magic happens. It also has a menu configuration like the Linux kernel.

### Downloading a CrossTool Configuration ###
Download the default .config file to place in "~/cross/bin" as shown below. **Once you do this, do not run "menuconfig" or values will be overwritten**. Choose either "v5" or "v7" depending on the target platform.

    cd /home/your_user/cross/bin/
    wget http://archlinuxarm.org/mirror/development/ct-ng/xtools-dotconfig-[v5|v7]
    mv xtools-dotconfig-[v5|v7] .config
    ./ct-ng build

### Make Symlinks for DistCC ###
The toolchain will install under "~/x-tools" for armv5 and "~/x-tools7h" for armv7 (hard-float).  You can move this somewhere else if you like, or leave it where it is. Before we can use the compiler binaries that were created, links need to be created to make their names more appropriate. When compile jobs are sent to distcc, the program specified in the CC environment variable on the build master is what gets executed on the build clients. All the binaries that have been produced by crosstool-ng are in the (correct) format specifying the target platform as a prefix. This script will fix our problem. Make "~/x-tools/arm-unknown-linux-gnueabi/bin/" writable and in there create a file called "link" and paste this into it. Make it executable ("chmod +x") and run it afterward.

    #!/bin/bash
    for file in `ls`; do
        if [[ "$file" == "link" ]]; then
                continue
        fi
        ln -s $file ${file#arm-unknown-linux-gnueabi-}
    done

Now the "bin" directory contains links with names that distcc will play nice with. To get distcc to use these binaries instead of the default system ones, we need to place this directory into the path for the distcc daemon:

* Debian/Ubuntu: Edit "/etc/init.d/distcc"
* Arch Linux: Edit "/etc/rc.d/distccd"

After the initial header block, add this line or modify PATH if it already exists in the file. Note that we are placing our binary directory at the very front.
`PATH=/home/your_user/x-tools/arm-unknown-linux-gnueabi/bin:$PATH`

At this point, start/restart distcc:

* Debian/Ubuntu: /etc/init.d/distcc start|restart
* Arch Linux: /etc/rc.d/distccd start|restart

### Compiling Packages ###
Back on ALARM master device, make sure that distcc has been enabled in makepkg.conf and specify the cross-compiler computer's IP address in the DISTCC_HOSTS variable. Build packages like you normally would with makepkg.