# Maintainer: Eric Belanger <eric@archlinux.org>
# Contributor: Aurelien Foret <orelien@chez.com>

pkgname=lm_sensors
pkgver=3.1.0
pkgrel=1
pkgdesc="Collection of user space tools for general SMBus access and hardware monitoring."
arch=('i686' 'x86_64')
url="http://www.lm-sensors.org/"
license=('GPL')
depends=('perl' 'sysfsutils')
makedepends=('bison' 'flex' 'rrdtool')
optdepends=('rrdtool: for logging with sensord')
backup=('etc/sensors3.conf')
install=sensors.install
source=(http://dl.lm-sensors.org/lm-sensors/releases/lm_sensors-${pkgver}.tar.bz2 \
	sensors.rc fancontrol.rc sensors-detect.patch)
md5sums=('e23130eaa46c66b861729835e08bdccf' 'c370f5e620bfe41113354a1e22c0c18c'\
         'f14e335a8eea27388892c36af8099782' '47c40b381d1f25d6634ae84cecf35f33')
sha1sums=('b36577ca07d3028910a93958e5809ba7b85e0bf8'
          'b2e664b9b87759991f02d0a1e8cac5e95098c0a5'
          '4a5c7b9114118f66e283a728d41b5fa7fe8b551d'
          '47095a32a918d6be50bd8daa8aaa9c24940d60e9')

build() {
  cd ${srcdir}/${pkgname}-${pkgver}
  patch -p1 < ../sensors-detect.patch || return 1

  make PREFIX=/usr PROG_EXTRA:=sensord user || return 1
  make user_install PREFIX=/usr MANDIR=/usr/share/man DESTDIR=${pkgdir} || return 1
  install -DT -m755 ${srcdir}/${pkgname}-${pkgver}/prog/sensord/sensord ${pkgdir}/usr/sbin/sensord || return 1

  install -DT -m755 ${srcdir}/sensors.rc ${pkgdir}/etc/rc.d/sensors || return 1
  install -DT -m755 ${srcdir}/fancontrol.rc ${pkgdir}/etc/rc.d/fancontrol || return 1

  # remove the static lib
  rm -rf ${pkgdir}/usr/lib/libsensors.a

  # FIXME: avoid conflicts with glibc headers
  rm -rf ${pkgdir}/usr/include/linux
}
