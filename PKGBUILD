# Maintainer: Eric Belanger <eric@archlinux.org>
# Contributor: Aurelien Foret <orelien@chez.com>

pkgname=lm_sensors
pkgver=3.1.1
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
	sensors.rc fancontrol.rc sensors-detect.patch healthd healthd.conf healthd.rc)
md5sums=('613d7cfa23b70c0abae3fabb0a72ff5f' 'c370f5e620bfe41113354a1e22c0c18c'\
         'f14e335a8eea27388892c36af8099782' '47c40b381d1f25d6634ae84cecf35f33'\
         '6415014dc77365a48525901f30fe99da' 'f649261f52bd4329347bf93f5f83cb0a'\
         '970408d2e509dc4138927020efefe323')
sha1sums=('8be15806d229305491f11b77c67496074480faf4'
          'b2e664b9b87759991f02d0a1e8cac5e95098c0a5'
          '4a5c7b9114118f66e283a728d41b5fa7fe8b551d'
          '47095a32a918d6be50bd8daa8aaa9c24940d60e9'
          '06128ebb689aa271eef916e14ae1f2c42bee1f1d'
          'c6ddfebc20685ba69700f66038c6b00a7c0bdb80'
          'e662881f5d3f3f35a1bc97ba45d2c471dd28c37f')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  patch -p1 < ../sensors-detect.patch || return 1

  make PREFIX=/usr PROG_EXTRA:=sensord || return 1
  make PREFIX=/usr MANDIR=/usr/share/man DESTDIR="${pkgdir}" install || return 1

  install -D -m755 prog/sensord/sensord "${pkgdir}/usr/sbin/sensord" || return 1
  install -D -m755 "${srcdir}/sensors.rc" "${pkgdir}/etc/rc.d/sensors" || return 1
  install -D -m755 "${srcdir}/fancontrol.rc" "${pkgdir}/etc/rc.d/fancontrol" || return 1
  install -D -m755 "${srcdir}/healthd" "${pkgdir}/usr/sbin/healthd" || return 1
  install -D -m755 "${srcdir}/healthd.rc" "${pkgdir}/etc/rc.d/healthd" || return 1
  install -D -m644 "${srcdir}/healthd.conf" "${pkgdir}/etc/conf.d/healthd" || return 1

  # remove the static lib
  rm -rf "${pkgdir}/usr/lib/libsensors.a"
  rmdir "${pkgdir}/etc/sensors.d"
}
