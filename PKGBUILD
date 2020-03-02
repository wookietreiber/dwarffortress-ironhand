# Maintainer: Christian Krause ("wookietreiber") <christian.krause@mailbox.org>
# Contributor: Nathaniel Stickney <nstickney@gmail.com>
# shellcheck disable=2034
# shellcheck disable=2148

pkgname=dwarffortress-ironhand
_pkgname=dwarffortress
pkgver=0.47.04
_pkgver=47_04
pkgrel=1
tilesetver=47.04
pkgdesc="A single-player fantasy game in which you build a dwarven outpost or play an adventurer in a randomly generated world"
arch=(x86_64)
url="http://www.bay12games.com/dwarves/"
license=('custom:dwarffortress')
depends=(gtk2 glu sdl_image libsndfile openal sdl_ttf glew gcc-libs glib2)
makedepends=(git cmake)
optdepends=('nvidia-utils: If you have nvidia graphics'
            'alsa-lib: for alsa sound'
            'libpulse: for pulse sound')
options=('!strip' '!buildflags')
install=${_pkgname}.install
# I made a fucking github repo with the sole purpose of unfucking df a bit
# We try to compile whatever little bit of df is open source
source=(git://github.com/svenstaro/dwarf_fortress_unfuck.git#tag=${pkgver}
        dwarffortress
        dwarffortress.desktop
        "ironhand-$tilesetver.tar.gz::https://github.com/DFgraphics/Ironhand/archive/$tilesetver.tar.gz"
        dwarffortress.png
        "http://www.bay12games.com/dwarves/df_${_pkgver}_linux.tar.bz2")
sha256sums=('SKIP'
            '211eaec6559d4fd5c08341dbed1f27bfab997a57bdf61fd268f9940e244652c5'
            'e79e3d945c6cc0da58f4ca30a210c7bf1bc3149fd10406d1262a6214eb40445a'
            'fa30d5b11fe1fa3e14b85e75ea599ad0dd6ac4e8a2139efac5d4f048b58d2935'
            '83183abc70b11944720b0d86f4efd07468f786b03fa52fe429ca8e371f708e0f'
            '1de5872bf3ac32906a0082129ec88d6879b6ac7059a3513607d628090b1328e6')

conflicts=(dwarffortress dwarffortress-obsidian)
provides=("dwarffortress=$pkgver")

build() {
  # shellcheck disable=2154
  cd "$srcdir"/dwarf_fortress_unfuck || exit
  cmake .
  make
}

package() {
  # shellcheck disable=2154
  install -dm755 "$pkgdir"/opt/
  cp -r "$srcdir"/df_linux "$pkgdir"/opt/"$_pkgname"
  rm -r "$pkgdir"/opt/"$_pkgname"/df "$pkgdir"/opt/"$_pkgname"/libs/* "$pkgdir"/opt/"$_pkgname"/g_src

  cp -r "$srcdir"/Ironhand-$tilesetver/* "$pkgdir"/opt/"$_pkgname"

  find "$pkgdir"/opt/"$_pkgname" -type d -exec chmod 755 {} +
  find "$pkgdir"/opt/"$_pkgname" -type f -exec chmod 644 {} +

  install -Dm755 "$srcdir"/df_linux/libs/Dwarf_Fortress "$pkgdir"/opt/"$_pkgname"/libs/Dwarf_Fortress
  install -Dm755 "$srcdir"/dwarf_fortress_unfuck/libgraphics.so "$pkgdir"/opt/"$_pkgname"/libs/libgraphics.so
  install -Dm755 "$srcdir"/dwarffortress "$pkgdir"/usr/bin/"$_pkgname"

  # No idea why we need this. Really. This isn't being loaded dynamically, it's not linked and
  # in general there is no indication this is being used. However, it doesn't work without this symlink.
  ln -s /usr/lib/libpng.so "$pkgdir"/opt/"$_pkgname"/libs/libpng.so.3

  # Set pkgname in runscript
  sed -i "s/^pkgname=.*/pkgname=$_pkgname/" "$pkgdir"/usr/bin/"$_pkgname"

  # Desktop launcher with icon
  install -Dm644 "$srcdir"/dwarffortress.desktop "$pkgdir"/usr/share/applications/"$_pkgname".desktop
  install -Dm644 "$srcdir"/dwarffortress.png     "$pkgdir"/usr/share/pixmaps/"$_pkgname".png
  install -Dm644 "$srcdir"/df_linux/readme.txt "$pkgdir"/usr/share/licenses/"$pkgname"/readme.txt
}

# vim:set ts=2 sw=2 et:
