#!/hint/bash
# Maintainer: bartus <arch-user-repoᘓbartus.33mail.com>
# Contributor: Vekhir <vekhir at yahoo dot com>

## Configuration env vars:
((ENABLE_QT5)) && QT_VER="5" || QT_VER="6"
qt="qt${QT_VER}"

pkgname=openboard-git
_fragment="#branch=dev"
pkgver=1.7.1.r45.g844264f1
pkgrel=1
pkgdesc="Interactive whiteboard software for schools and universities (development version current dev)"
arch=('x86_64' 'i686')
url="http://openboard.ch/index.en.html"
license=('GPL3')
provides=("${pkgname%-git}=${pkgver%%.r*}")
conflicts=("${pkgname%-git}")
depends+=(${qt}-{base,declarative,multimedia,svg,webchannel,webengine})
depends+=('openssl' 'ffmpeg')
depends+=(quazip-${qt})  #drop internal quazip and use system one.
depends+=(poppler) #replace internal xpdf with poppler and drop freetype/xpdf from deps
makedepends=('git' 'cmake' ${qt}-tools)
source=("git+https://github.com/OpenBoard-org/OpenBoard.git${_fragment}")
source+=(30fps.patch)
sha256sums=('SKIP'
            '205062adbbd48d6622341e316e14a5496f73696385a3ed5cda7a89d3e7d2861d')

pkgver() {
  cd "$srcdir/OpenBoard"
  git describe --long --tags | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

build() {
  export CCACHE_BASEDIR="$srcdir"
  cmake -B build -S "$srcdir/OpenBoard" \
    -DCMAKE_BUILD_TYPE=None \
    -DQT_VERSION=${QT_VER} \
    -DCMAKE_INSTALL_PREFIX:PATH=/usr \
    -DCMAKE_CXX_STANDARD=20 \
    -Wno-dev
  cmake --build build
}

package() {
  DESTDIR="$pkgdir" cmake --install build
}
