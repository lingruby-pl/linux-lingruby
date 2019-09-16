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
###### patchset  [linux-lucjan](https://github.com/sirlucjan/linux-lucjan) 


* [bfq improvements](https://groups.google.com/forum/#!forum/bfq-iosched) - latest fixes authored by Paolo Valente and BFQ Team

* [bfq-dev](https://github.com/Algodev-github/bfq-mq/commits/dev-bfq-on-5.2) - latest fixes authored by Paolo Valente and BFQ Team

* [bfq-lucjan-dev](https://github.com/sirlucjan/bfq-mq-lucjan/commits/dev-bfq-on-5.2-lucjan) - latest fixes authored by Paolo Valente and BFQ Team and forked by Piotr Gorski
 
* [graysky's GCC patch](https://github.com/graysky2/kernel_gcc_patch) - version for gcc 9.1

* [BMQ](https://gitlab.com/alfredchen/bmq) / [BMQ blog](http://cchalpha.blogspot.com) - contains the newest vesion with latest fixes

* [random fixes from zen-kernel](https://github.com/zen-kernel/zen-kernel) - specific patches authored by Jan Alexander Steffens and ZEN Kernel Team

* [random fixes from pfkernel](https://github.com/pfactum/pf-kernel) - for example patches from Arch Linux or specific patches authored by Oleksandr Natalenko

* [fixes from ClearLinux](https://github.com/clearlinux-pkgs/linux) - specific patches authored by ClearLinux Team

* [AUFS](https://github.com/sfjro/aufs5-standalone) / [AUFS](http://aufs.sourceforge.net) - advanced multi-layered unification filesystem

* [WireGuard](https://git.zx2c4.com/WireGuard) - fast and secure kernelspace VPN

* [LL-patches](https://github.com/sirlucjan/kernel-patches/tree/master/5.1/ll-patches) / [LL-patches](https://gitlab.com/sirlucjan/kernel-patches/tree/master/5.1/ll-patches) - specific patches authored by Piotr Gorski

* [LL-branding](https://github.com/sirlucjan/kernel-patches/tree/master/5.1/ll-branding) / [LL-branding](https://gitlab.com/sirlucjan/kernel-patches/tree/master/5.1/ll-branding) - specific patches authored by Piotr Gorski

***

###### Some patches for BFQ conflict with patches for BFQ-dev.

###### To use lucjan-kernels smoothly apply bfq-reverts before linux-lucjan patch. Otherwise the kernel will not compile.

* [bfq-reverts](https://github.com/sirlucjan/kernel-patches/tree/master/5.2/bfq-reverts) / [bfq-reverts](https://gitlab.com/sirlucjan/kernel-patches/tree/master/5.2/bfq-reverts) - specific patches authored by Piotr Gorski



***



# no Docs
