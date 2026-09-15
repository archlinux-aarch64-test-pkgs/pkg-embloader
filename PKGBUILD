# Maintainer: BigfootACA <bigfoot@classfun.cn>

pkgname=embloader
pkgver=0.7
pkgrel=1
pkgdesc="Embedded Bootloader"
arch=(x86_64 aarch64)
url="https://github.com/BigfootACA/embloader"
license=(GPL-2.0-or-later)
makedepends=(gcc git make nasm python)
options=(!debug !strip)
source=("git+https://github.com/BigfootACA/embloader.git#tag=${pkgver}")
md5sums=(SKIP)

prepare(){
	cd "$srcdir/embloader"
	git submodule update --init --recursive --depth 1
	sed -i 's/-Werror//g' edk2/BaseTools/Conf/tools_def.template
	sed -i 's/-Werror//g' edk2/BaseTools/Source/C/Makefiles/header.makefile
}

build(){
	cd "$srcdir/embloader"
	find build/Build/embloader/ -name '*.efi' -delete 2>/dev/null || true
	find build/Build/embloader/ -name '*.debug' -delete 2>/dev/null || true
	export EDK2_PATH="$(realpath edk2)"
	make -j$(nproc) basetools
	make -j$(nproc) build-edk2
}

package() {
	cd "$srcdir/embloader"
	install -Dm644 build/Build/embloader/*_*/*/embloader.efi \
		"$pkgdir/usr/share/embloader/embloader.efi"
	install -Dm644 build/Build/embloader/*_*/*/embloader.debug \
		"$pkgdir/usr/share/embloader/embloader.debug"
	install -d "$pkgdir/usr/share/doc/embloader/"
	cp -a docs/* "$pkgdir/usr/share/doc/embloader/"
	install -d "$pkgdir/usr/share/embloader/configs/"
	cp -a configs/* "$pkgdir/usr/share/embloader/configs/"
	if [ -f README.md ]; then install -m644 README.md "$pkgdir/usr/share/doc/embloader/"; fi
	if [ -f LICENSE ]; then install -m644 LICENSE "$pkgdir/usr/share/doc/embloader/"; fi
}
