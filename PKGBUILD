# Originally by: Michael Hansen <zrax0111 gmail com>

pkgname=vscode-oss-git
_pkgname=vscode
pkgdesc='Visual Studio Code for Linux'
pkgver=0.10.1
pkgrel=2
arch=('i686' 'x86_64')
url='https://code.visualstudio.com/'
license=('MIT')
makedepends=('npm' 'gulp')
depends=('gtk2' 'gconf')
conflicts=('vscode-bin' 'vscode-oss')

source=("git+https://github.com/Microsoft/vscode.git"
        vscode.desktop
        welcome.md
        vs_diff.patch)
sha1sums=('SKIP'
          '3187aeb37731109d25f3cd76752c0633b4e2ed02'
          'bc89c962078b0ed78987d4d5cd35e6c6fbf2dc3b'
          '0c137590b73e017c6e156025fbb43fd21cc6db70')

case "$CARCH" in
    i686)
        _vscode_arch=ia32
        ;;
    x86_64)
        _vscode_arch=x64
        ;;
    armv7h)
        _vscode_arch=arm
        ;;
    *)
        # Needed for mksrcinfo
        _vscode_arch=DUMMY
        ;;
esac


prepare() {
    cd "${srcdir}/${_pkgname}"
    patch -i ../vs_diff.patch
    sed -i 's/enableTelemetry: true/enableTelemetry: false/g' src/vs/platform/telemetry/browser/mainTelemetryService.ts
    sed -i 's/"electronVersion": "0.34.1"/"electronVersion": "0.35.2"/g' package.json
}

build() {
    cd "${srcdir}/${_pkgname}"
    ./scripts/npm.sh install
    gulp vscode-linux-${_vscode_arch}
}

package() {
    install -m 0755 -d "${pkgdir}/opt/VSCode"
    cp -r "${srcdir}/VSCode-linux-${_vscode_arch}"/* "${pkgdir}/opt/VSCode"

    # Include symlink in system bin directory
    install -m 0755 -d "${pkgdir}/usr/bin"
    ln -s '/opt/VSCode/Code [OSS Build]' "${pkgdir}/usr/bin/vscode"

    # Add .desktop file
    install -D -m644 "${srcdir}/vscode.desktop" \
            "${pkgdir}/usr/share/applications/vscode.desktop"

    # Add welcome.md
    install -D -m 644 "${srcdir}/welcome.md" \
            "${pkgdir}/opt/VSCode/resources/app/resources/welcome.md"

    # Install license file
    install -D -m644 "${srcdir}/VSCode-linux-${_vscode_arch}/resources/app/LICENSE.txt" \
            "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

