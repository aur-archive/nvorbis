pkgname=nvorbis
pkgver=0.8.4
pkgrel=1
pkgdesc="NVorbis is a .Net library for decoding Xiph.org Vorbis files."
depends=(mono)
makedepends=(opentk dos2unix)
arch=(any)
license=("MSPL, public domain")
url="http://nvorbis.codeplex.com"
source=("nvorbis.zip::http://download-codeplex.sec.s-msft.com/Download/SourceControlFileDownload.ashx?ProjectName=nvorbis&changeSetId=cecae43dfdaad0c8e0b0eb7a2a85a4d11e60811e"
"nvorbis.pc" "nvorbis-opentk.pc")
optdepends=("opentk: OpenTK support")
sha1sums=('617499e45e4e99c052660046a1bb0cdd4f652678'
          '3d76ed8e3668477b647dddf208f5cfd0304f97e7'
          '1c3315ec24281a153f2f89749398373d20c82a8b')

prepare() {
	cd "$srcdir"
	find . -type f -exec dos2unix {} \;
}
          
build() {
	cd "$srcdir/NVorbis"
	xbuild NVorbis.csproj /p:Configuration=Release
	cd ../OpenTKSupport
	mcs /debug:pdbonly /optimize+ /out:../bin/NVorbis.OpenTKSupport.dll \
		`ls *.cs` Properties/AssemblyInfo.cs /t:library /define:TRACE \
		`pkg-config opentk --libs` /r:../bin/NVorbis.dll /warn:4
}

package() {
	cd "$srcdir"
	find . -name '*.pc' -exec install -Dm644 {} "$pkgdir/usr/lib/pkgconfig/"{} \;
	install -Dm644 COPYING "$pkgdir/usr/share/licenses/nvorbis/COPYING"
	cd bin
	rm *.XML
	find . -type f -exec install -Dm644 {} "$pkgdir/usr/lib/nvorbis/"{} \;
	find "$pkgdir/usr/lib/pkgconfig" -type f -exec sed -i "s|@VERSION@|${pkgver//_/-}|" {} \;
}
