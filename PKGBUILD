# Maintainer: Eric Bélanger <eric@archlinux.org>

pkgname=lm_sensors
pkgver=3.4.0
pkgrel=2
pkgdesc="Collection of user space tools for general SMBus access and hardware monitoring"
arch=('i686' 'x86_64')
url="http://www.lm-sensors.org/"
license=('GPL' 'LGPL')
depends=('perl' 'sysfsutils')
makedepends=('rrdtool')
optdepends=('rrdtool: for logging with sensord')
backup=('etc/sensors3.conf' 'etc/healthd.conf' 'etc/conf.d/sensord')
source=($pkgname-$pkgver::https://github.com/groeck/lm-sensors/archive/V${pkgver//\./-}.tar.gz
	healthd healthd.conf healthd.service sensord.conf
        lm_sensors-fancontrol.patch)
sha1sums=('4a9026e4db894c98ee7cea0bec1188108e415f71'
          '1c91ae403d3cd02b6177ad1f1b2f2c3a7a3257f5'
          '1edd4d72ade22adfc128fb8d670e85c633fd1d18'
          'd72ec328e9303acef86342483b6f8537de6117d9'
          'f4b5f21fdb3b2a55aa353afa1603f953b207b73b'
          'b0bc977348610d6a008d75a43f65800251c4c9f7')
validpgpkeys=('7CA69F4460F1BDC41FD2C858A5526B9BB3CD4E6A')

prepare() {
  cd ${pkgname/_/-}-${pkgver//\./-}
  sed -i 's|/etc/sysconfig|/etc/conf.d|' prog/{detect/sensors-detect,init/{sensord,lm_sensors}.service}
  sed -i 's/EnvironmentFile=/EnvironmentFile=-/' prog/init/lm_sensors.service
  patch -p0 -i "${srcdir}/lm_sensors-fancontrol.patch"
}

build() {
  cd ${pkgname/_/-}-${pkgver//\./-}
  make PREFIX=/usr
}

package() {
  cd ${pkgname/_/-}-${pkgver//\./-}
  make PROG_EXTRA=sensord BUILD_STATIC_LIB=0 \
    PREFIX=/usr SBINDIR=/usr/bin MANDIR=/usr/share/man DESTDIR="${pkgdir}" install

  install -D -m755 "${srcdir}/healthd" "${pkgdir}/usr/bin/healthd"

  install -D -m644 "${srcdir}/healthd.conf" "${pkgdir}/etc/healthd.conf"
  install -D -m644 "${srcdir}/sensord.conf" "${pkgdir}/etc/conf.d/sensord"
 
  install -D -m644 "${srcdir}/healthd.service" "${pkgdir}/usr/lib/systemd/system/healthd.service"
  install -D -m644 prog/init/*.service "${pkgdir}/usr/lib/systemd/system/"
}
