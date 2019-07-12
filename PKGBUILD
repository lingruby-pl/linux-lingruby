# Maintainer: Jacek Bienias <sp7ezd@gmail.com>
# Maintainer: Piotr Gorski <lucjan.lucjanov@gmail.com>
# Contributor: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Contributor: Tobias Powalowski <tpowa@archlinux.org>
# Contributor: Thomas Baechler <thomas@archlinux.org>

### BUILD OPTIONS
# Set these variables to ANYTHING that is not null to enable them

### Tweak kernel options prior to a build via nconfig
_makenconfig=

### Tweak kernel options prior to a build via menuconfig
_makemenuconfig=

### Tweak kernel options prior to a build via xconfig
_makexconfig=

### Tweak kernel options prior to a build via gconfig
_makegconfig=

### Setting GCC Flags for CONFIG_MHASWELL
_gcc_haswell=y

### Set performance governor as default
_per_gov=y

# NUMA is optimized for multi-socket motherboards.
# A single multi-core CPU actually runs slower with NUMA enabled.
# See, https://bugs.archlinux.org/task/31187
_NUMAdisable=y

# Compile ONLY probed modules
# Build in only the modules that you currently have probed in your system VASTLY
# reducing the number of modules built and the build time.
#
# WARNING - ALL modules must be probed BEFORE you begin making the pkg!
#
# To keep track of which modules are needed for your specific system/hardware,
# give module_db script a try: https://aur.archlinux.org/packages/modprobed-db
# This PKGBUILD will call it directly to probe all the modules you have logged!
#
# More at this wiki page ---> https://wiki.archlinux.org/index.php/Modprobed-db
_localmodcfg=

# Use the current kernel's .config file
# Enabling this option will use the .config of the RUNNING kernel rather than
# the ARCH defaults. Useful when the package gets updated and you already went
# through the trouble of customizing your config options.  NOT recommended when
# a new kernel is released, but again, convenient for package bumps.
_use_current=

### Disable Deadline I/O scheduler
_deadline_disable=y

### Disable Kyber I/O scheduler
_kyber_disable=y

### Running with a 1000 HZ tick rate
_1k_HZ_ticks=y

### Enable WireGuard
_net_udp_tunnel=y
_wireguard=y

### Download linux-lucjan patchset
# ATTENTION - one of four predefined values should be selected!
# 'actual' - stable releases (recommended)
# 'testing' - rc releases (active only temporarily)
# 'trunk' - pre-rc releases (active only temporarily)
_linux_lucjan='actual'

### Do not edit below this line unless you know what you're doing

pkgbase=linux-lingruby-git
_pkgbase=linux-lucjan
# pkgname=('linux-lingruby-git-kernel' 'linux-lingruby-git-headers')
_major=5.2
_srcname=linux
pkgver=5.2.r0.g0ecfebd2b524.ll1
pkgrel=1
arch=('x86_64')
url="https://github.com/lingruby-pl/linux-lingruby"
license=('GPL2')
options=('!strip')
makedepends=('kmod' 'inetutils' 'bc' 'libelf' 'git')

if [ "$_linux_lucjan" = "actual" ]; then

 _lucjanpath="https://raw.githubusercontent.com/sirlucjan/${_pkgbase}/master/actual"
# _lucjanpath="https://gitlab.com/sirlucjan/${_pkgbase}/raw/master/actual"
 _lucjanname='lucjan'
 _lucjanrev='ll1'
 _lucjanpatch="${_major}-${_lucjanname}-${_lucjanrev}.patch"

elif [ "$_linux_lucjan" = "testing" ]; then

 _lucjanpath="https://raw.githubusercontent.com/sirlucjan/${_pkgbase}/master/testing"
# _lucjanpath="https://gitlab.com/sirlucjan/${_pkgbase}/raw/master/testing"
 _lucjanname='lucjan'
 _lucjanrev='ll11'
 _lucjanrc='rc1'
 _lucjanpatch="${_major}-${_lucjanname}-${_lucjanrev}-${_lucjanrc}.patch"

elif [ "$_linux_lucjan" = "trunk" ]; then

