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
if [[ ! -v "_git_service" ]]; then
  _git_service="github"
fi
_pkg=libassuan
if [[ ! -v "_git_http" ]]; then
  if [[ "${_git_service}" == "github" ]]; then
    _git_http="github.com"
  elif [[ "${_git_service}" == "gitlab" ]]; then
    _git_http="gitlab.com"
  elif [[ "${_git_service}" == "gnupg" ]]; then
    _git_http="dev.${_pkg}.org"
  fi
fi
if [[ ! -v "_ns" ]]; then
  if [[ "${_git_service}" == "github" ]]; then
    _ns="themartiancompany"
  elif [[ "${_git_service}" == "gitlab" ]]; then
    _ns="freepg"
  elif [[ "${_git_service}" == "gnupg" ]]; then
    _ns="sources"
  fi
fi
if [[ ! -v "_git" ]]; then
  if [[ "${_git_service}" == "gnupg" ]]; then
    _git="true"
  else
    _git="false"
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
pkgbase="${_pkg}"
pkgname=(
  "${_pkg}"
)
pkgver=3.0.2
_commit="0f84595a4bc706d3afb969d59618244c7db3b59f"
_gpg_error_pkgver="1.17"
pkgrel=29
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
_git_http_uri="https://${_git_http}.com/${_ns}/${_pkg}"
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
_http="https://${_git_http}"
_url="${_http}/${_ns}/${_pkg}"
_tag_name="commit"
if [[ "${_tag_name}" == "commit" ]]; then
  _tag="${_commit}"
elif [[ "${_tag_name}" == "tag" ]]; then
  if [[ "${_git_service}" == "gnupg" ]]; then
    _tag="${_pkg}-${pkgver}"
  else
    _tag="${_pkg}-${pkgver}-freepg"
  fi
fi
_tarname="${_pkg}-${_tag}"
_tarfile="${_pkg}-${_tag}.${_archive_format}"
_gitlab_sum="SKIP"
_gitlab_sig_sum="SKIP"
_github_sum="boh"
_github_sig_sum="boh"
_bundle_sum="SKIP"
_bundle_sig_sum="SKIP"
# Dvorak
_evmfs_ns="0x87003Bd6C074C713783df04f36517451fF34CBEf"
if [[ "${_evmfs}" == "true" ]]; then
  if [[ "${_git}" == "true" ]]; then
    _sum="${_bundle_sum}"
    _sig_sum="${_bundle_sig_sum}"
    # Tallero
    _evmfs_ns="0x6ec7cC56dCeC0a00CB15E97C64B1a5Ec7A31403c"
  elif [[ "${_git}" == "false" ]]; then
    if [[ "${_git_service}" == "github" ]]; then
      _sum="${_github_sum}"
      _sig_sum="${_github_sig_sum}"
      # Dvorak
      _evmfs_ns="0x87003Bd6C074C713783df04f36517451fF34CBEf"
      # Truocolo
      _evmfs_ns="0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b"
    elif [[ "${_git_service}" == "gitlab" ]]; then
      _sum="${_gitlab_sum}"
      _sig_sum="${_gitlab_sig_sum}"
      # Tallero
      _evmfs_ns="0x6ec7cC56dCeC0a00CB15E97C64B1a5Ec7A31403c"
    fi
  fi
elif [[ "${_evmfs}" == "false" ]]; then
  if [[ "${_git}" == "true" ]]; then
    if [[ "${_tag_name}" == "tag" ]]; then
      _sum='lal'
      _b2_sum='lallissimo'
    elif [[ "${_tag_name}" == "commit" ]]; then
      _sum="SKIP"
      _b2_sum="SKIP"
    fi
  elif [[ "${_git}" == "false" ]]; then
    if [[ "${_git_service}" == "github" ]]; then
      _sum="${_github_sum}"
      _sig_sum="${_github_sig_sum}"
      # Truocolo
      _evmfs_ns="0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b"
    elif [[ "${_git_service}" == "gitlab" ]]; then
      _sum="${_gitlab_sum}"
      _sig_sum="${_gitlab_sig_sum}"
      # Dvorak
      _evmfs_ns="0x87003Bd6C074C713783df04f36517451fF34CBEf"
      # Tallero
      _evmfs_ns="0x6ec7cC56dCeC0a00CB15E97C64B1a5Ec7A31403c"
    fi
  fi
fi
_evmfs_network="100"
_evmfs_address="0x69470b18f8b8b5f92b48f6199dcb147b4be96571"
_evmfs_dir="evmfs://${_evmfs_network}/${_evmfs_address}/${_evmfs_ns}"
_evmfs_uri="${_evmfs_dir}/${_sum}"
_evmfs_src="${_tarfile}::${_evmfs_uri}"
_sig_uri="${_evmfs_dir}/${_sig_sum}"
_sig_src="${_tarfile}.sig::${_sig_uri}"
if [[ "${_evmfs}" == "true" ]]; then
  _src="${_evmfs_src}"
  source+=(
    "${_sig_src}"
  )
  sha256sums+=(
    "${_sig_sum}"
  )
elif [[ "${_evmfs}" == "false" ]]; then
  if [[ "${_git}" == true ]]; then
    _src="${_tarname}::git+${_url}#${_tag_name}=${_tag}"
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
