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

_etc_get() {
  local \
    _etc \
    _os
  _os="$(
    uname \
      -o)"
  _etc="etc"
  if [[ "${_os}" == "Android" ]]; then
    _etc="usr/etc"
  fi
  echo \
    "${_etc}"
}

_os="$(
  uname \
    -o)"
# Android and Windows do require patches
# or to import them to the repo.
# I'll do that asap
if [[ "${_os}" == "Android" ]]; then
  _libc="ndk-sysroot"
  _compiler="clang"
  _libcompiler="llvm-libs"
elif [[ "${_os}" == "GNU/Linux" ]]; then
  _libc="glibc"
  _compiler="gcc"
  _libcompiler="libgcc"
elif [[ "${_os}" == "Msys" ]]; then
  _libc="msys2-w32api-runtime"
  _libc_headers="msys2-w32api-headers"
  _compiler="gcc"
  _libcompiler="gcc-libs"
  _sh="sh"
else
  _msg=(
    "Unknown os '${_os}'."
  )
  msg \
    "${_msg[*]}"
  _libc="msys2-w32api-runtime"
  _libc_headers="msys2-w32api-headers"
  _compiler="gcc"
  _libcompiler="gcc-libs"
  _sh="sh"
fi
_evmfs_available="$(
  command \
    -v \
    "evmfs" || \
    true)"
if [[ ! -v "_evmfs" ]]; then
  if [[ "${_evmfs_available}" != "" ]]; then
    _evmfs="true"
  elif [[ "${_evmfs_available}" == "" ]]; then
    _evmfs="false"
  fi
fi
if [[ ! -v "_git" ]]; then
  _git="false"
fi
if [[ ! -v "_offline" ]]; then
  _offline="false"
fi
if [[ ! -v "_ns" ]]; then
  _ns="themartiancompany"
  # went rogue
  # _ns="pacman"
fi
if [[ ! -v "_git_service" ]]; then
  if [[ "${_ns}" == "themartiancompany" ]]; then
    _git_service="github"
  fi
fi
if [[ ! -v "_archive_format" ]]; then
  if [[ "${_git}" == "true" ]]; then
    if [[ "${_evmfs}" == "true" ]]; then
      _archive_format="bundle"
    elif [[ "${_evmfs}" == "false" ]]; then
      _archive_format="git"
    fi
  elif [[ "${_git}" == "false" ]]; then
    if [[ "${_git_service}" == "github" ]]; then
      _archive_format="zip"
    elif [[ "${_git_service}" == "gitlab" ]]; then
      _archive_format="tar.gz"
    fi
  fi
fi
if [[ ! -v "_bash" ]]; then
  _shell="bash"
fi
if [[ ! -v "_docs" ]]; then
  _docs="true"
fi
if [[ ! -v "_sissystemd" ]]; then
  _sissystemd="false"
  if [[ "${_os}" == "Android" ]]; then
    _sissystemd="true"
  fi
fi
if [[ ! -v "_systemd" ]]; then
  if [[ "${_os}" == "Android" || \
        "${_os}" == "Msys" ]]; then
    # should check if sissystemd is
    # available
    _systemd="false"
  elif [[ "${_os}" == "GNU/Linux" ]]; then
    # for now we do assume yes
    # but it should simply be detected
    _systemd="true"
  fi
fi
_py="python"
_pkg=pacman
pkgbase="${_pkg}"
pkgname=(
  "${_pkg}"
)
_pkgver=7.1.0.17
_gnupg_pkgver=2.5
pkgver="${_pkgver}"
pkgrel=6
# use annotated tag and patch level commit
# from release branch (can be empty for no patches)
_git_tag="${_pkgver}"
_commit="9d5d9bf03d037f0ce48988db10e07a460eb41f8e"
_git_patch_level_commit="1f38429b1c5f30edce30c731aa352e6363cc788e"
_pkgdesc=(
  "A library-based package"
  "manager with dependency support."
)
pkgdesc="${_pkgdesc[*]}"
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
_http_arch="https://gitlab.archlinux.org"
_url_old="${_http_arch}/${_pkg}/${_pkg}"
_http="https://github.com"
url="${_http}/${_ns}/${_pkg}"
license=(
  'GPL-2.0-or-later'
)
depends=(
  "${_shell}"
  "coreutils"
  "curl"
  "libcurl.so"
  "gawk"
  "gettext"
  "${_libc}"
  "${_libcompiler}"
  # "gnupg"
  # Test QR
  "gnupg>=${_gnupg_pkgver}"
  "gpgme"
  "libgpgme.so"
  "grep"
  "libarchive"
  "libarchive.so"
  "openssl"
  "libcrypto.so"
  "${_pkg}-mirrorlist"
)
if [[ "${_systemd}" == "true" ]]; then
  depends+=(
    "systemd"
  )
