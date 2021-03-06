#!/hint/bash
# Maintainer: bartus <arch-user-repoᘓbartus.33mail.com>


pkgname=openboard-git
_fragment="#branch=master"
pkgver=1.6.1.r6.gdd19e752
pkgrel=1
pkgdesc="Interactive whiteboard software for schools and universities (development version current master)"
arch=('x86_64' 'i686')
url="http://openboard.ch/index.en.html"
license=('GPL3')
provides=("${pkgname%-git}=${pkgver%%.r*}")
conflicts=("${pkgname%-git}")
depends=('qt5-base' 'qt5-multimedia' 'qt5-svg' 'qt5-script' 'qt5-webkit' 'qt5-tools' 'qt5-xmlpatterns' 'libpaper' 'bzip2' 'openssl' 'libfdk-aac' 'sdl' 'ffmpeg')
depends+=(quazip)  #drop internal quazip and use system one.
depends+=(poppler) #replace internal xpdf with poppler and drop freetype/xpdf from deps
makedepends=(git)
source=("git://github.com/OpenBoard-org/OpenBoard.git${_fragment}"
        openboard.desktop)
source+=(qchar.patch)
source+=(quazip.patch)
source+=(drop_ThirdParty_repo.patch)
source+=(30fps.patch)
sha256sums=('SKIP'
            '00688af02006bddeab797f624e5cbae66a5c02f4e14315d87d3f198f74797c17'
            'b40fdab85f5921d0404c07db64628a2428a87d39193d2797bbef2e69b1d51549'
            '8c0b28ebd6cade0a551b695af9c04a2c45052f2d955357825b3be97bf00d5be7'
            'a6a9bc1f9c9bee0345b735fcf422245ae7946f96f6c34520dd63530a98978c14'
            '205062adbbd48d6622341e316e14a5496f73696385a3ed5cda7a89d3e7d2861d')

pkgver() {
  cd "$srcdir/OpenBoard"
  git describe --long --tags | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
  cd "$srcdir"/OpenBoard
  msg2 "drop_ThirdParty_repo"
  patch -p1 < "$srcdir"/drop_ThirdParty_repo.patch
  msg2 "qchar"
  patch -p1 < "$srcdir"/qchar.patch
  msg2 "quazip"
  patch -p1 < "$srcdir"/quazip.patch
  msg2 "gcc11"
  sed 's/_serialize/serialize/g' -i src/pdf-merger/Object.{h,cpp}
}

build() {
  cd "$srcdir"/OpenBoard
# convert translations to binary form
  lrelease OpenBoard.pro
  qmake OpenBoard.pro -spec linux-g++
  make
}

package() {
  cd "$srcdir"/OpenBoard

  install -Dm755 build/linux/release/product/OpenBoard -t "$pkgdir"/opt/openboard/
  cp -rp "$srcdir"/OpenBoard/resources/{customizations,etc,i18n,library} -t "$pkgdir"/opt/openboard/
  install -Dm644 "$srcdir"/OpenBoard/resources/images/OpenBoard.png -t "$pkgdir"/usr/share/icons/hicolor/64x64/apps/
  install -Dm644 "$srcdir"/openboard.desktop -t "$pkgdir"/usr/share/applications/
  install -dm755 "$pkgdir"/usr/bin/
  ln -s /opt/openboard/OpenBoard "$pkgdir"/usr/bin/openboard
}
