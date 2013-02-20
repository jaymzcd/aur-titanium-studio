# Maintainer: Jaymz Campbell <jaymz@jaymz.eu>
# Prior Maintainer: Sergio Correia <sergio.correia@uece.net>
# Contributor: Shanto <shanto@hotmail.com>

pkgname=titanium-studio
pkgver=3.0.2.201302191606
pkgrel=1
pkgdesc="A free and open source application development platform, Titanium lets you create native mobile, tablet and desktop application experiences using existing web skills like Javascript, HTML, CSS, Python, Ruby, and PHP."
url="http://www.appcelerator.com/products/titanium-studio/"
arch=('i686' 'x86_64')
license=('Apache')
depends=('java-environment>=6' 'gtk2>=2.20' 'webkitgtk2' 'libpng12' 'libjpeg6' 'nodejs' 'sudo')
makedepends=('findutils')
options=(!strip)
install=${pkgname}.install

source=(
	"${pkgname}.desktop"
	"${pkgname}"
	"titanium-studio-new-namespace.c"
)

md5sums=(
	'71ea5cc8c1eefdf2c394655add2bef42'
	'2c1d98f11846ac0c7c4989265ddee4cf'
	'7f3c4e54696bca03912e3a8820a1e78d'
)

# to give us the size of the array; we are gonna add the last entry based on the architecture
_pkgarraysize=${#source[@]}

if [ "$CARCH" = "i686" ]; then
	source[${_pkgarraysize}]="http://titanium-studio.s3.amazonaws.com/latest/titanium.linux.gtk.x86.zip"
	md5sums[${_pkgarraysize}]='760a4282d1a3eb01c4492e0e4544e94d'
else
	# x86_64
	source[${_pkgarraysize}]="http://titanium-studio.s3.amazonaws.com/latest/titanium.linux.gtk.x86_64.zip"
	md5sums[${_pkgarraysize}]='47d6f90cc4de3004ed0e0f04b5b5ae20'
fi


build() {
	_pkgroot="usr/share/${pkgname}"

	cd "${srcdir}"
	gcc "${srcdir}/titanium-studio-new-namespace.c" -o titanium-studio-new-namespace
}


package() {
	mkdir -p $pkgdir/usr/{bin,share/{applications,pixmaps,icons/hicolor/{16x16,32x32}/apps}}

	_pkgroot="usr/share/${pkgname}"
	_unpackedpkg=Titanium_Studio

	cp -r "${srcdir}/${_unpackedpkg}" "$pkgdir/${_pkgroot}"
	cp "${srcdir}/${pkgname}" "${pkgdir}/usr/bin"
	cp "${srcdir}/titanium-studio-new-namespace" "${pkgdir}/${_pkgroot}"

	cd "${pkgdir}/${_pkgroot}"

	sed -i 's|/usr/bin/readlink|readlink|' TitaniumStudio

	install -m644 "${srcdir}/${pkgname}.desktop" "../applications/"
	ln -sfT "/${_pkgroot}/icon.xpm" "../pixmaps/${pkgname}.xpm"

	cd $pkgdir/${_pkgroot}
	find ./ -path '*titanium*/window16*.png' -printf "/${_pkgroot}/%P" -quit | xargs -i ln -sf "{}" "../icons/hicolor/16x16/apps/${pkgname}.png"
	find ./ -path '*titanium*/window32*.png' -printf "/${_pkgroot}/%P" -quit | xargs -i ln -sf "{}" "../icons/hicolor/32x32/apps/${pkgname}.png"

	# fix directories permissions
	find ./ -type d -exec chmod 755 {} \;

	# remove problematic rubles
	rm -rf "${pkgdir}/usr/share/titanium-studio/plugins/com.appcelerator.titanium.core_2.2.0.1346185493/bundles"
}
