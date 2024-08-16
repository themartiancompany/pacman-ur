# Maintainer: Levente Polyak <anthraxx[at]archlinux[dot]org>
# Maintainer: Morten Linderud <foxboron@archlinux.org>

pkgname=pacman
pkgver=7.0.0.r1.g7cf2b01
pkgrel=1
_commit=7cf2b0186d873be4218fe5be127dd029a0af03fe
pkgdesc="A library-based package manager with dependency support"
arch=('x86_64')
url="https://www.archlinux.org/pacman/"
license=('GPL-2.0-or-later')
depends=(
  bash
  coreutils
  curl
  gawk
  gettext
  glibc
  gnupg
  gpgme
  grep
  libarchive
  pacman-mirrorlist
)
makedepends=(
  asciidoc
  doxygen
  git
  meson
)
checkdepends=(
  fakechroot
  python
)
optdepends=(
  'perl-locale-gettext: translation support in makepkg-template'
)
provides=('libalpm.so')
backup=(etc/pacman.conf
        etc/makepkg.conf)
validpgpkeys=('6645B0A8C7005E78DB1D7864F99FFE0FEAE999BD'  # Allan McRae <allan@archlinux.org>
              'B8151B117037781095514CA7BBDFFC92306B1121') # Andrew Gregory (pacman) <andrew@archlinux.org>
source=("git+https://gitlab.archlinux.org/pacman/pacman.git#commit=${_commit}"
        revertme-makepkg-remove-libdepends-and-libprovides.patch::https://gitlab.archlinux.org/pacman/pacman/-/commit/354a300cd26bb1c7e6551473596be5ecced921de.patch
        pacman.conf
        makepkg.conf)
sha256sums=('955e38a88c99a42600f24219a3084855d2d37dc10df5473604e62913876cb42b'
            'b3bce9d662e189e8e49013b818f255d08494a57e13fc264625f852f087d3def2'
            '656c4d4cb8cb12adbf178fc8cb2fd25f8c285d6572bbdbb24d865d00e0d5a85a'
            '23d512b4d504c36fd49b58a32eb1a93a7f2ed67424a3672fc3c645cb443ad6f7')

pkgver() {
  cd "$pkgname"
  git describe --abbrev=7 --match 'v*' | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
  cd "$pkgname"

  # handle patches
  local -a patches
  patches=($(printf '%s\n' "${source[@]}" | grep '.patch'))
  patches=("${patches[@]%%::*}")
  patches=("${patches[@]##*/}")

  if (( ${#patches[@]} != 0 )); then
    for patch in "${patches[@]}"; do
      if [[ $patch =~ revertme-* ]]; then
        msg2 "Reverting patch $patch..."
        patch -RNp1 < "../$patch"
      else
        msg2 "Applying patch $patch..."
        patch -Np1 < "../$patch"
      fi
    done
  fi
}

build() {
  cd "$pkgname"

  meson --prefix=/usr \
        --buildtype=plain \
        -Ddoc=enabled \
        -Ddoxygen=enabled \
        -Dscriptlet-shell=/usr/bin/bash \
        -Dldconfig=/usr/bin/ldconfig \
        build

  meson compile -C build
}

check() {
  cd "$pkgname"

  meson test -C build
}

package() {
  cd "$pkgname"

  DESTDIR="$pkgdir" meson install -C build

  # install Arch specific stuff
  install -dm755 "$pkgdir/etc"
  install -m644 "$srcdir/pacman.conf" "$pkgdir/etc"
  install -m644 "$srcdir/makepkg.conf" "$pkgdir/etc"

  local wantsdir="$pkgdir/usr/lib/systemd/system/sockets.target.wants"
  install -dm755 "$wantsdir"

  local unit
  for unit in dirmngr gpg-agent gpg-agent-{browser,extra,ssh} keyboxd; do
    ln -s "../${unit}@.socket" "$wantsdir/${unit}@etc-pacman.d-gnupg.socket"
  done
}

# vim: set ts=2 sw=2 et:
