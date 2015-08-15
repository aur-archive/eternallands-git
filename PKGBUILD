# Based on the eternallands package from aur
# Maintainer (yuan_modu): Atila Satilmis <atila.satilmis@gmail.com>

pkgname=eternallands-git
pkgver=r5722.3012402
pkgrel=1
pkgdesc="Eternal Lands is a FREE 3D fantasy MMORPG (Game core built from git)"
arch=('i686' 'x86_64')
license=('custom')
url="http://www.eternal-lands.com/"
depends=('sdl_net' 'sdl_image' 'openal' 'cal3d' 'libxml2' 'libvorbis' 'libgl' \
         'glu' 'eternallands-data')
makedepends=('git' 'mesa')
conflicts=('eternallands')
source=("${pkgname}::git+https://github.com/raduprv/Eternal-Lands.git"
        'eternallands.desktop')
md5sums=('SKIP'
         '4564fba195fc39fce438f717dde0ad9e')

pkgver() {
  cd "${srcdir}/${pkgname}"  
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare() {
  cd "${srcdir}/${pkgname}"
  if [ "${CARCH}" = "x86_64" ]; then
    sed -i "/PLATFORM=-march=/s|native|x86-64|" make.defaults
  else
    sed -i "/PLATFORM=-march=/s|native|i686|" make.defaults
  fi
  # Fix Xorg lib parth.
  sed -i "/XDIR=-L/s|/usr/X11R6/lib|/usr/lib/xorg|" make.defaults
  # DATA_DIR should be set.
  sed -i "s|OPTIONS = |OPTIONS = -DDATA_DIR="\\\\\"/usr/share/eternallands/"\\\\\" |" Makefile.linux
}

build() {
  cd "${srcdir}/${pkgname}"
  make -f Makefile.linux release
}

package() {
  cd "${srcdir}"  
  mkdir -p "${pkgdir}"/usr/{bin,share/{licenses/eternallands,applications,pixmaps}}
  install -m755 ${pkgname}/el.x86.linux.bin "${pkgdir}/usr/bin/el"
  install -m644 ${pkgname}/eternal_lands_license.txt \
    "${pkgdir}/usr/share/licenses/eternallands/LICENSE.source.txt"
  install -m644 ${pkgname}/elc.png "${pkgdir}/usr/share/pixmaps/eternallands.png"
  install -m644 eternallands.desktop "${pkgdir}/usr/share/applications/"
}
