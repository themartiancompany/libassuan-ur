# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2022, 2023, 2024, 2025  Pellegrino Prevete
#
#    All rights reserved
#    ----------------------------------------------------------------------
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General Public License
#    along with this program.  If not, see <https://www.gnu.org/licenses/>.

# Maintainer:
#   Truocolo
#     <truocolo@aol.com>
#     <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
#   Pellegrino Prevete (dvorak)
#     <pellegrinoprevete@gmail.com>
#     <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
#   David Runge
#     <dvzrv@archlinux.org>
# Contributors:
#   Gaetan Bisson
#     <bisson@archlinux.org>
#   Tobias Powalowski
#     <tpowa@archlinux.org>

if [[ ! -v "_os" ]]; then
  _os="$(
    uname \
      -o)"
fi
if [[ "${_os}" == "Android" ]]; then
  _libc="ndk-sysroot"
  _compiler="clang"
  _libcompiler="libllvm"
  _sh="dash"
elif [[ "${_os}" == "GNU/Linux" ]]; then
  _libc="glibc"
  _compiler="gcc"
  _libcompiler="libgcc"
  _sh="sh"
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
  _git="true"
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
_pkg=libassuan
pkgbase="${_pkg}"
pkgname=(
  "${_pkg}"
)
pkgver=3.0.2
_commit="0f84595a4bc706d3afb969d59618244c7db3b59f"
_gpg_error_pkgver="1.17"
pkgrel=3
_pkgdesc=(
  'IPC library used by some GnuPG related software'
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  "aarch64"
  "arm"
  "armv7l"
  "i686"
  "mips"
  "x86_64"
)
url="https://www.gnupg.org/related_software/${_pkg}"
license=(
  "FSFULLR"
  "GPL-2.0-or-later"
  "LGPL-2.1-or-later"
)
depends=(
  "${_libc}"
  "libgpg-error>=${_gpg_error_pkgver}"
  "${_sh}"
)
makedepends=(
  "autoconf"
  "automake"
  "${_libc}"
  "${_libcompiler}"
  "${_compiler}"
  "libgpg-error>=${_gpg_error_pkgver}"
  "texinfo"
)
if [[ "${_os}" == "Msys" ]]; then
  makedepends+=(
    "${_libc_headers}"
    "windows-default-manifest"
  )
fi
if [[ "${_git}" == "true" ]]; then
  makedepends+=(
    "git"
  )
fi
provides=(
  "${_pkg}.so"
)
_url="https://dev.gnupg.org/source/${_pkg}"
if [[ "${_git}" == "true" ]]; then
  _tag_name="commit"
  if [[ "${_tag_name}" == "commit" ]]; then
    _tag="${_commit}"
    _uri="${_url}.git?signed"
  elif [[ "${_tag_name}" == "tag" ]]; then
    _tag="${_pkg}-${pkgver}"
    _uri="${_url}.git?signed"
  fi
fi
_tarname="${_pkg}-${_tag}"
_tarfile="${_pkg}-${_tag}.${_archive_format}"
_src="${_tarfile}::git+${_uri}#${_tag_name}=${_tag}"
_sum="SKIP"
source=(
  "${_src}"
)
sha256sums=(
  "${_sum}"
)
validpgpkeys=(
  # "Werner Koch (dist signing 2020)"
  "6DAA6E64A76D2840571B4902528897B826403ADA"
  # Niibe Yutaka (GnuPG Release Key)
  "AC8E115BF73E2D8D47FA9908E98E9B2D19C6C8BD"
)

prepare() {
  if [[ "${_archive_format}" == "git" ]]; then
    mv \
      "${srcdir}/${_tarfile}" \
      "${srcdir}/${_tarname}"
  else
    echo \
      "Archive format: '${_archive_format}'"
  fi
  cd \
    "${_tarname}"
  sh \
    "autogen.sh"
}

build() {
  local \
    _configure_opts=()
  _configure_opts+=(
    --enable-maintainer-mode
    --prefix="/usr"
  )
  cd \
    "${_tarname}"
  "./configure" \
    "${_configure_opts[@]}"
  make
}

check() {
  make \
    check \
    -C \
      "${_tarname}"
}

package() {
  local \
    _make_opts=()
  _make_opts+=(
    DESTDIR="${pkgdir}"
  )
  make \
    "${_make_opts[@]}" \
    install \
    -C \
      "${_tarname}"
  install \
    -vDm644 \
    "${_tarname}/"{"AUTHORS","NEWS","README","ChangeLog"} \
    -t \
    "${pkgdir}/usr/share/doc/${pkgname}/"
}