fi
makedepends=(
  "gettext"
  "meson"
  "${_libc}"
  "${_compiler}"
  "${_libcompiler}"
  "libarchive"
)

if [[ "${_os}" == "Android" ]]; then
  makedepends+=(
    "termux-tools"
  )
fi
if [[ "${_os}" == "Msys" ]]; then
  makedepends+=(
    "${_libc_headers}"
    "windows-default-manifest"
  )
fi
if [[ "${_docs}" == "true" ]]; then
  makedepends+=(
    "asciidoc"
    "doxygen"
  )
fi
if [[ "${_git}" == "true" ]]; then
  makedepends+=(
    "git"
  )
fi
checkdepends=(
  "fakechroot"
  "${_py}"
)
_base_devel_optdepends=(
  'base-devel:'
    "required to use makepkg"
 )
_perl_locale_gettext_optdepends=(
  'perl-locale-gettext:'
    "translation support in makepkg-template"
)
_reallymakepkg_optdepends=(
  "reallymakepkg:"
    "needed to build packages"
    "reliably"
)
optdepends=(
  "${_base_devel_optdepends[*]}"
  "${_perl_locale_gettext_optdepends[*]}"
  "${_reallymakepkg_optdepends[*]}"
)
provides=(
  'libalpm.so'
)
_etc="$(
  _etc_get)"
backup=(
  "${_etc}/${_pkg}.conf"
  "${_etc}/makepkg.conf"
  "${_etc}/makepkg.conf.d/fortran.conf"
  "${_etc}/makepkg.conf.d/rust.conf"
)
# whats this
_libdepends_libprovides_commit="354a300cd26bb1c7e6551473596be5ecced921de"
source=(
  "revertme-makepkg-remove-libdepends-and-libprovides.patch::${_url_old}/-/commit/${_libdepends_libprovides_commit}.patch"
  "${_pkg}.conf"
  "makepkg.conf"
  "alpm.sysusers"
  "fortran.conf"
  "rust.conf"
  # Windows
  "makepkg.msys.conf"
  "makepkg_mingw.conf"
  "makepkg_mingw.d.clang64.conf"
  "makepkg_mingw.d.clangarm64.conf"
  "makepkg_mingw.d.mingw64.conf"
  "makepkg_mingw.d.ucrt64.conf"
)
sha256sums=(
  'b3bce9d662e189e8e49013b818f255d08494a57e13fc264625f852f087d3def2'
  'bc80e9d0439caddd29b99a69b5060b5589cad2398c23abc5b2b8b990fae6ad8c'
  'd99c1f9608362fff9ab3a2ca0a3096a317927b42a6725bc86599da6849c9c67c'
  "190ef25d5a320d7cfe7a17683d1956f0c39710291ae27d0968d12f431cb2bdaa"
  '933b0b878fa611bf24b92f655040a3bcb4a1b67841d929013802abbb09b2ccf4'
  "6fe03e6ea3f69d99d59a48847a8ae97c2160fca847c7aedf7b89d05e4aa9386d"
  # Windows
  "9ed1492c92d9c46e117738365eee25e8fa622a92b1f4db288e919b3102be3c6b"
  "6c9ce9f77b397435fc62966c8d56056f28d10cba16dcb964e4d9f39b9a50b7e5"
  "5f62ef7f560c5215348bd26ff4a7fd9a7f8857193bbad43f139dbff7df449350"
  "500e46d05baccd23ec24245afd66084510b99baf3ebcd2fcc21d20687817abf5"
  "96d7dfa9f17424918f4efbec2c08b6cc7aa31616324382be7483b96d7559e119"
  "274e5dc04f8213c170da2c15ffaad216846ae047a7f21a34156d64f12ce75677"
)
_url="${url}"
_tag="${_commit}"
_tag_name="commit"
_tarname="${pkgname}-${_tag}"
_tarfile="${_tarname}.${_archive_format}"
_bundle_sum="dc93b98c622e4eeb36969e26982f727d63f54e69b2083ade3e074f716bb22ce6"
_bundle_sig_sum="b1f3f0591e12b8e2f374aa2b806d6bce5e1b27544195ea5f40e0ba1e18a37339"
_github_sum="7f026bfd9b840910b80f58ed10cc9bf6264a9e770bb38a1b6664750519ce797a"
_github_sig_sum="9d7792082022e440636a6af2acc63704ae1d5d32ace4c27a8f0bebad2b025007"
# All of this stuff must absoleutely go as
# soon as EVMFS 1.0 and gurl release
# I'm tired of specifying different sums
# when everything can just be derived from
# the commit
if [[ "${_evmfs}" == "true" ]]; then
  if [[ "${_git}" == "true" ]]; then
    _sum="${_bundle_sum}"
    _sig_sum="${_bundle_sig_sum}"
  elif [[ "${_git}" == "false" ]]; then
    _sum="${_bundle_sum}"
    _sig_sum="${_bundle_sig_sum}"
  fi
elif [[ "${_evmfs}" == "false" ]]; then
  if [[ "${_git}" == "true" ]]; then
    _sum="SKIP"
    _sig_sum="SKIP"
  elif [[ "${_git}" == "false" ]]; then
    if [[ "${_git_service}" == "github" ]]; then
      _sum="${_github_sum}"
      _sig_sum="${_github_sig_sum}"
      # _sum="SKIP"
      # _sig_sum="SKIP"
    fi
  fi
fi
# Dvorak
_evmfs_ns="0x87003Bd6C074C713783df04f36517451fF34CBEf"
_evmfs_network="100"
_evmfs_address="0x69470b18f8b8b5f92b48f6199dcb147b4be96571"
_evmfs_dir="evmfs://${_evmfs_network}/${_evmfs_address}/${_evmfs_ns}"
_evmfs_uri="${_evmfs_dir}/${_sum}"
_evmfs_src="${_tarfile}::${_evmfs_uri}"
_sig_uri="${_evmfs_dir}/${_sig_sum}"
_sig_src="${_tarfile}.sig::${_sig_uri}"
if [[ "${_evmfs}" == "true" ]]; then
  _src="${_evmfs_src}"
  if [[ "${_git}" == "false" ]]; then
    source+=(
      "${_sig_src}"
    )
    sha256sums+=(
      "${_sig_sum}"
    )
    _sum="${_github_sum}"
  elif [[ "${_git}" == "true" ]]; then
    source+=(
      "${_sig_src}"
    )
    sha256sums+=(
      "${_sig_sum}"
    )
    _sum="${_bundle_sum}"
  fi
elif [[ "${_evmfs}" == "false" ]]; then
  if [[ "${_git}" == true ]]; then
    _src="${_tarname}::git+${_url}#${_tag_name}=${_tag}?signed"
    _sum="SKIP"
  elif [[ "${_git}" == false ]]; then
    _uri=""
    if [[ "${_git_service}" == "github" ]]; then
      if [[ "${_tag_name}" == "commit" ]]; then
        _uri="${_url}/archive/${_commit}.${_archive_format}"
        _sum="${_github_sum}"
      fi
    elif [[ "${_git_service}" == "gitlab" ]]; then
      if [[ "${_tag_name}" == "commit" ]]; then
        _uri="${_url}/-/archive/${_tag}/${_tag}.${_archive_format}"
      fi
    fi
    _src="${_tarfile}::${_uri}"
  fi
fi
source+=(
  "${_src}"
)
sha256sums+=(
  "${_sum}"
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
  elif [[ "${_git}" == "false" ]]; then
    echo \
      "${_pkgver}"
  fi
}

_root_get() {
  dirname \
    "$(dirname \
         "$(dirname \
              "$(command \
                -v \
                "env")")")"
}

_android_configure() {
  local \
    _msg=()
  # Not needed i gue-
  # sed \
  #   "/fakeroot.sh.in/d;
  #    /sudo.sh.in/d" \
  #   -i \
  #   "scripts/libmakepkg/executable/meson.build"
  # true location
  _msg=(
    "wat"
  )
  echo \
    "${_msg[*]}"
  sed \
    -e \
      "s%'/bin/true'%'true'%g" \
    -i \
    "${srcdir}/${_tarname}/doc/meson.build"
  cat \
    "${srcdir}/${_tarname}/doc/meson.build"
  # sed \
  #   -e \
  #     "s%BINDIR, symlink%'@ANDROID_ROOT@/bin', symlink%g" \
  #   -i \
  #   "${srcdir}/${_tarname}/scripts/meson.build"
  sed \
    -e \
      "s%join_paths(BINDIR, symlink))%join_paths(BINDIR, symlink), '@ANDROID_ROOT@/bin')%g" \
    -i \
    "${srcdir}/${_tarname}/scripts/meson.build"
  termux-fix-shebang \
    "${srcdir}/${_tarname}/build-aux/meson-make-symlink.sh"
  # sed \
  #   -e \
  #     "s%'ln', '-sf', 'repo-add', '@OUTPUT@'%'ln', '-sf', 'repo-add', '@OUTPUT@', '@ANDROID_ROOT@'%g" \
  #   -i \
  #   "${srcdir}/${_tarname}/scripts/meson.build"
  for _file in \
    "${srcdir}/${_tarname}/build-aux/meson-make-symlink.sh" \
    "${srcdir}/${_tarname}/lib/libalpm/trans.c" \
    "${srcdir}/${_tarname}/lib/libalpm/util.c" \
    "${srcdir}/${_tarname}/scripts/meson.build"; do
    sed \
      -e \
        "s%@ANDROID_ROOT@%$(_root_get)%g" \
      -i \
      "${_file}"
  done
  cat \
    "${srcdir}/${_tarname}/scripts/meson.build"
}

_windows_configure() {
  sed \
    -e \
      "s%'/bin/true'%'true'%g" \
    -i \
    "${srcdir}/${_tarname}/doc/meson.build"
}

prepare() {
  local \
    _commit_short \
    _os \
    _patch
  local \
    -a \
    _patches
  _os="$(
    uname \
      -o)"
  cd \
    "${_tarname}"
  if [[ "${_git}" == "true" ]]; then
    _commit_short="$(
      git \
        describe \
          --tags \
          --abbrev=0 \
          "${_git_patch_level_commit}")"
  fi
  # apply patch level commits on top of annotated tag
  # if [[ -n "${_git_patch_level_commit}" ]]; then
  #   if [[ "v${_git_tag}" != "${_commit_short}" ]] then
  #     _msg=(
  #       "Patch level commit '${_git_patch_level_commit}"
  #       "is not a descendant of v${_git_tag}."
  #     )
  #     error \
  #       "${_msg[*]}"
  #     exit \
  #       1
  #   fi
  #   git \
  #     rebase \
  #     "${_git_patch_level_commit}"
  # fi
  # handle patches
  _patches=( $(
    printf \
      '%s\n' \
      "${source[@]}" |
      grep \
        '.patch')
  )
  _patches=(
    "${_patches[@]%%::*}"
  )
  _patches=(
    "${_patches[@]##*/}"
  )
  if (( ${#_patches[@]} != 0 )); then
    for _patch in "${_patches[@]}"; do
      if [[ "${_patch}" =~ revertme-* ]]; then
        msg2 \
          "Reverting patch '${_patch}'..."
        patch \
          -RNp1 < \
          "../${_patch}"
      else
        msg2 \
          "Applying patch '${_patch}'..."
        patch \
          -Np1 < \
          "../${_patch}"
      fi
    done
  fi
  if [[ "${_os}" == "Android" ]]; then
    _android_configure
  elif [[ "${_os}" == "Msys" ]]; then
    _windows_configure
  else
    echo \
      "OS: ${_os}"
  fi
  true
  # if [[ "${_os}" == "Android" ]]; then
  #   sed \
  #     -e \
  #       "/command : ['/bin/true'],/command : ['true'],/g" \
  #     -i \
  #     "doc/meson.build"
  # fi
}

build() {
  local \
    _docs_option \
    _meson_opts=() \
    _cflags=() \
    _meson \
    _os
  _meson="$(
    command \
      -v \
      "meson")"
  _os="$(
    uname \
      -o)"
  _cflags+=(
    ${CFLAGS}
  )
  if [[ "${_os}" == "Android" ]]; then
    _cflags+=(
      -Wno-format-security
      #-Nno-undeclared-identifiers
      -Wno-incompatible-pointer-types
    )
  fi
  if [[ "${_os}" == "Msys" ]]; then
    _cflags+=(
      -Wno-implicit-function-declaration
    )
    pacman \
      -Ql \
      mingw-w64-x86_64-gcc | \
      grep \
        "bin"
    export \
      CC="$(
        command \
          -v \
          "gcc")"
      # CC="mingw-w64-x86_64-gcc" \
      # LD="$(which ld)"
    _meson="/mingw64/bin/meson"
    echo \
      "meson: '${_meson}'"
    echo \
      "compiler: '${CC}'"
  fi
  if [[ "${_docs}" == "true" ]]; then
    _docs_option="enabled"
    _doxygen_option="enabled"
  fi
  _meson_opts+=(
    --prefix="/usr"
    --buildtype="plain"
    -D
      doc="${_docs_option}"
    -D
      doxygen="${_doxygen_option}"
    -D
      scriptlet-shell="/usr/bin/bash"
    -D
      ldconfig="/usr/bin/ldconfig"
  )
  export \
    CFLAGS="${_cflags[*]}" \
    DESTDIR="${pkgname}"
  cd \
    "${_tarname}"
  DESTDIR="${pkgdir}" \
  meson \
    "${_meson_opts[@]}" \
    build
  DESTDIR="${pkgdir}" \
  meson \
    compile \
    -C \
      "build"
}

check() {
  cd \
    "${_tarname}"
  meson \
    test \
    -C \
      "build"
}

package() {
  local \
    _etc \
    _unit \
    _wantsdir
  _wantsdir="${pkgdir}/usr/lib/systemd/system/sockets.target.wants"
  _etc="$(
    _etc_get)"
  cd \
    "${_tarname}"
  DESTDIR="${pkgdir}" \
  meson \
    install \
    -C \
      "build"
  # install Arch specific stuff
  install \
    -vdm755 \
    "${pkgdir}/${_etc}"
  install \
    -vDm644 \
    "${srcdir}/${_pkg}.conf" \
    "${pkgdir}/${_etc}"
  install \
    -vDm644 \
    "${srcdir}/makepkg.conf" \
    "${pkgdir}/${_etc}"
  install \
    -vDm644 \
    "${srcdir}/alpm.sysusers" \
    "${pkgdir}/usr/lib/sysusers.d/alpm.conf"
  install \
    -vDm644 \
    "${srcdir}/fortran.conf" \
    "${pkgdir}/${_etc}/makepkg.conf.d"
  install \
    -vDm644 \
    "${srcdir}/rust.conf" \
    "${pkgdir}/${_etc}/makepkg.conf.d"
  if [[ "${_systemd}" == "true" ]]; then
    install \
      -vdm755 \
      "${_wantsdir}"
    for _unit in "dirmngr" \
                 "gpg-agent" \
                 "gpg-agent-"{"browser","extra","ssh"} \
                 "keyboxd"; do
      ln \
        -s \
        "../${_unit}@.socket" \
        "${_wantsdir}/${_unit}@etc-${_pkg}.d-gnupg.socket"
    done
  fi
  true
}

# vim: set ts=2 sw=2 et:
