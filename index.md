# Arch Linux ARM #

<div class="meta subtitle">Developer Documentation</div>

## Overview ##
Arch Linux ARM (ALARM) is a Linux distribution for ARM computers.

We support ARMv5, OXNAS-based ARMv6, Cortex-A8, Cortex-A9, and Tegra platforms. Our collaboration with Arch Linux (Upstream) brings users the best platform, newest packages, and installation support.

This collection of pages is meant for people interested in how the ALARM ecosystem works. This documentation is meant to be short and easy to maintain. Its main focus is on the ALARM distribution and how it is built, not the hardware it runs on.

## Design/OS Concepts ##
* [ABS (Upstream)](https://wiki.archlinux.org/index.php/Arch_Build_System) - Arch Linux Upstream PKGBUILDs
* [ALARM PKGBUILDs](https://github.com/archlinuxarm/PKGBUILDs) - Custom PKGBUILDs for ARM
* [DistCC](#!/concepts/distcc) - DistCC, used to send source to another computer to build (general)
* [Cross-Compiling with DistCC](#!/concepts/cross-compiling) - Using DistCC to cross-compile from ALARM to x86/x86_64

## Main Projects ##
* [ALARM PKGBUILDs](https://github.com/archlinuxarm/PKGBUILDs) - Fix/maintain Upstream PKGBUILDs for ARM
* [PlugBuild](#!/projects/plugbuild) - Automated ALARM repository builder, server and client
* [PlugUI](https://github.com/archlinuxarm/PlugUI) - Web interface for ALARM

## Side Projects/Ideas ##
* [Google Summer of Code 2012](#!/ideas/gsoc) - Maybe?
* The PandaFarm - A cluster of 6-8 PandaBoards in a slick, custom-designed rack.

## Porting to New Devices ##
* Coming soon

## How to Get Involved with Developing ALARM ##
1. Join IRC, and optionally ask what needs to be done.
2. Clone a Git repository, make changes, and submit a pull request.
3. Go back on IRC and let others know!
4. Over time, if you show a desire to help the project out, you'll get permissions to more and more things.

## Todo ##
Each project with a Github repository stores Todo items in its "Issues" section.

## Other Ways to Help ##
* We're slowly gaining mirrors worldwide. If you have a reliable server where you can mirror ALARM packages, let us know on IRC.
* We do not need any more dedicated "builders". If you're interested in helping us build packages, let us know on IRC.
* We need ARM devices. If you are an OEM or anyone with access to ARM devices, we could use your help. Contact us by email or on IRC.
* We could use some dedicated/VPS/colocation hosting or discounts. Arch Linux ARM is 100% funded by donations.

## Additional Reading ##
* [Arch Boot Process](https://wiki.archlinux.org/index.php/Arch_Boot_Process) - Step-by-step walkthrough of boot-up
* [rc.conf](https://wiki.archlinux.org/index.php/Rc.conf) - rc.conf, the main configuation file