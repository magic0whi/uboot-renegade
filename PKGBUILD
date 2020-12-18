# U-Boot: ROC-RK3328-CC
# Maintainer: magic0whi <lollipop.studio.cn@gmail.com>

buildarch=8

pkgname=uboot-renegade
_pkgname=u-boot
pkgver=r50884.73d62b5c6b
pkgrel=1
pkgdesc="U-Boot for ROC-RK3328-CC"
arch=('aarch64')
url='http://www.denx.de/wiki/U-Boot/WebHome'
license=('GPL')
backup=('boot/boot.txt' 'boot/boot.scr')
makedepends=('bc' 'git' 'rockchip-tools' 'swig' 'python2')
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
        '0009-Revert-spl-fit-all-rockchip-based-soc-use-dram-as-sr.patch'
        '0010-scripts-dtc-Remove-redundant-YYLOC-global-declaratio.patch'
        '0011-rockchp-firefly-rk3328-enable-CONFIG_USB_DWC3.patch'
        '0012-Fix-invalid-DTS-includes.patch'
        '0013-firefly-amend-dwc3-nodes-to-keep-in-line-with-kernel.patch'
        '0014-host-tools-use-python2-explicitly-for-shebang.patch'
        '0015-pylibfdt-Convert-to-Python-2.patch'
        '0016-firefly-enable-sdram-common.patch')
md5sums=('SKIP'
         '0f61f295eaeacd2c74da28defa41761b'
         'f1aa5fd08c19752d1de23989648730fa'
         '021623a04afd29ac3f368977140cfbfd'
         '04c908ca59f32f0501742c50360a608a'
         '0403a2f2a684b3699b44d2431a339fba'
         '652168bbd25603bbaf7fa38a75a3e6cd'
         '4de76e7b3a2b844dbf401f05dee96613'
         'db4d855aacbe20273e6a74948be82def'
         'e0b865f4163cb5461ae826ed9176ddd7'
         'a71f25fc16f8cd8223ac5a667d4d75a7'
         '957e369fd1439f92a534de5cb9a37b59'
         '95943db7e6a223605eae9d7a286d5365'
         '301689e9c5852746af2f4cb2c56371fd'
         '2bd3ba52ff4c1f76835921c3d1c08af9'
         'e8b3128c36adc31688e59e432a44d36a'
         'd853a2882571c47f759e415e2455deb7'
         'b1cc5c1b5e64c535704565ee48308daf'
         'a280e03f20459ed9971b2f1eb73d67e2'
         'cd98ac79224c5d7d1209e602eac63cc3'
         'f1baefa73a0e4ca902a2cde4d9d9d174'
         '41a12b6a5500f276c08af2e02e8be33f'
         'd22c56611a1108f45039395b1776cf7d'
         'faaa68097f37bfa4a437cd74d2bd550b')

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
  git am ../0009-Revert-spl-fit-all-rockchip-based-soc-use-dram-as-sr.patch
  git am ../0010-scripts-dtc-Remove-redundant-YYLOC-global-declaratio.patch
  git am ../0011-rockchp-firefly-rk3328-enable-CONFIG_USB_DWC3.patch
  git am ../0012-Fix-invalid-DTS-includes.patch
  git am ../0013-firefly-amend-dwc3-nodes-to-keep-in-line-with-kernel.patch
  git am ../0014-host-tools-use-python2-explicitly-for-shebang.patch
  git am ../0015-pylibfdt-Convert-to-Python-2.patch
  git am ../0016-firefly-enable-sdram-common.patch
}

build() {
  cd "${_pkgname}"

  #unset CLFAGS CXXFLAGS CPPFLAGS LDFLAGS

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
