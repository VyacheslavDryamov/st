# Maintainer: mar77i <mar77i at mar77i dot ch>
# Past Maintainer: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Scytrin dai Kinthra <scytrin@gmail.com>

pkgname=st-git
_pkgname=st
pkgver=0.7.24.g5a10aca
pkgrel=1
pkgdesc='Simple virtual terminal emulator for X'
url='http://st.suckless.org/'
arch=('i686' 'x86_64')
license=('MIT')
options=('zipman')
depends=('libxft')
makedepends=('ncurses' 'libxext' 'git')
epoch=1
# include config.h and any patches you want to have applied here
source=('git://git.suckless.org/st'
	'config.h'
'http://st.suckless.org/patches/st-hidecursor-20170404-745c40f.diff'
'http://st.suckless.org/patches/st-externalpipe-20170320-e7ed326.diff'
'http://st.suckless.org/patches/st-scrollback-20170329-149c0d3.diff'
'http://st.suckless.org/patches/st-scrollback-mouse-20170427-5a10aca.diff'
'http://st.suckless.org/patches/st-scrollback-mouse-altscreen-20170427-5a10aca.diff'
)
sha1sums=('SKIP'
          '0907fa4504b8c27ceea6563fb2314ea88aa30f0c'
          'b85e06f6190a55c2754fd47a1ff3f3dfd74722f6'
          '928058c0074879f0ad281c77d3c8452ec262327a'
          '6a41683a04375f192d014cead227c749c595a721'
          '81b4a93c7bbf6854ec60630a5f7f50268a106a4b'
          'b181427a52dea0f1b5635cf51263a016d4d27cdf')

provides=("${_pkgname}")
conflicts=("${_pkgname}")

pkgver() {
	cd "${_pkgname}"
	git describe --tags |sed 's/-/./g'
}

prepare() {
	local file
	cd "${_pkgname}"
		sed \
		-e 's/CPPFLAGS =/CPPFLAGS +=/g' \
		-e 's/CFLAGS =/CFLAGS +=/g' \
		-e 's/LDFLAGS =/LDFLAGS +=/g' \
		-e 's/_BSD_SOURCE/_DEFAULT_SOURCE/' \
		-i config.mk
	sed '/@tic/d' -i Makefile
	for file in "${source[@]}"; do
		if [[ "$file" == "config.h" ]]; then
			# add config.h if present in source array
			# Note: this supersedes the above sed to config.def.h
			cp "$srcdir/$file" .
		elif [[ "$file" == *.diff || "$file" == *.patch ]]; then
			# add all patches present in source array
			patch -F3 -Np1 <"$srcdir/$(basename ${file})"
		fi
	done
}

build() {
	cd "${_pkgname}"
	make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
	cd "${_pkgname}"
	make PREFIX=/usr DESTDIR="${pkgdir}" install
	install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
	install -Dm644 README "${pkgdir}/usr/share/doc/${pkgname}/README"
}
