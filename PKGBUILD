# U-Boot: ROC-RK3328-CC
# Maintainer: magic0whi <lollipop.studio.cn@gmail.com>

buildarch=8

pkgname=uboot-roc-rk3328-cc-git
_pkgname=u-boot
pkgver=r48813.9e07df4964
pkgrel=1
pkgdesc="U-Boot for ROC-RK3328-CC"
arch=('aarch64')
url='http://www.denx.de/wiki/U-Boot/WebHome'
license=('GPL')
backup=('boot/boot.txt' 'boot/boot.scr')
makedepends=('bc' 'git' 'rockchip-tools')
install=${pkgname}.install
source=("git+https://github.com/rockchip-linux/u-boot#branch=next-dev"
        'rk3328trust.ini'
        'boot.txt'
        'mkscr'
        'rk3328_ddr_786MHz_v1.13.bin'
        'rk322xh_miniloader_v2.49.bin'
        'rk322xh_bl31_v1.40.elf'
        'rk322xh_bl32_v1.48.bin'
        '0001-configs-add-firefly-rk3328_defconfig.patch'
        '0002-firefly-rk3328-press-key-recovery-to-enter-the-rocku.patch'
        '0003-arm-dts-add-roc-rk3328-cc-support.patch'
        '0004-arm64-dts-fix-sdboot-bug.patch'
        '0005-arm-dts-set-usb-vbus-supply-use-uboot-dtb.patch'
        '0006-arm64-dts-fix-vcc_sd-voltage-reversal.patch'
        '0007-configs-rk3328-add-pinctrl-in-spl-dtb-make-savedefco.patch'
        '0008-firefly-rename-rk3328-roc-cc.dts-rk3328-firefly.dts-.patch'
        '0009-include-update-log2-header-from-the-Linux-kernel.patch'
        '0010-host-tools-use-python2-explicitly-for-shebang.patch'
        '0011-Revert-spl-fit-all-rockchip-based-soc-use-dram-as-sr.patch')
md5sums=('SKIP'
         '0f61f295eaeacd2c74da28defa41761b'
         'f1aa5fd08c19752d1de23989648730fa'
         '021623a04afd29ac3f368977140cfbfd'
         '04c908ca59f32f0501742c50360a608a'
         '0403a2f2a684b3699b44d2431a339fba'
         '652168bbd25603bbaf7fa38a75a3e6cd'
         '4de76e7b3a2b844dbf401f05dee96613'
         '86e5f7051f98b9a310d605860cbfb57e'
         '17b1707093ea45ed31e6a946e14d388f'
         '775a7228adb33a4e06569d137137be26'
         '665a92d657c25a02ee6b4618d43c728e'
         '99008624c45db9fecac894e04bf9651b'
         'cc02fb93ad0ce1416af98178488af9bd'
         'b0209b33b646a0bb5525d63bc20c66b9'
         'a1ceb5c011cf7ff331cb7a6397f9f8aa'
         '5b1244d3f9ea73d25e18dc6d72326f14'
         '1544c63e9d9cb823f17557b18ee25f29'
         '17d190627052ae3dacf9144fed65e6f5')

pkgver() {
  cd "${_pkgname}"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare() {
  cd "${_pkgname}"

  # add patches
  git am ../0001-configs-add-firefly-rk3328_defconfig.patch
  git am ../0002-firefly-rk3328-press-key-recovery-to-enter-the-rocku.patch
  git am ../0003-arm-dts-add-roc-rk3328-cc-support.patch
  git am ../0004-arm64-dts-fix-sdboot-bug.patch
  git am ../0005-arm-dts-set-usb-vbus-supply-use-uboot-dtb.patch
  git am ../0006-arm64-dts-fix-vcc_sd-voltage-reversal.patch
  git am ../0007-configs-rk3328-add-pinctrl-in-spl-dtb-make-savedefco.patch
  git am ../0008-firefly-rename-rk3328-roc-cc.dts-rk3328-firefly.dts-.patch
  git am ../0009-include-update-log2-header-from-the-Linux-kernel.patch
  git am ../0010-host-tools-use-python2-explicitly-for-shebang.patch
  git am ../0011-Revert-spl-fit-all-rockchip-based-soc-use-dram-as-sr.patch
}

build() {
  cd "${_pkgname}"

  unset CLFAGS CXXFLAGS CPPFLAGS LDFLAGS

  make firefly-rk3328_defconfig
  echo 'CONFIG_IDENT_STRING=" Arch Linux ARM"' >> .config
  make EXTRAVERSION=-${pkgrel}
}

package() {
  cd "${_pkgname}"

  mkdir -p "${pkgdir}/boot"

  tools/mkimage -n rk3328 -T rksd -d ../rk3328_ddr_786MHz_v1.13.bin "${pkgdir}/boot/idbloader.img"
  cat ../rk322xh_miniloader_v2.49.bin >> "${pkgdir}/boot/idbloader.img"

  loaderimage --pack --uboot u-boot-dtb.bin "${pkgdir}/boot/uboot.img" 0x200000

  trust_merger ../rk3328trust.ini

  cp u-boot-dtb.bin trust.img "${pkgdir}/boot"

  tools/mkimage -A arm -O linux -T script -C none -n "U-Boot boot script" -d ../boot.txt "${pkgdir}/boot/boot.scr"
  cp ../{boot.txt,mkscr} "${pkgdir}"/boot
}
