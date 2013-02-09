# Arch Linux ARM #

<div class="meta subtitle">Developer Documentation</div>

## DistCC ##
This is the official cross-compiling method used at Arch Linux ARM. If you plan on building a lot of packages and want to speed up the process, the following guide will turn an Arch Linux or Ubuntu/Debian Linux box into an ARM cross-compiler.

If you have multiple plugs, using distcc is a very simple way to get all of them in on compiling packages. `makepkg` is already set up for using distcc, all that's needed is to enable the functionality.

### Installation ###

Install distcc on all devices to be used:

    pacman -Sy distcc

### Configuration: Introduction ###
For the purposes of this example, all of the hosts are in the local network of 10.3.0.0/24, and we'll allow access from that entire subnet. If you want to restrict access further, you will have to specify individual IPs. Be sure to change the address from the examples to fit your local network configuration.

Additionally, this example assumes that you are starting compiles from one specific device (master), which is sending out to the other device(s) (client). If you want to compile from any device, simply apply the client configuration to the master device, and the master device configuration to the client device(s).

If you want to use the master device to compile as well, apply the client configuration to it.

### Configuration: Master Device ###
This is what you run on the ARM device you'll be running `makepkg` from and using to coordinate builds.

In /etc/makepkg.conf, change `BUILDENV` to un-negate the distcc option:

    BUILDENV=(fakeroot distcc color !ccache)

Change `DISTCC_HOSTS` to a space-delimited list of the distcc clients on your network. If you want to use the master plug, be sure to put its IP address in there too. The hosts listed will be prioritized in the order listed. To keep configurations simple, use the LAN IP address for the local plug, not ''localhost'' or ''127.0.0.1''.

    DISTCC_HOSTS="10.3.0.6 10.3.0.2"

Change `MAKEFLAGS -j` flag to reflect the total number of processors available, plus 1.

### Configuration: Client Device(s) ###
This is what you'd run on a second ARM device or a cross-compiling computer.

In /etc/conf.d/distccd, change `DISTCC_ARGS` to reflect the hosts or network you're allowing to connect:

    DISTCC_ARGS="--allow 10.3.0.0/24"

In /etc/hosts.allow, define distccd hosts or network, otherwise the master won't be able to connect (distcc runs on port 3632):

    distccd: 10.3.0.0/24

Add distccd to the `DAEMONS` line in /etc/rc.conf to start distccd on boot and start the daemon:

    DAEMONS=(syslog-ng network ... distccd)
    systemctl enable distccd
    systemctl start distccd.service

You can now run `makepkg` as you normally would and the hosts specified will start receiving work. If the hosts are unavailable, the package will simply compile locally.
