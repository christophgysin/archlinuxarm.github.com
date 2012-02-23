# Arch Linux ARM #

<div class="meta subtitle">Developer Documentation</div>

## Cross-Compiling with DistCC ##


## Vocabulary ##
* Client: The x86/x86_64 computer that will get work from ALARM, cross-compile it, and send it back.
* Crosstool-NG: This will build a cross-compiler on your client (x86/x86_64) computer.
* DistCC: Runs on both client and server, used for sending code and binaries back and forth easily.
* Master: The ARM device running Arch Linux ARM, with DistCC installed and running.

## Configuring the Master ##
Just point distcc to your client's (x86/x86_64 computer) IP address and adjust the `-j` flag in /etc/makepkg.conf.

## Installation on the Client ##