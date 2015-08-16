# Maintainer: Tom Bu <tom.bu@openmailbox.org>
# Based on the PKGBUILD of makemkv by Olaf Bauer <hydro@freenet.de>

pkgname=makemkv-static-avcodec
_pkgname=makemkv
pkgver=1.8.13
pkgrel=4
_ffname=libav
_ffver=11
pkgdesc="DVD and Blu-ray to MKV converter and network streamer, with a static libavcodec to enable AAC encoder and 24 bit FLAC" # http://www.makemkv.com/forum2/viewtopic.php?f=3&t=224
arch=('i686' 'x86_64')
url="http://www.makemkv.com"
license=('LGPL' 'MPL' 'custom')
provides='makemkv'
conflicts='makemkv'
depends=('qt4' 'icu')
makedepends=('libfdk-aac')
if [ "$CARCH" = "x86_64" ]; then
  optdepends=('lib32-glibc: dts support')
fi
install=makemkv.install
source=(${url}/download/${_pkgname}-bin-${pkgver}.tar.gz
        ${url}/download/${_pkgname}-oss-${pkgver}.tar.gz
	http://www.libav.org/releases/${_ffname}-${_ffver}.tar.xz
        makemkv.1
        makemkvcon.1
        mmdtsdec.1)
md5sums=('1ae9d9e4c8d2b9d91474446276e6b01b'
         'd840efb7a0b240d08f8dd3156107940d'
         'bfc894b3a2747212c2f48a38a468a15e'
         '1f9b3a91427a2015434e501542443f4c'
         '7f4b112c5178860cc2eb25059ae1af2a'
         '9476154228bf1b1f983178ba8565ac44')

build() {
  cd $srcdir/${_ffname}-${_ffver}
  ./configure --prefix=/tmp/libav --enable-static --disable-shared --enable-pic --disable-yasm --enable-nonfree --enable-libfdk-aac
  make
  make install
  cd $srcdir/${_pkgname}-oss-${pkgver}
  PKG_CONFIG_PATH=/tmp/libav/lib/pkgconfig ./configure --prefix=/usr
  make
}

package() {
  cd $srcdir/${_pkgname}-oss-${pkgver}
  make DESTDIR="$pkgdir" install

  cd $srcdir/${_pkgname}-bin-${pkgver}
  install -d $pkgdir/usr/bin/
  install -t $pkgdir/usr/bin/ bin/i386/mmdtsdec
  if [ "$CARCH" = "x86_64" ]; then
    install -t $pkgdir/usr/bin/ bin/amd64/makemkvcon
  else
    install -t $pkgdir/usr/bin/ bin/i386/makemkvcon
  fi
  install -d $pkgdir/usr/share/MakeMKV
  install -t $pkgdir/usr/share/MakeMKV src/share/makemkv_*.mo.gz src/share/*.mmcp.xml
  
  install -Dm 644 src/eula_en_linux.txt $pkgdir/usr/share/licenses/${_pkgname}/eula_en_linux.txt

  cd $srcdir/
  install -d $pkgdir/usr/share/man/man1/
  install -m 644 -t $pkgdir/usr/share/man/man1/ makemkv.1 makemkvcon.1 mmdtsdec.1
  rm -rf /tmp/libav
}
