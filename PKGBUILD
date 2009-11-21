# Maintainer: Eric Belanger <eric@archlinux.org>
# Contributor: Aurelien Foret <orelien@chez.com>

pkgname=lm_sensors
pkgver=3.1.1
pkgrel=3
pkgdesc="Collection of user space tools for general SMBus access and hardware monitoring"
arch=('i686' 'x86_64')
url="http://www.lm-sensors.org/"
license=('GPL')
depends=('perl' 'sysfsutils')
makedepends=('rrdtool')
optdepends=('rrdtool: for logging with sensord')
backup=('etc/sensors3.conf')
options=('!emptydirs')
install=sensors.install
source=(http://dl.lm-sensors.org/lm-sensors/releases/lm_sensors-${pkgver}.tar.bz2 \
	sensors.rc fancontrol.rc sensors-detect.patch healthd healthd.conf healthd.rc \
        sensord.conf sensord.rc)
md5sums=('613d7cfa23b70c0abae3fabb0a72ff5f' 'c370f5e620bfe41113354a1e22c0c18c'\
         'e54e3d0522f1705515d211edcdc3d0a9' '47c40b381d1f25d6634ae84cecf35f33'\
         '6549050897c237514aeaa2bb6cfd29ea' 'f649261f52bd4329347bf93f5f83cb0a'\
         '970408d2e509dc4138927020efefe323' '96a8dd468e81d455ec9b165bdf33e0b7'\
         '41a5c20854bbff00ea7174bd2276b736')
sha1sums=('8be15806d229305491f11b77c67496074480faf4' 'b2e664b9b87759991f02d0a1e8cac5e95098c0a5'\
         '8d3e4cc5b158b9c937062b180a94620b998527b6' '47095a32a918d6be50bd8daa8aaa9c24940d60e9'\
         '78b5cd36c3cb8e98b972cdd8c4a12687d79a79a8' 'c6ddfebc20685ba69700f66038c6b00a7c0bdb80'\
         'e662881f5d3f3f35a1bc97ba45d2c471dd28c37f' 'de8d4d65406815c389f8a04e2a8508a1ae6749c8'\
         '72a60251d1d55a67307dab4105d9f3f01a080af4')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  patch -p1 < ../sensors-detect.patch || return 1

  make PROG_EXTRA=sensord BUILD_STATIC_LIB=0 \
    PREFIX=/usr MANDIR=/usr/share/man DESTDIR="${pkgdir}" install || return 1

  install -D -m755 "${srcdir}/sensors.rc" "${pkgdir}/etc/rc.d/sensors" || return 1
  install -D -m755 "${srcdir}/fancontrol.rc" "${pkgdir}/etc/rc.d/fancontrol" || return 1
  install -D -m755 "${srcdir}/healthd" "${pkgdir}/usr/sbin/healthd" || return 1
  install -D -m755 "${srcdir}/healthd.rc" "${pkgdir}/etc/rc.d/healthd" || return 1
  install -D -m644 "${srcdir}/healthd.conf" "${pkgdir}/etc/conf.d/healthd" || return 1
  install -D -m755 "${srcdir}/sensord.rc" "${pkgdir}/etc/rc.d/sensord" || return 1
  install -D -m644 "${srcdir}/sensord.conf" "${pkgdir}/etc/conf.d/sensord" || return 1
}
