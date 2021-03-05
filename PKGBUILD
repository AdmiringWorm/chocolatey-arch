# This is an example PKGBUILD file. Use this as a start to creating your own,
# and remove these comments. For more information, see 'man PKGBUILD'.
# NOTE: Please fill out the license field for your package! If it is unknown,
# then please put 'unknown'.

# Maintainer: Your Name <youremail@domain.com>
pkgname=choco
_pkgver=0.10.16-beta
pkgver=${_pkgver//-}
pkgrel=1
pkgdesc="Chocolatey - the package manager for Windows"
arch=(any)
url="https://chocolatey.org"
license=('Apache')
depends=('mono')
makedepends=('git')
provides=(choco chocolatey)
options=(!emptydirs)
source=("${pkgname}::git+https://github.com/chocolatey/choco#tag=${_pkgver}"
        0001-Fix-path-to-ls-executable.patch
	    0002-Don-t-replace-prefixed-slash-on-unix-systems.patch
	    0003-Use-correct-exclude-filer-on-unix.patch
        choco.sh)
sha512sums=('SKIP'
            'b86f1e652ea9f7b3f639536590901614d291adddf765e4e4d5f011e0da8eb1b42d05af4e5ea98ec2650d16d9d8d42884fd3e8b69350845d1ac5aa6a7177537f6'
            '8ec8e861669e07b2514e2583e573aaffe07a0b4b2eb25c73a470077945525781629dc723b4350d4e475fa92e653493b8fb4878679362be15ea5899f653161559'
            '0275ee67cf592879ffc7e87a03467bad1a4c34f49f27470623e551cdcf0b70a3c2f7d29113c6c963d003de45abe76e2f4aace7cee483bb29d0f4b4829baf2779'
            'cf9642b943335f535e6f1d692f2e85e9d68d8539b2e68747d06df84c918237afc0fba87400be82e0953fbcc3c0a054d7b614aa1293cb33ad0c1fa4f5524f84d0')

prepare() {
	cd "$pkgname"
	patch -p1 -i "$srcdir/0001-Fix-path-to-ls-executable.patch"
	patch -p1 -i "$srcdir/0002-Don-t-replace-prefixed-slash-on-unix-systems.patch"
	patch -p1 -i "$srcdir/0003-Use-correct-exclude-filer-on-unix.patch"
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
	install -Dm0644 "LICENSE" "$pkgdir/usr/share/licenses/${pkgname}/LICENSE"
	install -Dm0755 "$srcdir/choco.sh" "$pkgdir/usr/bin/choco"
}
