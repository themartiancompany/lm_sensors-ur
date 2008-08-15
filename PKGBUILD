# Maintainer: aurelien <aurelien@archlinux.org>
# Contributor: Aurelien Foret <orelien@chez.com>
pkgname=lm_sensors
pkgver=3.0.2
pkgrel=2
pkgdesc="Collection of user space tools for general SMBus access and hardware monitoring."
arch=('i686' 'x86_64')
license=('GPL')
depends=('perl' 'sysfsutils')
makedepends=('bison' 'flex' 'rrdtool')
optdepends=('rrdtool:  for logging with sensord')
backup=(etc/sensors3.conf)
install=sensors.install
source=(http://dl.lm-sensors.org/lm-sensors/releases/lm_sensors-$pkgver.tar.bz2
	sensors.rc
	fancontrol.rc
	sensors-detect.patch)
url="http://www.lm-sensors.org/"
md5sums=('5b210ba9cc01f00161c438fd618484e5'
         'c9f7f38964963ae3ced4dff3f1f0b7b9'
         'f14e335a8eea27388892c36af8099782'
         '6fd30ed1e5ac739b8a27f3913ba706f4')

build() {
  cd ${srcdir}/$pkgname-$pkgver
  patch -Np0 -i ${srcdir}/sensors-detect.patch || return 1

  make PREFIX=/usr PROG_EXTRA:=sensord user || return 1
  make user_install PREFIX=/usr DESTDIR=${pkgdir}
  install -DT -m755 ${srcdir}/$pkgname-$pkgver/prog/sensord/sensord ${pkgdir}/usr/sbin/sensord

  install -DT -m755 ${srcdir}/sensors.rc ${pkgdir}/etc/rc.d/sensors
  install -DT -m755 ${srcdir}/fancontrol.rc ${pkgdir}/etc/rc.d/fancontrol

  # remove the static lib
  rm -rf ${pkgdir}/usr/lib/libsensors.a

  # FIXME: avoid conflicts with glibc headers
  rm -rf ${pkgdir}/usr/include/linux
}
