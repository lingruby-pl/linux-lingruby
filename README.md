# My kernel linux-lingruby-git

###### for 

###### Machine:   
######            Type: Laptop System: Acer product: Aspire E5-571 v: V1.32 serial: NXML8EP0034480810E3400 Chassis: 
######            type: 10 serial: N/A 
######            Mobo: Acer model: EA50_HB v: V1.32 serial: NBV9M11001448425A13400 UEFI: Insyde v: 1.32 
######            date: 09/15/2015 
###### CPU:       
######            Topology: Dual Core model: Intel Core i3-4005U bits: 64 type: MT MCP arch: Haswell rev: 1 
######            L1 cache: 64 KiB L2 cache: 3072 KiB L3 cache: 3072 KiB 
######            flags: lm nx pae sse sse2 sse3 sse4_1 sse4_2 ssse3 vmx bogomips: 13568 
######            Speed: 1696 MHz min/max: 800/1700 MHz Core speeds (MHz): 1: 1696 2: 1696 3: 1696 4: 1696

***
###### Patches developed and offered only for the last, stable line (not EOL). 
###### Active support is offered only for this kernel line. 
###### Any others are not supported anymore, but you can find patchsets for previous versions of kernel in "archive" directory. 
###### The patchset not includes upstream bugfixes patches from kernel line 5.XX. 
###### You should applied them on your own. It should be possible to applied patchset on any supported version of kernel, but I personally support only the last possible version of kernel from stable branch.
###### The patchset contains various patches developed by their authors, always in the newest versions and sometimes with fixes from master branch or from the other authors when it's necessary. 

***

###### patchset  [linux-lucjan](https://github.com/sirlucjan/linux-lucjan) 


* [bfq improvements](https://groups.google.com/forum/#!forum/bfq-iosched) - latest fixes authored by Paolo Valente and BFQ Team

* [bfq-dev](https://github.com/Algodev-github/bfq-mq/tree/dev-bfq-on-5.5) - latest fixes authored by Paolo Valente and BFQ Team

* [bfq-lucjan-dev](https://github.com/sirlucjan/bfq-mq-lucjan/tree/dev-bfq-on-5.5-lucjan) - latest fixes authored by Paolo Valente and BFQ Team and forked by Piotr Gorski

* [graysky's GCC patch](https://github.com/graysky2/kernel_gcc_patch) - version for gcc 9.1

* [BMQ](https://gitlab.com/alfredchen/linux-bmq/tree/linux-5.5.y-bmq) / [BMQ blog](http://cchalpha.blogspot.com) - contains the newest vesion with latest fixes

* [random fixes from zen-kernel](https://github.com/zen-kernel/zen-kernel/tree/5.5/master) - specific patches authored by Jan Alexander Steffens and ZEN Kernel Team

* [random fixes from pfkernel](https://github.com/pfactum/pf-kernel/tree/pf-5.5) / [random fixes from pfkernel](https://gitlab.com/post-factum/pf-kernel/tree/pf-5.5) - for example patches from Arch Linux or specific patches authored by Oleksandr Natalenko

* [fixes from ClearLinux](https://github.com/clearlinux-pkgs/linux) - specific patches authored by ClearLinux Team

* [UKSM (sources)](https://github.com/dolohow/uksm) / [UKSM (info)](https://www.usenix.org/sites/default/files/conference/protected-files/fast18_slides_xia.pdf) - resync from dolohow’s github

* [LL-patches](https://github.com/sirlucjan/kernel-patches/tree/master/5.5/ll-patches) / [LL-patches](https://gitlab.com/sirlucjan/kernel-patches/tree/master/5.5/ll-patches) - specific patches authored by Piotr Gorski

* [LL-branding](https://github.com/sirlucjan/kernel-patches/tree/master/5.5/ll-branding) / [LL-branding](https://gitlab.com/sirlucjan/kernel-patches/tree/master/5.5/ll-branding) - specific patches authored by Piotr Gorski

* [WireGuard](https://git.zx2c4.com/WireGuard) - fast and secure kernelspace VPN

***

###### Some patches for BFQ conflict with patches for BFQ-dev.

###### To use lucjan-kernels smoothly apply bfq-reverts before linux-lucjan patch. Otherwise the kernel will not compile.

* [bfq-reverts](https://github.com/sirlucjan/kernel-patches/tree/master/5.5/bfq-reverts) / [bfq-reverts](https://gitlab.com/sirlucjan/kernel-patches/tree/master/5.5/bfq-reverts) - specific patches authored by Piotr Gorski



***



# no Docs
