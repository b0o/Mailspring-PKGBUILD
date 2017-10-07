# Maintainer: Conor Anderson <conor@conr.ca>
pkgname=mailspring-bin
_pkgname=${pkgname%-bin}
pkgver=1.0.2
pkgvercommit="4eab3f85"
pkgrel=1
pkgdesc="The best email app for people and teams at work"
url='http://getmailspring.com/'
arch=('x86_64')
license=('GPL3')
depends=('alsa-lib' 'gconf' 'gtk2' 'gnome-keyring' 'libgnome-keyring' 'libxss' 'libxkbfile' 'libxtst' 'nss' 'pcre' 'python')
# https://mailspring-builds.s3.amazonaws.com/client/4eab3f85/linux/mailspring-1.0.2-amd64.deb
source=("${_pkgname}-${pkgver}-amd64.deb::https://mailspring-builds.s3.amazonaws.com/client/${pkgvercommit}/linux/${_pkgname}-${pkgver}-amd64.deb"
        "http://mirrors.kernel.org/ubuntu/pool/main/c/cyrus-sasl2/libsasl2-2_2.1.27~101-g0780600+dfsg-3ubuntu1_amd64.deb")
noextract=('libsasl2-2_2.1.27~101-g0780600+dfsg-3ubuntu1_amd64.deb')
sha256sums=('dcdc6c67006ee148e1db943dd6cd20e41a4846798071b5ab08b59c24637d97ef'
            'ede12253ba336c4ff8069999a07aa09fc9ea34d2f0272b76eb74ccf659871969')

package() {
  cd "$srcdir"
  tar xfJ data.tar.xz

  # Place files
  install -d "${pkgdir}/usr/lib/${_pkgname}"
  cp -a "${srcdir}/usr/share/${_pkgname}/"* "${pkgdir}/usr/lib/${_pkgname}"

  # Symlink main binary
  install -d "${pkgdir}/usr/bin"
  ln -s "/usr/lib/${_pkgname}/${_pkgname}" "${pkgdir}/usr/bin/${_pkgname}"

  # Place desktop entry and icons
  desktop-file-install -m 644 --dir "${pkgdir}/usr/share/applications/" "${srcdir}/usr/share/applications/${_pkgname}.desktop"
  install -dm755 "${pkgdir}/usr/share/icons/hicolor/"
  cp -R "${srcdir}/usr/share/icons/hicolor/"* "${pkgdir}/usr/share/icons/hicolor/"

  # Place license files
  for license in "LICENSE" "LICENSES.chromium.html"; do
    install -Dm644 "${pkgdir}/usr/lib/${_pkgname}/${license}" "${pkgdir}/usr/share/licenses/${_pkgname}/${license}"
    rm "${pkgdir}/usr/lib/${_pkgname}/${license}"
  done

  ##FIXME: Nasty workaround because we need an old version of libsasl2.so
  ar x libsasl2-2_2.1.27~101-g0780600+dfsg-3ubuntu1_amd64.deb
  tar xfJ data.tar.xz
  cp usr/lib/x86_64-linux-gnu/libsasl2.so.2.0.25 "${pkgdir}/usr/lib/libsasl2.so.2.0.25"
  ln -s "/usr/lib/libsasl2.so.2.0.25" "${pkgdir}/usr/lib/libsasl2.so.2"
}