_lucjanpath="https://raw.githubusercontent.com/sirlucjan/${_pkgbase}/master/trunk"
# _lucjanpath="https://gitlab.com/sirlucjan/${_pkgbase}/raw/master/trunk"
_lucjanname='lucjan'
_lucjanrev='ll1'
_lucjanrc='rc3'
_lucjanpatch="${_major}-${_lucjanname}-${_lucjanrev}-${_lucjanrc}.patch"

fi

source=("git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git#branch=linux-${_major}.y"
        "${_lucjanpath}/${_lucjanpatch}"{,.sig}
         # the main kernel config files
        'config'
         # pacman hook for depmod
        '60-linux-01-depmod.hook'
         # pacman hook for initramfs regeneration
        '90-linux-02-initcpio.hook'
         # pacman hook for remove initramfs
        '99-linux-03-remove.hook'
         # standard config files for mkinitcpio ramdisk
        'linux.preset')

_kernelname=${pkgbase#linux}
: ${_kernelname:=-lingruby-git+}

pkgver() {
    cd $_srcname

    _ver="$(git describe --long --tags | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g')"
    printf ${_ver}.${_lucjanrev}
}

prepare() {
    cd $_srcname

    ### Setting version
        msg2 "Setting version..."
        sed -e "/^EXTRAVERSION =/s/=.*/=/" -i Makefile
        touch .scmversion
        scripts/setlocalversion --save-scmversion
        echo "-$pkgrel" > localversion.10-pkgrel
        echo "$_kernelname" > localversion.20-pkgname

    ### Patching sources
        local src
        for src in "${source[@]}"; do
          src="${src%%::*}"
          src="${src##*/}"
          [[ $src = *.patch ]] || continue
          msg2 "Applying patch $src..."
          patch -Np1 < "../$src"
        done

    ### Setting config
        msg2 "Setting config..."
        cp ../config .config

    ### Optionally use running kernel's config
	# code originally by nous; http://aur.archlinux.org/packages.php?ID=40191
	if [ -n "$_use_current" ]; then
		if [[ -s /proc/config.gz ]]; then
			msg2 "Extracting config from /proc/config.gz..."
			# modprobe configs
			zcat /proc/config.gz > ./.config
		else
			warning "You kernel was not compiled with IKCONFIG_PROC!"
			warning "You cannot read the current config!"
			warning "Aborting!"
			exit
		fi
	fi

    ### Set GCC flags
        if [ -n "$_gcc_haswell" ]; then
                msg2 "Setting GCC flags for Intel Haswell CPU..."
                sed -i -e s'/^CONFIG_GENERIC_CPU=y/# CONFIG_GENERIC_CPU is not set/' \
                    -i -e s'/^# CONFIG_MHASWELL is not set/CONFIG_MHASWELL=y/' ./.config
        fi

    ### Set performance governor
        if [ -n "$_per_gov" ]; then
                msg2 "Setting performance governor..."
                sed -i -e s'/^CONFIG_CPU_FREQ_DEFAULT_GOV_SCHEDUTIL=y/# CONFIG_CPU_FREQ_DEFAULT_GOV_SCHEDUTIL is not set/' \
                    -i -e s'/^# CONFIG_CPU_FREQ_DEFAULT_GOV_PERFORMANCE is not set/CONFIG_CPU_FREQ_DEFAULT_GOV_PERFORMANCE=y/' ./.config
                msg2 "Disabling uneeded governors..."
                sed -i -e s'/^CONFIG_CPU_FREQ_GOV_ONDEMAND=m/# CONFIG_CPU_FREQ_GOV_ONDEMAND is not set/' \
                    -i -e s'/^CONFIG_CPU_FREQ_GOV_CONSERVATIVE=m/# CONFIG_CPU_FREQ_GOV_CONSERVATIVE is not set/' \
                    -i -e s'/^CONFIG_CPU_FREQ_GOV_USERSPACE=m/# CONFIG_CPU_FREQ_GOV_USERSPACE is not set/' \
                    -i -e s'/^CONFIG_CPU_FREQ_GOV_SCHEDUTIL=y/# CONFIG_CPU_FREQ_GOV_SCHEDUTIL is not set/' ./.config
        fi

    ### Disable Deadline I/O scheduler
	if [ -n "$_deadline_disable" ]; then
		msg2 "Disabling Deadline I/O scheduler..."
		sed -i -e s'/^CONFIG_MQ_IOSCHED_DEADLINE=y/# CONFIG_MQ_IOSCHED_DEADLINE is not set/' ./.config
	fi

    ### Disable Kyber I/O scheduler
	if [ -n "$_kyber_disable" ]; then
		msg2 "Disabling Kyber I/O scheduler..."
		sed -i -e s'/^CONFIG_MQ_IOSCHED_KYBER=y/# CONFIG_MQ_IOSCHED_KYBER is not set/' ./.config
	fi

    ### Optionally set tickrate to 1000
	if [ -n "$_1k_HZ_ticks" ]; then
		msg2 "Setting tick rate to 1k..."
		sed -i -e 's/^CONFIG_HZ_300=y/# CONFIG_HZ_300 is not set/' \
                    -i -e 's/^# CONFIG_HZ_1000 is not set/CONFIG_HZ_1000=y/' \
                    -i -e 's/^CONFIG_HZ=300/CONFIG_HZ=1000/' ./.config
	fi

    ### Optionally disable NUMA for 64-bit kernels only
        # (x86 kernels do not support NUMA)
        if [ -n "$_NUMAdisable" ]; then
            msg2 "Disabling NUMA from kernel config..."
            sed -i -e 's/CONFIG_NUMA=y/# CONFIG_NUMA is not set/' \
                -i -e '/CONFIG_AMD_NUMA=y/d' \
                -i -e '/CONFIG_X86_64_ACPI_NUMA=y/d' \
                -i -e '/CONFIG_NODES_SPAN_OTHER_NODES=y/d' \
                -i -e '/# CONFIG_NUMA_EMU is not set/d' \
                -i -e '/CONFIG_NODES_SHIFT=6/d' \
                -i -e '/CONFIG_NEED_MULTIPLE_NODES=y/d' \
                -i -e '/# CONFIG_MOVABLE_NODE is not set/d' \
                -i -e '/CONFIG_USE_PERCPU_NUMA_NODE_ID=y/d' \
                -i -e '/CONFIG_ACPI_NUMA=y/d' ./.config
        fi

    ### Optionally load needed modules for the make localmodconfig
        # See https://aur.archlinux.org/packages/modprobed-db
        if [ -n "$_localmodcfg" ]; then
        msg2 "If you have modprobed-db installed, running it in recall mode now"
            if [ -e /usr/bin/modprobed-db ]; then
            [[ -x /usr/bin/sudo ]] || {
            echo "Cannot call modprobe with sudo. Install sudo and configure it to work with this user."
            exit 1; }
            sudo /usr/bin/modprobed-db recall
            make localmodconfig
            fi
        fi

    ### Enable WireGuard
        if [ ! -e "$_net_udp_tunnel" ]; then
            msg2 "Enabling WireGuard..."
            sed -i s'/^CONFIG_NET_UDP_TUNNEL=m/CONFIG_NET_UDP_TUNNEL=y/' ./.config
        fi
        if [ ! -e "$_wireguard" ]; then
            msg2 "Enabling WireGuard..."
            sed -i s'/^# CONFIG_WIREGUARD is not set/CONFIG_WIREGUARD=y/' ./.config
        fi

    ### Rewrite configuration
        msg2 "Rewrite configuration"
        make prepare
        yes "" | make config >/dev/null

    ### Prepared version
        make -s kernelrelease > ../version
        msg2 "Prepared %s version %s" "$pkgbase" "$(<../version)"

    ### Running make nconfig

	[[ -z "$_makenconfig" ]] ||  make nconfig

    ### Running make menuconfig

	[[ -z "$_makemenuconfig" ]] || make menuconfig

    ### Running make xconfig

	[[ -z "$_makexconfig" ]] || make xconfig

    ### Running make gconfig

	[[ -z "$_makegconfig" ]] || make gconfig

    ### Save configuration for later reuse
	msg2 "Save configuration for later reuse..."
	cat .config > "${startdir}/config.last"
}

build() {
  cd $_srcname

  make bzImage modules
}

_package-kernel() {
  pkgdesc="The ${pkgbase/linux/Linux} kernel and modules with the ${_lucjanname}-${_lucjanrev} patchset. Fourth Gen Intel Core i3/i5/i7 optimized."
  depends=('coreutils' 'linux-firmware' 'mkinitcpio')
  optdepends=('crda: to set the correct wireless channels of your country' 'modprobed-db: Keeps track of EVERY kernel module that has ever been probed - useful for those of us who make localmodconfig')
  groups=('linux-lingruby-git')
  backup=("etc/mkinitcpio.d/$pkgbase.preset")
  install=linux.install

  local kernver="$(<version)"
  local modulesdir="$pkgdir/usr/lib/modules/$kernver"

  cd $_srcname

  msg2 "Installing boot image..."
  # systemd expects to find the kernel here to allow hibernation
  # https://github.com/systemd/systemd/commit/edda44605f06a41fb86b7ab8128dcf99161d2344
  install -Dm644 "$(make -s image_name)" "$modulesdir/vmlinuz"
  install -Dm644 "$modulesdir/vmlinuz" "$pkgdir/boot/vmlinuz-$pkgbase"

  msg2 "Installing modules..."
  make INSTALL_MOD_PATH="$pkgdir/usr" modules_install

  # a place for external modules,
  # with version file for building modules and running depmod from hook
  local extramodules="extramodules$_kernelname"
  local extradir="$pkgdir/usr/lib/modules/$extramodules"
  install -Dt "$extradir" -m644 ../version
  ln -sr "$extradir" "$modulesdir/extramodules"

  # remove build and source links
  rm "$modulesdir"/{source,build}

  msg2 "Installing hooks..."

  # sed expression for following substitutions
  local subst="
    s|%PKGBASE%|$pkgbase|g
    s|%KERNVER%|$kernver|g
    s|%EXTRAMODULES%|$extramodules|g
  "

  # hack to allow specifying an initially nonexisting install file
  sed "$subst" "$startdir/$install" > "$startdir/$install.pkg"
  true && install=$install.pkg

  # fill in mkinitcpio preset and pacman hooks
  sed "$subst" ../linux.preset | install -Dm644 /dev/stdin \
    "$pkgdir/etc/mkinitcpio.d/$pkgbase.preset"
  sed "$subst" ../60-linux-01-depmod.hook | install -Dm644 /dev/stdin \
    "$pkgdir/usr/share/libalpm/hooks/60-${pkgbase}-01-depmod.hook"
  sed "$subst" ../90-linux-02-initcpio.hook | install -Dm644 /dev/stdin \
    "$pkgdir/usr/share/libalpm/hooks/90-${pkgbase}-02-initcpio.hook"
  sed "$subst" ../99-linux-03-remove.hook | install -Dm644 /dev/stdin \
    "$pkgdir/usr/share/libalpm/hooks/99-${pkgbase}-03-remove.hook"

  msg2 "Fixing permissions..."
  chmod -Rc u=rwX,go=rX "$pkgdir"
}

_package-headers() {
  pkgdesc="Header files and scripts for building modules for ${pkgbase/linux/Linux} kernel. Fourth Gen Intel Core i3/i5/i7 optimized."
  depends=('linux-lingruby-git-kernel')
  groups=('linux-lingruby-git')

  local builddir="$pkgdir/usr/lib/modules/$(<version)/build"

  cd $_srcname

  msg2 "Installing build files..."
  install -Dt "$builddir" -m644 Makefile .config Module.symvers System.map vmlinux
  install -Dt "$builddir/kernel" -m644 kernel/Makefile
  install -Dt "$builddir/arch/x86" -m644 arch/x86/Makefile
  cp -t "$builddir" -a scripts

  # add objtool for external module building and enabled VALIDATION_STACK option
  install -Dt "$builddir/tools/objtool" tools/objtool/objtool

  # add xfs and shmem for aufs building
  mkdir -p "$builddir"/{fs/xfs,mm}

  # ???
  mkdir "$builddir/.tmp_versions"

  msg2 "Installing headers..."
  cp -t "$builddir" -a include
  cp -t "$builddir/arch/x86" -a arch/x86/include
  install -Dt "$builddir/arch/x86/kernel" -m644 arch/x86/kernel/asm-offsets.s

  install -Dt "$builddir/drivers/md" -m644 drivers/md/*.h
  install -Dt "$builddir/net/mac80211" -m644 net/mac80211/*.h

  # http://bugs.archlinux.org/task/13146
  install -Dt "$builddir/drivers/media/i2c" -m644 drivers/media/i2c/msp3400-driver.h

  # http://bugs.archlinux.org/task/20402
  install -Dt "$builddir/drivers/media/usb/dvb-usb" -m644 drivers/media/usb/dvb-usb/*.h
  install -Dt "$builddir/drivers/media/dvb-frontends" -m644 drivers/media/dvb-frontends/*.h
  install -Dt "$builddir/drivers/media/tuners" -m644 drivers/media/tuners/*.h

  msg2 "Installing KConfig files..."
  find . -name 'Kconfig*' -exec install -Dm644 {} "$builddir/{}" \;

  msg2 "Removing unneeded architectures..."
  local arch
  for arch in "$builddir"/arch/*/; do
    [[ $arch = */x86/ ]] && continue
    echo "Removing $(basename "$arch")"
    rm -r "$arch"
  done

  msg2 "Removing broken symlinks..."
  find -L "$builddir" -type l -printf 'Removing %P\n' -delete

  msg2 "Removing loose objects..."
  find "$builddir" -type f -name '*.o' -printf 'Removing %P\n' -delete

  msg2 "Stripping build tools..."
  local file
  while read -rd '' file; do
    case "$(file -bi "$file")" in
      application/x-sharedlib\;*)      # Libraries (.so)
        strip -v $STRIP_SHARED "$file" ;;
      application/x-archive\;*)        # Libraries (.a)
        strip -v $STRIP_STATIC "$file" ;;
      application/x-executable\;*)     # Binaries
        strip -v $STRIP_BINARIES "$file" ;;
      application/x-pie-executable\;*) # Relocatable binaries
        strip -v $STRIP_SHARED "$file" ;;
    esac
  done < <(find "$builddir" -type f -perm -u+x ! -name vmlinux -print0)

  msg2 "Adding symlink..."
  mkdir -p "$pkgdir/usr/src"
  ln -sr "$builddir" "$pkgdir/usr/src/$pkgbase-$pkgver"

  msg2 "Fixing permissions..."
  chmod -Rc u=rwX,go=rX "$pkgdir"
}

