# https://github.com/archlinux/svntogit-packages/raw/packages/webkit2gtk/trunk/PKGBUILD
# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Eric Bélanger <eric@archlinux.org>

pkgname=webkit2gtk
pkgver=2.34.1
pkgrel=1
pkgdesc="Web content engine for GTK"
url="https://webkitgtk.org"
groups=('modified')
arch=(x86_64)
license=(custom)
depends=(cairo fontconfig freetype2 libgcrypt glib2 gtk3 harfbuzz harfbuzz-icu
         icu libjpeg libsoup libxml2 zlib libpng sqlite atk libwebp at-spi2-core
         libegl libgl libgles libwpe wpebackend-fdo libxslt libsecret libtasn1
         enchant libx11 libxext libice libxt wayland libnotify hyphen openjpeg2
         woff2 libsystemd bubblewrap libseccomp xdg-dbus-proxy gstreamer
         gst-plugins-base-libs libmanette)
makedepends=(cmake ninja gtk-doc python ruby gobject-introspection
             wayland-protocols systemd gst-plugins-bad gperf)
optdepends=('geoclue: Geolocation support'
            'gst-plugins-good: media decoding'
            'gst-plugins-bad: media decoding'
            'gst-libav: nonfree media decoding')
source=($url/releases/webkitgtk-$pkgver.tar.xz{,.asc}
  PasteboardGtk.cpp.patch
WebKitCompilerFlags.cmake.patch)
sha256sums=('443c1316705de024741748e85fe32324d299d9ee68e6feb340b89e4a04073dee'
            'SKIP'
	    '2bef06563212e116a78ec47665e4baec94bef44029e1f0e20f115bc65de5567b'
        '9b8e2f0f4e07b164adfb7212097c258692a14bf84539ebeae0593a572149db18')
validpgpkeys=('D7FCF61CF9A2DEAB31D81BD3F3D322D0EC4582C3'  # Carlos Garcia Campos <cgarcia@igalia.com>
              '5AA3BC334FD7E3369E7C77B291C559DBE4C9123B') # Adrián Pérez de Castro <aperez@igalia.com>

prepare() {
  cd webkitgtk-$pkgver
  patch --forward --strip=1 --input="${srcdir}/PasteboardGtk.cpp.patch"
  patch --forward --strip=1 --input="${srcdir}/WebKitCompilerFlags.cmake.patch"
}

build() {
  cmake -S webkitgtk-$pkgver -B build -G Ninja \
    -DPORT=GTK \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCMAKE_INSTALL_LIBDIR=lib \
    -DCMAKE_INSTALL_LIBEXECDIR=lib \
    -DCMAKE_SKIP_RPATH=ON \
    -DUSE_SOUP2=ON \
    -DENABLE_GTKDOC=ON \
    -DENABLE_MINIBROWSER=ON
  cmake --build build
}

package() {
  depends+=(libwpe-1.0.so libWPEBackend-fdo-1.0.so)
  provides+=(libjavascriptcoregtk-4.0.so libwebkit2gtk-4.0.so)

  DESTDIR="$pkgdir" cmake --install build

  cd webkitgtk-$pkgver
  find Source -name 'COPYING*' -or -name 'LICENSE*' -print0 | sort -z |
    while IFS= read -d $'\0' -r _f; do
      echo "### $_f ###"
      cat "$_f"
      echo
    done |
    install -Dm644 /dev/stdin "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}

# vim:set sw=2 et:
