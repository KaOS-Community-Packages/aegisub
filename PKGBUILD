pkgname=aegisub
pkgver=3.2.2
pkgrel=1
pkgdesc='A general-purpose subtitle editor with ASS/SSA support'
arch=('x86_64')
url='http://www.aegisub.org'
license=('GPL' 'BSD')
depends=('alsa-lib' 'boost-libs' 'fftw' 'fontconfig' 'gcc-libs' 'glibc'
         'hunspell' 'icu' 'libgl' 'pulseaudio' 'wxgtk' 'zlib'
         'libass' 'ffms2')
makedepends=('boost' 'intltool' 'mesa')
source=("http://ftp.aegisub.org/pub/archives/releases/source/aegisub-${pkgver}.tar.xz"
        'boost-1.68.patch'
        'icu59.patch')
sha256sums=('c55e33945b82d8513c02ea6e782f0d72c726adcd3707e95b8c0022f6151e6885'
            'aa1689a2204f6a617000f3380b8dea9c3dca4f500d0643e05172750c49cc5a21'
            '29d8cc91e73602d5e3c8517c977dcc77cb84af7d42787f510d487d0a6abf7fa6')

prepare() {
  cd aegisub-${pkgver}

  patch -Np1 -i ../boost-1.68.patch
  patch -Np1 -i ../icu59.patch

  sed 's/$(LIBS_BOOST) $(LIBS_ICU)/$(LIBS_BOOST) $(LIBS_ICU) -pthread/' -i tools/Makefile
}

build() {
  cd aegisub-${pkgver}

  # http://site.icu-project.org/download/61#TOC-Migration-Issues
  CPPFLAGS+=' -DU_USING_ICU_NAMESPACE=1'

  ./configure \
    --prefix='/usr' \
    --with-wx-config='/usr/bin/wx-config' \
    --without-{portaudio,openal,oss} \
    --disable-update-checker
  make
}

package() {
  cd aegisub-${pkgver}

  make DESTDIR="${pkgdir}" install

  install -dm 755 "${pkgdir}"/usr/share/licenses/aegisub
  install -m 644 LICENCE "${pkgdir}"/usr/share/licenses/aegisub/LICENSE
}
