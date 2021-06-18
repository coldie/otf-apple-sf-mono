# Maintainer: Jeffrey Thompson <coldie@coldie.net>

pkgname=('otf-apple-sf-mono')
pkgver=1
pkgrel=1
pkgdesc="Monospaced variant of Apple's San Francisco font"
url=https://developer.apple.com/fonts/
arch=('any')
license=('custom')
makedepends=('p7zip')
sha256sums=('ec0518e310797d2f9cb924c18e3e7b661359f4fb653d1ad4315758ebcdb5ff11')

source=("$pkgname-$pkgver.dmg::https://developer.apple.com/design/downloads/SF-Mono.dmg")
noextract=("${source[@]%%::*}")

prepare() {
	mkdir -p "$pkgname-$pkgver"

	7z e -y -so "$pkgname-$pkgver.dmg" '-i!*/*.pkg' > $pkgname.pkg
	bsdtar -xOf $pkgname.pkg 'Resources/English.lproj/license.rtf' | xz > "$pkgname-$pkgver/LICENSE.rtf.xz"
	bsdtar -xOf $pkgname.pkg 'SFMonoFonts.pkg/Payload' > $pkgname.cpio
	bsdtar -C . -s ',.*/,,g' -xzf $pkgname.cpio \*.otf
		
	7z e -y -r "-o./$pkgname-$pkgver/" '-i!*.otf' "$pkgname.cpio"
	mv *.otf $pkgname-$pkgver

	rm -f "$pkgname.pkg"
	rm -f "$pkgname.cpio"
}

package() {
	install -Dm644 $pkgname-$pkgver/LICENSE.rtf.xz "$pkgdir/usr/share/licenses/$pkgname/LICENSE.rtf.xz"
	install -d "$pkgdir/usr/share/fonts/OTF"
	install -m644 $pkgname-$pkgver/*.otf "$pkgdir/usr/share/fonts/OTF"
}
