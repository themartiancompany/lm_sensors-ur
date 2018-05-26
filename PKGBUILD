# Maintainer: Eric Bélanger <eric@archlinux.org>

pkgname=lm_sensors
pkgver=3.4.0
pkgrel=4
pkgdesc="Collection of user space tools for general SMBus access and hardware monitoring"
arch=('x86_64')
url="https://hwmon.wiki.kernel.org/lm_sensors"
license=('GPL' 'LGPL')
depends=('perl')
makedepends=('rrdtool')
optdepends=('rrdtool: for logging with sensord')
backup=('etc/sensors3.conf' 'etc/healthd.conf' 'etc/conf.d/sensord')
source=($pkgname-$pkgver::https://github.com/groeck/lm-sensors/archive/V${pkgver//\./-}.tar.gz
	healthd healthd.conf healthd.service sensord.conf)
sha256sums=('e334c1c2b06f7290e3e66bdae330a5d36054701ffd47a5dde7a06f9a7402cb4e'
            '0ac9afb2a9155dd74ab393756ed552cd542dde1081149beb2ab4ec7ff55b8f4a'
            '5d17a366b175cf9cb4bb0115c030d4b8d91231546f713784a74935b6e533da9f'
            '2638cd363e60f8d36bcac468f414a6ba29a1b5599f40fc651ca953858c8429d7'
            '23bebef4c250f8c0aaba2c75fd3d2c8ee9473cc91a342161a9f5b3a34ddfa9e5')
validpgpkeys=('7CA69F4460F1BDC41FD2C858A5526B9BB3CD4E6A')

prepare() {
  cd ${pkgname/_/-}-${pkgver//\./-}
  sed -i 's|/etc/sysconfig|/etc/conf.d|' prog/{detect/sensors-detect,init/{sensord,lm_sensors}.service}
  sed -i 's/EnvironmentFile=/EnvironmentFile=-/' prog/init/lm_sensors.service
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
