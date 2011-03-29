# Maintainer: Eric Bélanger <eric@archlinux.org>

pkgname=lm_sensors
pkgver=3.3.0
pkgrel=1
pkgdesc="Collection of user space tools for general SMBus access and hardware monitoring"
arch=('i686' 'x86_64')
url="http://www.lm-sensors.org/"
license=('GPL' 'LGPL')
depends=('perl' 'sysfsutils' 'rrdtool')
backup=('etc/sensors3.conf' 'etc/conf.d/healthd' 'etc/conf.d/sensord')
options=('!emptydirs')
source=(http://dl.lm-sensors.org/lm-sensors/releases/lm_sensors-${pkgver}.tar.bz2 \
	sensors.rc fancontrol.rc sensors-detect.patch healthd healthd.conf healthd.rc \
        sensord.conf sensord.rc daemonarg.patch)
md5sums=('5eb18d7531ead4f54f28a1133a606535'
         'c370f5e620bfe41113354a1e22c0c18c'
         '232bedf043dd5dedde82df1a399c682c'
         '47c40b381d1f25d6634ae84cecf35f33'
         '6549050897c237514aeaa2bb6cfd29ea'
         'f8af587038b0e2a89c441f7eeaa5e640'
         '970408d2e509dc4138927020efefe323'
         '96a8dd468e81d455ec9b165bdf33e0b7'
         '41a5c20854bbff00ea7174bd2276b736'
         '40c8eb16af8249a0f1d851fc1057ea15')
sha1sums=('16c13a186557164fa51459a02209b120c0335f96'
          'b2e664b9b87759991f02d0a1e8cac5e95098c0a5'
          'a068ac0a3115a6191a487e11422506baa922b40a'
          '47095a32a918d6be50bd8daa8aaa9c24940d60e9'
          '78b5cd36c3cb8e98b972cdd8c4a12687d79a79a8'
          '6c4e8a2d89dd2fd3ca2f0f4f3b1230111e01b0fc'
          'e662881f5d3f3f35a1bc97ba45d2c471dd28c37f'
          'de8d4d65406815c389f8a04e2a8508a1ae6749c8'
          '72a60251d1d55a67307dab4105d9f3f01a080af4'
          '34241388c4001bfb6e49b7e10da1217e29a258d6')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  patch -p1 < ../sensors-detect.patch
  patch -p1 < ../daemonarg.patch
  make PREFIX=/usr
}

package() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  make PROG_EXTRA=sensord BUILD_STATIC_LIB=0 \
    PREFIX=/usr MANDIR=/usr/share/man DESTDIR="${pkgdir}" install
  install -D -m755 "${srcdir}/sensors.rc" "${pkgdir}/etc/rc.d/sensors"
  install -D -m755 "${srcdir}/fancontrol.rc" "${pkgdir}/etc/rc.d/fancontrol"
  install -D -m755 "${srcdir}/healthd" "${pkgdir}/usr/sbin/healthd"
  install -D -m755 "${srcdir}/healthd.rc" "${pkgdir}/etc/rc.d/healthd"
  install -D -m644 "${srcdir}/healthd.conf" "${pkgdir}/etc/conf.d/healthd"
  install -D -m755 "${srcdir}/sensord.rc" "${pkgdir}/etc/rc.d/sensord"
  install -D -m644 "${srcdir}/sensord.conf" "${pkgdir}/etc/conf.d/sensord"
}
