# Maintainer: Eric Bélanger <eric@archlinux.org>

pkgname=lm_sensors
pkgver=3.3.2
pkgrel=4
pkgdesc="Collection of user space tools for general SMBus access and hardware monitoring"
arch=('i686' 'x86_64')
url="http://www.lm-sensors.org/"
license=('GPL' 'LGPL')
depends=('perl' 'sysfsutils')
makedepends=('rrdtool')
optdepends=('rrdtool: for logging with sensord')
backup=('etc/sensors3.conf' 'etc/healthd.conf' 'etc/conf.d/sensord')
options=('!emptydirs')
source=(http://dl.lm-sensors.org/lm-sensors/releases/lm_sensors-${pkgver}.tar.bz2{,.sig} \
	sensors.rc fancontrol.rc healthd healthd.conf healthd.rc sensord.conf \
        sensord.rc fancontrol.service sensord.service healthd.service \
        daemonarg.patch linux_3.0.patch)
sha1sums=('5d0f026ad763124e8c2ad733b6e1ad5e6473993d'
          'a486d9fb6c5b0aff4520f6312106c67f5163f1cf'
          'b2e664b9b87759991f02d0a1e8cac5e95098c0a5'
          'a068ac0a3115a6191a487e11422506baa922b40a'
          'afaad558d2ad4732aa53b69afa23ccf37bc67ab1'
          '6c4e8a2d89dd2fd3ca2f0f4f3b1230111e01b0fc'
          'de5a46afc39a77ae8721246a0223baf21a2d4c9c'
          'de8d4d65406815c389f8a04e2a8508a1ae6749c8'
          '72a60251d1d55a67307dab4105d9f3f01a080af4'
          '7a4a4d1442aeeba0ba8aefb742a3ef187b593f4c'
          'eff43b4882d25dae7dd0b33eb2e33b0836a5cc51'
          '9cc453eb0e9f9057227459cb07e74815383544bf'
          '34241388c4001bfb6e49b7e10da1217e29a258d6'
          '5662828085cdd981f0dc7cf8f79d3d6e2b72f50c')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  sed -i -e 's|/etc/sysconfig|/etc/conf.d|' \
         -e 's|/etc/init.d/lm_sensors|/etc/rc.d/sensors|' prog/{detect/sensors-detect,init/lm_sensors.service}
  sed -i 's@\(/bin/systemctl\|/lib/systemd/system\)@/usr\1@g' prog/detect/sensors-detect
  sed -i 's/EnvironmentFile=/EnvironmentFile=-/' prog/init/lm_sensors.service
  patch -p1 < ../daemonarg.patch
  patch -p0 < ../linux_3.0.patch
  make PREFIX=/usr
}

package() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  make PROG_EXTRA=sensord BUILD_STATIC_LIB=0 \
    PREFIX=/usr MANDIR=/usr/share/man DESTDIR="${pkgdir}" install
  install -D -m644 prog/init/lm_sensors.service "${pkgdir}/usr/lib/systemd/system/lm_sensors.service"
  install -D -m755 "${srcdir}/sensors.rc" "${pkgdir}/etc/rc.d/sensors"
  install -D -m755 "${srcdir}/fancontrol.rc" "${pkgdir}/etc/rc.d/fancontrol"
  install -D -m755 "${srcdir}/healthd" "${pkgdir}/usr/sbin/healthd"
  install -D -m755 "${srcdir}/healthd.rc" "${pkgdir}/etc/rc.d/healthd"
  install -D -m644 "${srcdir}/healthd.conf" "${pkgdir}/etc/healthd.conf"
  install -D -m755 "${srcdir}/sensord.rc" "${pkgdir}/etc/rc.d/sensord"
  install -D -m644 "${srcdir}/sensord.conf" "${pkgdir}/etc/conf.d/sensord"
  install -D -m644 "${srcdir}/fancontrol.service" "${pkgdir}/usr/lib/systemd/system/fancontrol.service"
  install -D -m644 "${srcdir}/sensord.service" "${pkgdir}/usr/lib/systemd/system/sensord.service"
  install -D -m644 "${srcdir}/healthd.service" "${pkgdir}/usr/lib/systemd/system/healthd.service"
}
