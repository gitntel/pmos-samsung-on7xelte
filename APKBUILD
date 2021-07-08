# Reference: <https://postmarketos.org/vendorkernel>
# Kernel config based on: arch/arm64/configs/(CHANGEME!)

pkgname=linux-samsung-on7xelte
pkgver=3.18.14
pkgrel=0
pkgdesc="Samsung Galaxy J7 Prime kernel fork"
arch="aarch64"
_carch="arm64"
_flavor="samsung-on7xelte"
url="https://kernel.org"
license="GPL-2.0-only"
options="!strip !check !tracedeps pmb:cross-native"
makedepends="
	bash
	bc
	bison
	devicepkg-dev
	flex
	openssl-dev
	perl
	dtbtool-exynos
"

# Source
_repository="android_kernel_samsung_exynos7870"
_commit="cea077cb774b8be539f79da6eb396fd5ac985020"
_config="config-$_flavor.$arch"
source="
	$pkgname-$_commit.tar.gz::https://github.com/DevOtag-Open-Source/$_repository/archive/$_commit.tar.gz
	$_config
	gcc7-give-up-on-ilog2-const-optimizations.patch
	gcc8-fix-put-user.patch
	gcc10-extern_YYLOC_global_declaration.patch
	01-fix-dtb.patch
"
builddir="$srcdir/$_repository-$_commit"
_outdir="out"

prepare() {
	default_prepare
	. downstreamkernel_prepare
}

build() {
	dtbTool-exynos -o "$_outdir/arch/$_carch/boot"/dt.img \
                $(find "$_outdir/arch/$_carch/boot/dts/" -name *on7xelte*.dtb)

	unset LDFLAGS
	make O="$_outdir" ARCH="$_carch" CC="${CC:-gcc}" \
		KBUILD_BUILD_VERSION="$((pkgrel + 1 ))-postmarketOS"
}

package() {
	install -Dm644 "$_outdir/arch/$_carch/boot"/dt.img \
		"$pkgdir"/boot/dt.img

	downstreamkernel_package "$builddir" "$pkgdir" "$_carch" "$_flavor" "$_outdir"
}

sha512sums="
a6526ece102777c808941ba0ecc8175688320f2f23b718f25b4d39de3b6200d97c5b9dc323eacb5f70371abb21b19f42117381df1a76888fafecf5fc5dc2cd78  linux-samsung-on7xelte-cea077cb774b8be539f79da6eb396fd5ac985020.tar.gz
f7435096c53f3b3c9ba0ea527fef2a671a5ba96e6c5d122e334ee111628c292f85fc6dde3d32c144453fe912d6279fc2a8c8d56b89c0985fa243855e958423e1  config-samsung-on7xelte.aarch64
77eba606a71eafb36c32e9c5fe5e77f5e4746caac292440d9fb720763d766074a964db1c12bc76fe583c5d1a5c864219c59941f5e53adad182dbc70bf2bc14a7  gcc7-give-up-on-ilog2-const-optimizations.patch
197d40a214ada87fcb2dfc0ae4911704b9a93354b75179cd6b4aadbb627a37ec262cf516921c84a8b1806809b70a7b440cdc8310a4a55fca5d2c0baa988e3967  gcc8-fix-put-user.patch
2b48f1bf0e3f70703d2cdafc47d5e615cc7c56c70bec56b2e3297d3fa4a7a1321d649a8679614553dde8fe52ff1051dae38d5990e3744c9ca986d92187dcdbeb  gcc10-extern_YYLOC_global_declaration.patch
f6e07edcb26b09dcd16cc84950a21e36a339019b6fefea5b6b277e942dd96e08b54b447bd6e20496b35ec04268c271bd4247a9edcb3b08cbfe0a4156f715ffd9  01-fix-dtb.patch
"
