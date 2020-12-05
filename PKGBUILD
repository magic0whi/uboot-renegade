# U-Boot: ROC-RK3328-CC
# Maintainer: magic0whi <lollipop.studio.cn@gmail.com>

buildarch=8

pkgname=uboot-renegade
_pkgname=u-boot
pkgver=20201113
pkgrel=1
pkgdesc="U-Boot for ROC-RK3328-CC"
arch=('aarch64')
url='http://www.denx.de/wiki/U-Boot/WebHome'
license=('GPL')
backup=('boot/boot.txt' 'boot/boot.scr')
makedepends=('bc' 'git' 'rockchip-tools')
install=${pkgname}.install
source=("git+https://gitlab.denx.de/u-boot/custodians/u-boot-rockchip.git#branch=u-boot-rockchip-20201113"
        'rk3328trust.ini'
        'boot.txt'
        'mkscr'
        'rk3328_ddr_786MHz_v1.13.bin'
        'rk322xh_miniloader_v2.50.bin'
        'rk322xh_bl31_v1.40.elf'
        'rk322xh_bl32_v1.51.bin')
md5sums=('SKIP'
         'e6acdc8bf77951c07ca27ccf4f784d20'
         'f1aa5fd08c19752d1de23989648730fa'
         '021623a04afd29ac3f368977140cfbfd'
         '04c908ca59f32f0501742c50360a608a'
         'b2529b302637d3be478ce5612a397c99'
         '652168bbd25603bbaf7fa38a75a3e6cd'
         '673c9263cdf899d9d59327519a53934e')

build() {
  cd "${_pkgname}"

  unset CLFAGS CXXFLAGS CPPFLAGS LDFLAGS

  make roc-cc-rk3328_defconfig
  echo 'CONFIG_IDENT_STRING=" Arch Linux ARM"' >> .config
  make EXTRAVERSION=-${pkgrel}
}

package() {
  cd "${_pkgname}"

  mkdir -p "${pkgdir}/boot"

  tools/mkimage -n rk3328 -T rksd -d ../rk3328_ddr_786MHz_v1.13.bin "${pkgdir}/boot/idbloader.img"
  cat ../rk322xh_miniloader_v2.50.bin >> "${pkgdir}/boot/idbloader.img"

  loaderimage --pack --uboot u-boot-dtb.bin "${pkgdir}/boot/uboot.img" 0x200000

  trust_merger ../rk3328trust.ini

  cp u-boot-dtb.bin trust.img "${pkgdir}/boot"

  tools/mkimage -A arm -O linux -T script -C none -n "U-Boot boot script" -d ../boot.txt "${pkgdir}/boot/boot.scr"
  cp ../{boot.txt,mkscr} "${pkgdir}"/boot
}
