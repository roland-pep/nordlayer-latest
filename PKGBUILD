# Maintainer: Roland Kiraly <rolandgyulakiraly at outlook dot com>
# https://github.com/raverecursion/nordlayer-latest/tree/master
pkgname=nordlayer
pkgver=3.2.2
pkgrel=1
pkgdesc="Proprietary VPN client for Linux"
arch=('x86_64')
url="https://nordlayer.com"
license=('custom:commercial')
replaces=('nordvpnteams-bin')
conflicts=('nordvpnteams-bin')
depends=('bash' 'libgcrypt' 'libgpg-error' 'libcap' 'hicolor-icon-theme' 'gmp')
options=('!strip' '!emptydirs')
install=${pkgname}.install
source_x86_64=("https://downloads.nordlayer.com/linux/latest/debian/pool/main/nordlayer_${pkgver}_amd64.deb")
sha512sums_x86_64=('f076ae853fb87941d1bc8c7cd34c31d233343a86b1d6b123353c328fda3fd938b7e38741ba1399f63eb2cdda990ddf460fde25e035eb810ea0c3d7de7e5303b2')

prepare() {
    cd "${srcdir}"
    ar x "nordlayer_${pkgver}_amd64.deb"
    tar -xf data.tar.gz -C "${srcdir}" || tar -xf data.tar.xz -C "${srcdir}" || tar -xf data.tar.bz2 -C "${srcdir}" || tar -xf data.tar -C "${srcdir}"
}

package() {
    cd "${srcdir}"
    mkdir -p "${pkgdir}"

    # Copy all necessary directories and files
    cp -r etc "${pkgdir}/"
    cp -r usr "${pkgdir}/"
    cp -r var "${pkgdir}/"

    # Move /usr/sbin/nordlayerd to /usr/bin
    if [ -f "${pkgdir}/usr/sbin/nordlayerd" ]; then
        mkdir -p "${pkgdir}/usr/bin"
        mv "${pkgdir}/usr/sbin/nordlayerd" "${pkgdir}/usr/bin/nordlayerd"
        rm -rf "${pkgdir}/usr/sbin"
    fi

    # Fix the systemd service file to point to the correct executable path
    sed -i 's|ExecStart=/usr/sbin/nordlayerd|ExecStart=/usr/bin/nordlayerd|' "${pkgdir}/usr/lib/systemd/system/nordlayer.service"

    # Ensure the necessary directories exist and have the correct permissions
    install -d -m 0755 "${pkgdir}/run/nordlayer"
    chown -R nordlayer:nordlayer "${pkgdir}/run/nordlayer"

    # Set permissions for executable files
    chmod 755 "${pkgdir}/usr/bin/nordlayer"
    chmod 755 "${pkgdir}/usr/bin/nordlayerd"
    chmod 755 "${pkgdir}/usr/libexec/nordlayer/nordlayer-charon"
    chmod 755 "${pkgdir}/usr/libexec/nordlayer/nordlayer-ip"
    chmod 755 "${pkgdir}/usr/libexec/nordlayer/nordlayer-openvpn"
    chmod 755 "${pkgdir}/usr/libexec/nordlayer/nordlayer-resolvconf"
    chmod 755 "${pkgdir}/usr/libexec/nordlayer/nordlayer-setcap"

    # Log the final contents for debugging
    echo "Final contents in ${pkgdir}:"
    find "${pkgdir}"
}