pkgname=("$pkgbase-kernel" "$pkgbase-headers")
for _p in "${pkgname[@]}"; do
  eval "package_$_p() {
    $(declare -f "_package${_p#$pkgbase}")
    _package${_p#$pkgbase}
  }"
done

sha512sums=('SKIP'
            'a1eb7e7fd376287740296cbd87a3edeea3fefe84e354abd4ddbd1479581a3201f3b333426ba06accd8d1c354fdf9e4338f71688cb607231aa2080ebc62a6915e'
            'SKIP'
            'f7eeeb09e5bfb20c80553103fc69c2e34532373eed10ac17ae97029053866b9061ca0fcd407030bb8ca6353815b2566e47dcd78394dd5357e97dc03b744adacd'
            '7ad5be75ee422dda3b80edd2eb614d8a9181e2c8228cd68b3881e2fb95953bf2dea6cbe7900ce1013c9de89b2802574b7b24869fc5d7a95d3cc3112c4d27063a'
            '6d474f9f8ccdbbf3cfc10987b2b11a405bfbbac6d77d17c727c83f297c43678db57b5af7fecc365de01474183cbd53dd70d77601465ee34a0be0d983c42e14d1'
            '8742e2eed421e2f29850e18616f435536c12036ff793f5682a3a8c980cf5dbfc88d17fd9539c87de15d9e4663dc3190f964f18a4722940465437927b6052abbf'
            '2dc6b0ba8f7dbf19d2446c5c5f1823587de89f4e28e9595937dd51a87755099656f2acec50e3e2546ea633ad1bfd1c722e0c2b91eef1d609103d8abdc0a7cbaf')

validpgpkeys=(
              '399521CE9D6D65B35EEF0F8C79AFA05ABDB26C5A' # Piotr Gorski
             )
