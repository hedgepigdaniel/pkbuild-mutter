# $Id$
# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Michael Kanis <mkanis_at_gmx_dot_de>

pkgname=mutter
pkgver=3.26.1+25+gb7fc6480d
pkgrel=1
pkgdesc="A window manager for GNOME"
url="https://git.gnome.org/browse/mutter"
arch=(i686 x86_64)
license=(GPL)
depends=(dconf gobject-introspection-runtime gsettings-desktop-schemas
         libcanberra startup-notification zenity libsm gnome-desktop upower
         libxkbcommon-x11 gnome-settings-daemon libgudev libinput pipewire)
makedepends=(intltool gobject-introspection git gnome-common)
groups=(gnome)
options=(debug !strip !emptydirs)
_commit=b7fc6480dd659d98e046fd23095509991f6bbe9e  # gnome-3-26
source=("git+https://git.gnome.org/browse/mutter#commit=$_commit"
        startup-notification.patch
	monitor-race.patch)
sha256sums=('SKIP'
            '5a35ca4794fc361219658d9fae24a3ca21a365f2cb1901702961ac869c759366'
            '8e08a56bfe92a6c5cdb15f5b2d04617e602544bd3bfda8ad297fd91fb8076b06')

pkgver() {
  cd $pkgname
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd $pkgname

  # https://bugs.archlinux.org/task/51940
  patch -Np1 -i ../startup-notification.patch
  patch -Np1 -i ../monitor-race.patch

  NOCONFIGURE=1 ./autogen.sh
}

build() {
  cd $pkgname

  ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var \
      --libexecdir=/usr/lib/$pkgname --disable-static \
      --disable-schemas-compile --enable-compile-warnings=minimum \
      --enable-gtk-doc --enable-egl-device --enable-remote-desktop

  #https://bugzilla.gnome.org/show_bug.cgi?id=655517
  sed -e 's/ -shared / -Wl,-O1,--as-needed\0/g' \
      -i {.,cogl,clutter}/libtool

  make
}

package() {
  cd $pkgname
  make DESTDIR="$pkgdir" install
}
