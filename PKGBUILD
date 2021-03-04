# This is an example PKGBUILD file. Use this as a start to creating your own,
# and remove these comments. For more information, see 'man PKGBUILD'.
# NOTE: Please fill out the license field for your package! If it is unknown,
# then please put 'unknown'.

# Maintainer: Your Name <youremail@domain.com>
pkgname=choco
pkgver=0.10.15
pkgrel=1
pkgdesc="Chocolatey - the package manager for Windows"
arch=(any)
url="https://chocolatey.org"
license=('Apache')
depends=('mono')
makedepends=('git' 'mono-msbuild')
provides=(choco chocolatey)
options=(!emptydirs)
source=("${pkgname}::git+https://github.com/chocolatey/choco#tag=${pkgver}"
        "0001-Arch-Linux-workaround-patch.patch"
		"choco.sh")
sha512sums=('SKIP'
            '210830b9903e96361b3d883e888e040fa2be24739f2d5e4b07290f8330f7919826ebada5755c8b86c38a658890b45650b508cb4fc78a669f0b9969110248737c'
            '493bbd882ef3f69f01a27840417cfab560a2bf1b633808503ccdbcce5306a2a1e242511066d86a7a9b3b40d838b56ac99aa7e9add6245e5f3b9be3b08ee48f37')

prepare() {
	cd "$pkgname"
	patch -p1 -i "$srcdir/0001-Arch-Linux-workaround-patch.patch"
}

build() {
	cd "$pkgname"
	./build.sh
}

package() {
	cd "$pkgname"
	dest="$pkgdir/opt/$pkgname"
	install -d "$dest/lib"
	cp -aT "build_output/chocolatey/_PublishedApplications/chocolatey.console" "$dest"
	mono "$dest/choco.exe" --allow-unofficial unpackself || echo "Unpacked Chocolatey" # Chocolatey will fail on this step
	install -Dm0644 "$dest/LICENSE.txt" "$pkgdir/usr/share/licenses/${pkgname}/LICENSE"
	install -Dm0755 "$srcdir/choco.sh" "$pkgdir/usr/bin/choco"
}
