# Maintainer: Eric Bélanger <eric@archlinux.org>

pkgname=lm_sensors
pkgver=3.3.3
pkgrel=2
pkgdesc="Collection of user space tools for general SMBus access and hardware monitoring"
arch=('i686' 'x86_64')
url="http://www.lm-sensors.org/"
license=('GPL' 'LGPL')
depends=('perl' 'sysfsutils')
makedepends=('rrdtool')
optdepends=('rrdtool: for logging with sensord')
backup=('etc/sensors3.conf' 'etc/healthd.conf')
options=('!emptydirs')
source=(http://dl.lm-sensors.org/lm-sensors/releases/lm_sensors-${pkgver}.tar.bz2{,.sig} \
	healthd healthd.conf fancontrol.service sensord.service healthd.service \
        linux_3.0.patch lm_sensors-fancontrol.patch)
sha1sums=('b55c06f425993e42f13553f204066c446da36fd3'
          '035a721f20e4ad568f4fdde2d7c25d906c192458'
          'afaad558d2ad4732aa53b69afa23ccf37bc67ab1'
          '6c4e8a2d89dd2fd3ca2f0f4f3b1230111e01b0fc'
          '7a4a4d1442aeeba0ba8aefb742a3ef187b593f4c'
          'eff43b4882d25dae7dd0b33eb2e33b0836a5cc51'
          'a7a20eb3c799d70287e6c7968a7ab42165925293'
          '5662828085cdd981f0dc7cf8f79d3d6e2b72f50c'
          'd3e419b4019451fb039ae3d3b8e0ec55121b9f17')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  sed -i 's|/etc/sysconfig|/etc/conf.d|' prog/{detect/sensors-detect,init/lm_sensors.service}
  sed -i 's@\(/bin/systemctl\|/lib/systemd/system\)@/usr\1@g' prog/detect/sensors-detect
  sed -i 's/EnvironmentFile=/EnvironmentFile=-/' prog/init/lm_sensors.service
  patch -p0 -i "${srcdir}/linux_3.0.patch"
  patch -p0 -i "${srcdir}/lm_sensors-fancontrol.patch"
  make PREFIX=/usr
}

package() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  make PROG_EXTRA=sensord BUILD_STATIC_LIB=0 \
    PREFIX=/usr MANDIR=/usr/share/man DESTDIR="${pkgdir}" install
  install -D -m644 prog/init/lm_sensors.service "${pkgdir}/usr/lib/systemd/system/lm_sensors.service"
  install -D -m755 "${srcdir}/healthd" "${pkgdir}/usr/sbin/healthd"
  install -D -m644 "${srcdir}/healthd.conf" "${pkgdir}/etc/healthd.conf"
  install -D -m644 "${srcdir}/fancontrol.service" "${pkgdir}/usr/lib/systemd/system/fancontrol.service"
  install -D -m644 "${srcdir}/sensord.service" "${pkgdir}/usr/lib/systemd/system/sensord.service"
  install -D -m644 "${srcdir}/healthd.service" "${pkgdir}/usr/lib/systemd/system/healthd.service"
}
