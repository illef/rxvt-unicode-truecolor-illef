# Maintainer: SergT <jobts[at]ya[dot]ru>
# Contributor: Levente Polyak <anthraxx[at]archlinux[dot]org>
# Contributor: Sébastien Luttringer
# Contributor: Angel Velasquez <angvp@archlinux.org>
# Contributor: tobias <tobias@archlinux.org>
# Contributor: dibblethewrecker dibblethewrecker.at.jiwe.dot.org

_pkgname=rxvt-unicode
pkgname=rxvt-unicode-truecolor-illef
pkgver=9.22
pkgrel=7
pkgdesc='Unicode enabled rxvt-clone terminal emulator (urxvt) with true color patch (24bit colors support). That enables vim 24-bit true color colorshemes'
arch=('x86_64')
url='http://software.schmorp.de/pkg/rxvt-unicode.html'
license=('GPL')
conflicts=('rxvt-unicode')
provides=('rxvt-unicode')
depends=('rxvt-unicode-terminfo' 'libxft' 'perl' 'startup-notification' 'libnsl')
optdepends=('gtk2-perl: to use the urxvt-tabbed')
makedepends=('libxft' 'perl' 'startup-notification' 'libnsl')
source=(http://dist.schmorp.de/rxvt-unicode/${_pkgname}-${pkgver}.tar.bz2
        urxvt.desktop
        urxvtc.desktop
        urxvt-tabbed.desktop
        https://gist.githubusercontent.com/illef/98955ba6a50b1462d30d5638abe45f58/raw/0f0916cdb8f21afe0f284da79557df821d5a8559/24-bit-color.patch
       )
sha256sums=('e94628e9bcfa0adb1115d83649f898d6edb4baced44f5d5b769c2eeb8b95addd'
            '5f9c435d559371216d1c5b49c6ec44bfdb786b12d925d543c286b0764dea0319'
            '91536bb27c6504d6cb0d33775a0c4709a4b439670b900f0c278c25037f19ad66'
            'ccd7c436e959bdc9ab4f15801a67c695b382565b31d8c352254362e67412afcb'
            '9ce7459fcc00423f6b21bfffb77aee400cd8e31b088a10adebc27e6e6fbfb538')
sha512sums=('b39f1b2cbe6dd3fbd2a0ad6a9d391a2b6f49d7c5e67bc65fe44a9c86937f8db379572c67564c6e21ff6e09b447cdfd4e540544e486179e94da0e0db679c04dd9'
            '7184714a908071a4e8e5c065c5f90255e94dfd072df459c8d6f66fca3647781b3d1f6908b9303bcfd0d5b3f2e3822a8d66efaaa8a7c4d44f6e682839031a6e99'
            'aa501eeeb220ba03b3f101b160230612efbca87694fef88c469b2976d29769c24b34576ea82f6c7941fad6039ac776f32e397add9b957b49bf2e84aeb67b66d6'
            '18c7afb0c3eb8c832893b9ead09d25374b70ae1cd5479a5291d11794906c53daa6f1a1bf698b37efda062bb2b991cacac53a0a6c185ca416b8718fde2bb6a7af'
            'eb4c5e5bc4672082c4528ae2153b0dd34e9da6117bf658e1aec1c46d64df81e7cd4f8dc15fd5bc0daa740428c970b2dfec8f2c6d222211ef09ac1c977393970c')

prepare() {
  cd ${_pkgname}-${pkgver}
  patch -p0 < ../24-bit-color.patch
}

build() {
  cd ${_pkgname}-${pkgver}
  # we disable smart-resize (FS#34807)
  # do not specify --with-terminfo (FS#46424)
  ./configure \
    --prefix=/usr \
    --enable-256-color \
    --enable-combining \
    --enable-fading \
    --enable-font-styles \
    --enable-iso14755 \
    --enable-keepscrolling \
    --enable-lastlog \
    --enable-mousewheel \
    --enable-next-scroll \
    --enable-perl \
    --enable-pointer-blank \
    --enable-rxvt-scroll \
    --enable-selectionscrolling \
    --enable-slipwheeling \
    --disable-smart-resize \
    --enable-startup-notification \
    --enable-transparency \
    --enable-unicode3 \
    --enable-utmp \
    --enable-wtmp \
    --enable-xft \
    --enable-xim \
    --enable-xterm-scroll \
    --disable-pixbuf \
    --disable-frills \
    --enable-24-bit-color
  make
}

package() {
  # install freedesktop menu
  for _f in urxvt urxvtc urxvt-tabbed; do
    install -Dm 644 ${_f}.desktop "${pkgdir}/usr/share/applications/${_f}.desktop"
  done

  cd ${_pkgname}-${pkgver}
  # workaround terminfo installation
  export TERMINFO="${srcdir}/terminfo"
  install -d "${TERMINFO}"
  make DESTDIR="${pkgdir}" install
  # install the tabbing wrapper ( requires gtk2-perl! )
  sed -i 's/\"rxvt\"/"urxvt"/' doc/rxvt-tabbed
  install -Dm 755 doc/rxvt-tabbed "${pkgdir}/usr/bin/urxvt-tabbed"
}


# vim: ts=2 sw=2 et:
