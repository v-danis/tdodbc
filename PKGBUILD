pkgname=tdodbc
pkgver=20.00.00.19
_pkgver="$(echo $pkgver | cut -d. -f1,2)"
_pkgpfx="${pkgname}$(echo ${_pkgver} | tr -d '.')"
pkgrel=1
pkgdesc="Teradata ODBC driver"
arch=('x86_64')
url="http://downloads.teradata.com/download/connectivity/odbc-driver/linux"
license=('Proprietary')
depends=('unixodbc')

source=("${_pkgpfx}__ubuntu_${arch}.${pkgver}-1.tar.gz")
md5sums=('b0c297ca08610170aa379a3e422ed3cc')

package() {
  ar xv "${srcdir}/${_pkgpfx}/${_pkgpfx}-${pkgver}-1.${arch}.deb"
  tar -xvaf ${srcdir}/data.tar.xz
  cp -r "${srcdir}/opt" "${srcdir}/usr" "${pkgdir}/"

  rm -rf "${pkgdir}/opt/teradata/client/${_pkgver}/lib" "${pkgdir}/opt/teradata/client/${_pkgver}/bin/tdxodbc"

  sed -i "s+lib64+odbc_64/lib+g" "${pkgdir}/opt/teradata/client/${_pkgver}/odbc_64/"odbc*.ini
  install -dm755 "${pkgdir}/opt/teradata/client/ODBC_64"
  install -m644 "${pkgdir}/opt/teradata/client/${_pkgver}/odbc_64/odbc.ini" "${pkgdir}/opt/teradata/client/${_pkgver}/odbc_64/odbcinst.ini" "${pkgdir}/opt/teradata/client/ODBC_64/"
  sed -i -e "s+${_pkgver}/odbc_64+ODBC_64+g" -e "s+client/${_pkgver}+client/ODBC_64+g" "${pkgdir}/opt/teradata/client/ODBC_64/"odbc*.ini

  ln -s /opt/teradata/client/${_pkgver}/lib64                  ${pkgdir}/opt/teradata/client/${_pkgver}/odbc_64/lib
  ln -s /opt/teradata/client/${_pkgver}/include                ${pkgdir}/opt/teradata/client/${_pkgver}/odbc_64/include

  ln -s /opt/teradata/client/${_pkgver}/lib64                  ${pkgdir}/opt/teradata/client/ODBC_64/lib
  ln -s /opt/teradata/client/${_pkgver}/odbc_64/locale         ${pkgdir}/opt/teradata/client/ODBC_64/locale
  ln -s /opt/teradata/client/${_pkgver}/include                ${pkgdir}/opt/teradata/client/ODBC_64/include

  # Don't install DataDirect drivers because they might conflict with unixodbc
  # ln -s ${pkgdir}/opt/teradata/client/${_pkgver}/lib64/ddtrc27.so       ${pkgdir}/opt/teradata/client/${_pkgver}/lib64/odbctrac.so
  # ln -s ${pkgdir}/opt/teradata/client/${_pkgver}/lib64/libddicu27.so    $OSLIB64/libddicu27.so
  # ln -s ${pkgdir}/opt/teradata/client/${_pkgver}/lib64/libodbcinst.so   $OSLIB64/libodbcinst.so
  # ln -s ${pkgdir}/opt/teradata/client/${_pkgver}/lib64/libodbc.so       $OSLIB64/libodbc.so
  # ln -s ${pkgdir}/opt/teradata/client/${_pkgver}/lib64/odbccurs.so      $OSLIB64/odbccurs.so

  printf "../.." > ${pkgdir}/opt/teradata/client/${_pkgver}/lib64/tdclientdir
  chmod 755 ${pkgdir}/opt/teradata/client/${_pkgver}/lib64/tdclientdir
}
