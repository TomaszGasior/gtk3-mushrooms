# Maintainer: krumelmonster <krumelmonster@zoho.com>
# Contributor: Tomasz Gąsior <tomaszgasior.pl>

# This file is based on original PKGBUILD of GTK3 package.
# https://git.archlinux.org/svntogit/packages.git/plain/trunk/PKGBUILD?h=packages/gtk3

__arch_pkg_commit="795341ad9f293de7836d91729a7190551f14246e"

pkgname=gtk3-mushrooms
pkgver=3.24.10
pkgrel=1
pkgdesc="GTK3 patched for classic desktops like XFCE or MATE. Please see README."
url="https://github.com/krumelmonster/gtk3-mushrooms"
conflicts=(gtk3 gtk3-print-backends)
provides=(gtk3=$pkgver gtk3-classic=$pkgver gtk3-print-backends)
arch=(x86_64)
license=(LGPL)
depends=(
	atk cairo libxcursor libxinerama libxrandr libxi libepoxy gdk-pixbuf2 fribidi
	libxcomposite libxdamage pango shared-mime-info at-spi2-atk wayland libxkbcommon
	json-glib librsvg wayland-protocols desktop-file-utils mesa gtk-update-icon-cache
)
makedepends=(
	gobject-introspection libcanberra gtk-doc sassc libcups meson
)
optdepends=(
	'libcups: printers in printing dialog'
	'dconf: default GSettings backend'
	'libcanberra: sounds events'
	'adwaita-icon-theme: default icon theme'
	'cantarell-fonts: default font'
)
source=(
	# Patch files.
	"appearance__buttons-menus-icons.patch"
	"appearance__disable-backdrop.patch"
	"appearance__file-chooser.patch"
	"appearance__message-dialogs.patch"
	"appearance__print-dialog.patch"
	"appearance__smaller-statusbar.patch"
	"csd__clean-headerbar.patch"
	"csd__disabled-by-default.patch"
	"csd__server-side-shadow.patch"
	"file-chooser__places-sidebar.patch"
	"file-chooser__typeahead.patch"
	"fixes__atk-bridge-errors.patch"
	"fixes__labels-wrapping.patch"
	"other__default-settings.patch"
	"other__hide-insert-emoji.patch"
	"other__mnemonics-delay.patch"
	"popovers__color-chooser.patch"
	"popovers__file-chooser-list.patch"
	"popovers__places-sidebar.patch"

	# Theme CSS stylesheet.
	"smaller-adwaita.css"

	# GTK source code.
	"https://download.gnome.org/sources/gtk+/${pkgver%.*}/gtk+-$pkgver.tar.xz"

	# Arch Linux package files.
	"settings.ini::https://git.archlinux.org/svntogit/packages.git/plain/trunk/settings.ini?h=packages/gtk3&id=$__arch_pkg_commit"
	"gtk-query-immodules-3.0.hook::https://git.archlinux.org/svntogit/packages.git/plain/trunk/gtk-query-immodules-3.0.hook?h=packages/gtk3&id=$__arch_pkg_commit"
)
sha256sums=('68b26360764a2ea7e057a2aaa29c6fdfe164b9987866e038d8d0188a025477fb'
            'eade303471a5929ccf6cf14ff434deccb0da017d5c4fdce3f5a3ffa117c1c954'
            '86f48054a2df6319d97db14fd17ea15d50b32ea6ba594d83e8faa1596ec657ab'
            '54020144ac0472ae170297b4158da719b49860b17234bf54351ba30f793a7fe7'
            'be4ddf03a5cce8270e8118eb331b3056972c0bd490faa6e4a4ebe332ec4c2e91'
            '81138fbaff82e37a83da1c4aa074a6c708e6c50340e0ddeff3fb70e2a0b52e1f'
            '515ff6df72934aa4294cdb1befd6c542a187fe3b4326cda68a8541dabbe657fd'
            '63bf214d836f688e628b30d1743ff9e47deb64d0f4bde9f0eb9c352fc00ca8d4'
            '1508ddc7e682cdaace327ffe2955abe90f903cf7ec923892b85673a37f76a32f'
            '8271342e6a0394f70b6e5a21afb21e2e0b645edf39a0d149d5e8b4d6b062846d'
            '7b987cc9bd7ca9722bfb881b30b082c0d7409e3cd68592f5e7a1f401d73e7672'
            '99b12d7af7efc6a014e6afcab1ee82ea0feb0b5a4e9bbd663d1c45354cd34f2b'
            '7a604d453beb9c425b8ed4a60b5e9435c3f4ee10438490641c0ade448401306a'
            '2f11b2e01862656ca2a131f4429600a1dd2784005e4bc90a9c188c51f4b6be37'
            'acd3babd22add981690728e84a89fb8bb332b7ac746e9db7cdb27c47f1ac0042'
            'c213812e1fafeb5565f7e329c4501195f04adcfe377b88439a6d51d478edc071'
            '7f3e5da1622e243243ea9b1e487460f608dc375e79d800d2f0d826fd30be68ed'
            'ef4fed3a364db8eb9c15c9ce0e733035722f168dc88b385df2178fc1168ada54'
            '2de68b575494d0d034accd7cd0ce881f366d5201a48496d8748c43f297836eac'
            'ba93f62e249f2713dbfe6c82de1be4ac655264d6407ed3dc5e05323027520f31'
            '35a8f107e2b90fda217f014c0c15cb20a6a66678f6fd7e36556d469372c01b03'
            '01fc1d81dc82c4a052ac6e25bf9a04e7647267cc3017bc91f9ce3e63e5eb9202'
            'de46e5514ff39a7a65e01e485e874775ab1c0ad20b8e94ada43f4a6af1370845')

__patch_gtk_code()
{
	for patch_file in $srcdir/*.patch; do
		patch -p 2 -i "$patch_file"
	done

	cat "$srcdir/smaller-adwaita.css" | tee -a gtk/theme/Adwaita/gtk-contained{,-dark}.css > /dev/null
}

prepare()
{
	cd "$srcdir/gtk+-$pkgver"

	# Apply patches to GTK code.
	__patch_gtk_code
}

build()
{
	CFLAGS+=" -DG_ENABLE_DEBUG -DG_DISABLE_CAST_CHECKS"
	arch-meson gtk+-$pkgver build \
		-D broadway_backend=true \
		-D colord=no \
		-D demos=false \
		-D examples=false \
		-D build-examples=false \
		-D tests=false \
		-D install_tests=false
	ninja -C build
}

package()
{
	DESTDIR="$pkgdir" meson install -C build

	install -Dt "$pkgdir/usr/share/gtk-3.0" -m644 settings.ini
	install -Dt "$pkgdir/usr/share/libalpm/hooks" -m644 gtk-query-immodules-3.0.hook

	rm "$pkgdir/usr/bin/gtk-update-icon-cache"
}
