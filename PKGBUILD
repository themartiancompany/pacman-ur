# SPDX-License-Identifier: AGPL-3.0

#    ---------------------------------------------------------------
#    Copyright © 2024, 2025, 2026
#              Pellegrino Prevete
#
#    All rights reserved
#    ---------------------------------------------------------------
#
#    This program is free software: you can redistribute it
#    and/or modify it under the terms of the GNU Affero
#    General Public License as published by the Free Software
#    Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General
#    Public License along with this program.
#    If not, see <https://www.gnu.org/licenses/>.

# Maintainers:
#   Truocolo
#     <truocolo@aol.com>
#     <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
#   Pellegrino Prevete (dvorak)
#     <pellegrinoprevete@gmail.com>
#     <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Contributors:
#   Matheus
#     <matheusgwdl@protonmail.com>
#   Serge K
#     <arch@phnx47.net>
#   Nicola Squartini
#     <tensor5@gmail.com>
# Contributors:
#   Levente Polyak
#     <anthraxx[at]archlinux[dot]org>
#   Morten Linderud
#     <foxboron@archlinux.org>

_pkg=pacman
pkgbase="${_pkg}"
pkgname=(
  "${_pkg}"
)
pkgver=7.1.0.1
pkgrel=0
# use annotated tag and patch level commit from release branch (can be empty for no patches)
_git_tag=7.1.0.1
_git_patch_level_commit=1f38429b1c5f30edce30c731aa352e6363cc788e
pkgdesc="A library-based package manager with dependency support"
arch=(
  "aarch64"
  "arm"
  "armv7l"
  "armv8l"
  "i686"
  "mips"
  "pentium4"
  "powerpc"
  'x86_64'
)
_http="https://github.com"
_ns="themartiancompany"
url="${_http}/${_ns}/${_pkg}"
license=(
  'GPL-2.0-or-later'
)
depends=(
  "bash"
  "coreutils"
  "curl"
  "libcurl.so"
  "gawk"
  "gettext"
  "glibc"
  "gnupg"
  "gpgme"
  "libgpgme.so"
  "grep"
  "libarchive"
  "libarchive.so"
  "openssl"
  "libcrypto.so"
  "pacman-mirrorlist"
  "systemd"
)
makedepends=(
  "asciidoc"
  "doxygen"
  "git"
  "meson"
)
checkdepends=(
  "fakechroot"
  "python"
)
_base_devel_optdepends=(
  'base-devel:'
    "required to use makepkg"
 )
_perl_locale_gettext_optdepends=(
  'perl-locale-gettext:'
    "translation support in makepkg-template"
)
optdepends=(
  "${_base_devel_optdepends[*]}"
  "${_perl_locale_gettext_optdepends[*]}"
)
provides=(
  'libalpm.so'
)
backup=(
  "etc/pacman.conf"
  "etc/makepkg.conf"
  "etc/makepkg.conf.d/fortran.conf"
  "etc/makepkg.conf.d/rust.conf"
)
validpgpkeys=(
  # Truocolo
  #   <truocolo@aol.com>
  '97E989E6CF1D2C7F7A41FF9F95684DBE23D6A3E9'
  #   <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
  'F690CBC17BD1F53557290AF51FC17D540D0ADEED'
  # Pellegrino Prevete (dvorak)
  #   <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
  '12D8E3D7888F741E89F86EE0FEC8567A644F1D16'
)
source=(
  "git+https://gitlab.archlinux.org/pacman/pacman.git#tag=v${_git_tag}?signed"
  "revertme-makepkg-remove-libdepends-and-libprovides.patch::https://gitlab.archlinux.org/pacman/pacman/-/commit/354a300cd26bb1c7e6551473596be5ecced921de.patch"
  "pacman.conf"
  "makepkg.conf"
  "alpm.sysusers"
  "fortran.conf"
  "rust.conf"
)
sha256sums=(
  '06d082c3ce6f0811ca728515aa82d69d372800bd3ada99f5c445ef9429b6e3a6'
  'b3bce9d662e189e8e49013b818f255d08494a57e13fc264625f852f087d3def2'
  'bc80e9d0439caddd29b99a69b5060b5589cad2398c23abc5b2b8b990fae6ad8c'
  'd99c1f9608362fff9ab3a2ca0a3096a317927b42a6725bc86599da6849c9c67c'
  'c8760d7ebb6c9817d508c691c67084be251cd9c8811ee1ccf92c1278bad74c1c'
  '933b0b878fa611bf24b92f655040a3bcb4a1b67841d929013802abbb09b2ccf4'
  '6fe03e6ea3f69d99d59a48847a8ae97c2160fca847c7aedf7b89d05e4aa9386d'
)

pkgver() {
  cd \
    "${_tarname}"
  if [[ "${_git}" == "true" ]]; then
  git \
    describe \
    --abbrev=7 \
    --match \
      'v*' |
    sed \
    's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
  else
    echo \
      "7.1.0.1"
  fi
}

prepare() {
  cd \
    "${_tarname}"

  # apply patch level commits on top of annotated tag
  if [[ -n ${_git_patch_level_commit} ]]; then
    if [[ v${_git_tag} != $(git describe --tags --abbrev=0 "${_git_patch_level_commit}") ]] then
      error "patch level commit ${_git_patch_level_commit} is not a descendant of v${_git_tag}"
      exit 1
    fi
    git rebase "${_git_patch_level_commit}"
  fi

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
  install -D -m644 "$srcdir/alpm.sysusers" "${pkgdir}"/usr/lib/sysusers.d/alpm.conf
  install -m644 "$srcdir/fortran.conf" "$pkgdir/etc/makepkg.conf.d"
  install -m644 "$srcdir/rust.conf" "$pkgdir/etc/makepkg.conf.d"

  local wantsdir="$pkgdir/usr/lib/systemd/system/sockets.target.wants"
  install -dm755 "$wantsdir"

  local unit
  for unit in dirmngr gpg-agent gpg-agent-{browser,extra,ssh} keyboxd; do
    ln -s "../${unit}@.socket" "$wantsdir/${unit}@etc-pacman.d-gnupg.socket"
  done
}

# vim: set ts=2 sw=2 et:
