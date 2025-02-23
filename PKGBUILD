# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2025  Pellegrino Prevete
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

# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Truocolo <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer: Pellegrino Prevete (dvorak) <pellegrinoprevete@gmail.com>
# Maintainer: Pellegrino Prevete (dvorak) <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Maintainer: Denis Falqueto <denisfalqueto@gmail.com>

_os="$( \
  uname \
    -o)"
if [[ "${_os}" == "GNU/Linux" ]]; then
  _expat="expat"
  _libc="gcc-libs"
elif [[ "${_os}" == "GNU/Linux" ]]; then
  _expat="libexpat"
  _libc="ndk-sysroot"
fi
_ml="lib32-"
_webview="false"
_gtk2="true"
_gtk3="true"
_sdl2="true"
_gl="false"
_compat28="true"
_Pkg="wxWidgets"
_pkg=wxwidgets
_majver="3.0"
_pkgbase="${_pkg}${_majver}"
pkgbase="${_ml}${_pkgbase}"
pkgname=(
  "${pkgbase}-common"
)
if [[ "${_gtk2}" == "true" ]]; then
  pkgname+=(
    "${pkgbase}-gtk2"
  )
fi
if [[ "${_gtk3}" == "true" ]]; then
  pkgname+=(
    "${pkgbase}-gtk3"
  )
fi
pkgver=3.0.5.1
pkgrel=1
_pkgdesc=(
  "wxWidgets API for GUI"
)
# _pkgdesc=("GTK+3 implementation of wxWidgets API for GUI"
arch=(
  'x86_64'
)
url="https://${_pkg}.org"
license=(
  'custom:wxWindows'
)
makedepends=(
  "${_ml}gst-plugins-base"
  "${_ml}libnotify"
  "${_ml}libsm"
)
if [[ "${_gtk2}" == "true" ]]; then
  makedepends+=(
    "${_ml}gtk2"
  )
fi
if [[ "${_gtk3}" == "true" ]]; then
  makedepends+=(
    "${_ml}gtk3"
  )
fi
if [[ "${_sdl2}" == "true" ]]; then
  makedepends+=(
    "${_ml}sdl2"
  )
fi
if [[ "${_gl}" == "false" ]]; then
  makedepends+=(
    "${_ml}glu"
  )
fi
optdepends=(
)
if [[ "${_webview}" == "true" ]]; then
  makedepends+=(
    "${_ml}webkit2gtk"
  )
elif [[ "${_webview}" == "false" ]]; then
  optdepends+=(
    "${_ml}webkit2gtk: for webview support"
  )
fi
_http="https://github.com"
_ns="${_Pkg}"
_url="${_http}/${_ns}/${_Pkg}"
_tarname="${_Pkg}-${pkgver}"
source=(
  "${_tarname}.tar.bz2::${_url}/releases/download/v${pkgver}/${_tarname}.tar.bz2"
  "${_pkg}-license.txt"
)
sha256sums=(
  '440f6e73cf5afb2cbf9af10cec8da6cdd3d3998d527598a53db87099524ac807'
  '5929947e6811ada59f65282a259ea167146977b8863b7371711a5d910e3b1d00'
)

_usr_get() {
  local \
    _bin
  _bin="$( \
    dirname \
      "$(command \
           -v \
	   "env")")"
  dirname \
    "${_bin}"
}

_build() {
  local \
    _gtk_ver="${1}" \
    _configure_opts=() \
    _build_dir \
    _gcc_opts=() \
    _cc \
    _cxx \
    _pkg_config_path \
    _lib \
    _oldpwd
  _oldpwd="$( \
    pwd)"
  _lib="$( \
    _usr_get)/lib32/${_pkgbase}"
  _pkg_config_path="${_lib}/pkgconfig"
  _gcc_opts=(
    -m32
  )
  _cc="gcc ${_gcc_opts[*]}"
  _cxx="g++ ${_gcc_opts[*]}"
  if [[ "${_gtk_ver}" != "" ]]; then
    _configure_opts+=(
      --with-gtk="${_gtk_ver}"
    )
    _build_dir="${_tarname}-gtk${_gtk_ver}"
  elif [[ "${_gtk_ver}" == "" ]]; then
    _build_dir="${_tarname}"
  fi
  cp \
    -a \
    "${_tarname}" \
    "${_build_dir}" || \
    true
  cd \
    "${_build_dir}"
  export \
    CC="${_cc}" \
    CXX="${_cxx}" \
    PKG_CONFIG_PATH="${_pkg_config_path}"
  _configure_opts+=(
    --prefix="/usr"
    --datadir="/usr/share/${_pkgbase}-32"
    --with-libpng="sys"
    --with-libxpm="sys"
    --with-libjpeg="sys"
    --with-libtiff="sys"
    --with-regex="builtin"
    --enable-mediactrl
    --enable-unicode
    --disable-precomp-headers
  )
  if [[ "${_webview}" == "true" ]]; then
    _configure_opts+=(
      --enable-webview
      --enable-webviewwebkit
    )
  elif [[ "${_webview}" == "false" ]]; then
    _configure_opts+=(
      --disable-webview
      --disable-webviewwebkit
    )
  fi
  if [[ "${_compat28}" == "true" ]]; then
    _configure_opts+=(
      --enable-compat28
    )
  elif [[ "${_compat28}" == "false" ]]; then
    _configure_opts+=(
      --disable-compat28
    )
  fi
  if [[ "${_gl}" == "true" ]]; then
    _configure_opts+=(
      --with-opengl
      --with-graphics_ctx
    )
  elif [[ "${_gl}" == "false" ]]; then
    _configure_opts+=(
      --without-opengl
    )
  fi
  if [[ "${_sdl2}" == "true" ]]; then
    _configure_opts+=(
      --with-sdl
    )
  fi
  ./configure \
    "${_configure_opts[@]}"
  pwd
  ls
  make
  if [[ "${_gtk_ver}" == "3" ]]; then
    make \
      -C \
        "locale" \
      allmo
  fi
  cd \
    "${_oldpwd}"
}

build() {
  if [[ "${_gtk2}" == "true" ]]; then
    _build \
      2
  fi
  if [[ "${_gtk3}" == "true" ]]; then
    _build \
      3
  fi
  if [[ "${_gtk2}" == "false" && \
	"${_gtk3}" == "false" ]]; then
    _build
  fi
}

package_lib32-wxwidgets3.0-common() {
  local \
    _lib
  _lib="$( \
    _usr_get)/lib32/${_pkgbase}"
  _pkgdesc=(
    'Common libraries and headers'
    'for wxgtk2 and wxgtk3, version 3.0.'
  )
  pkgdesc="${_pkgdesc[*]}"
  provides=(
    "${_pkg}-common=${pkgver}"
    "${_ml}${_pkg}-common-${_majver}"
    "${_ml}${_pkg}-common=${pkgver}"
  )
  depends=(
    "${_ml}${_expat}"
    "${_ml}${_libc}"
    "${_ml}zlib"
  )
  if [[ "${_gtk2}" == "false" && \
	"${_gtk3}" == "false" ]]; then
    _build_dir="${_tarname}"
  elif [[ "${_gtk2}" == "true" ]]; then
    _build_dir="${_tarname}-gtk2"
  elif [[ "${_gtk3}" == "true" ]]; then
    _build_dir="${_tarname}-gtk3"
  fi
  cd \
    "${_build_dir}"
  make \
    DESTDIR="${pkgdir}" \
    libdir="${_lib}" \
    install
  rm \
    -r \
    "${pkgdir}/usr/bin/wx-config" \
    "${pkgdir}${_lib}/wx" \
    "${pkgdir}${_lib}/libwx_gtk"* || \
    true
  mv \
    "${pkgdir}/usr/bin/wxrc" \
    "${pkgdir}/usr/bin/wxrc32" || \
    true
  install \
    -Dm644 \
    "${srcdir}/${_pkg}-license.txt" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

package_lib32-wxwidgets3.0-gtk2() {
  local \
    _lib
  _lib="$( \
    _usr_get)/lib32/${_pkgbase}"
  _pkgdesc=(
    'GTK+2 implementation of'
    'wxWidgets API for GUI, version 3.0.'
  )
  pkgdesc="${_pkgdesc[*]}"
  provides=(
    "${_pkg}-gtk2=${pkgver}"
    "${_ml}${_pkg}-gtk2-${_majver}"
    "${_ml}${_pkg}-gtk2=${pkgver}"
    "${_ml}wxgtk2=${pkgver}"
  )
  depends=(
    "${_ml}gtk2"
    "${_ml}gst-plugins-base-libs"
    "${_ml}libsm"
    "${_ml}libxxf86vm"
    "${pkgbase}-common"
    "${_ml}libnotify"
    "${_ml}libmspack"
  )
  if [[ "${_gl}" == "true" ]]; then
    depends+=(
      "${_ml}libgl"
    )
  fi
  cd \
    "${_tarname}-gtk2"
  make \
    DESTDIR="${pkgdir}" \
    libdir="${_lib}" \
    install
  rm \
    -r \
    "${pkgdir}/usr/include" \
    "${pkgdir}/usr/share" \
    "${pkgdir}${_lib}/libwx_base"* \
    "${pkgdir}/usr/bin/wxrc"* || \
    true
  mv \
    "${pkgdir}/usr/bin/wx-config" \
    "${pkgdir}/usr/bin/wx-config32-gtk2-3.0" || \
    true
  install \
    -Dm644 \
    "${srcdir}/${_pkg}-license.txt" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

package_lib32-wxwidgets3.0-gtk3() {
  local \
    _lib
  _lib="$( \
    _usr_get)/lib32/${_pkgbase}"
  _pkgdesc=(
    'GTK+3 implementation of'
    'wxWidgets API for GUI, version 3.0.'
  )
  pkgdesc="${_pkgdesc[*]}"
  provides=(
    "${_pkg}-gtk3=${pkgver}"
    "${_ml}${_pkg}-gtk3-${_majver}"
    "${_ml}${_pkg}-gtk3=${pkgver}"
    "${_ml}wxgtk=${pkgver}"
    "${_ml}wxgtk3=${pkgver}"
  )
  depends=(
    "${_ml}gtk3"
    "${_ml}gst-plugins-base-libs"
    "${_ml}libsm"
    "${_ml}libxxf86vm"
    "${pkgbase}-common"
    "${_ml}libnotify"
    "${_ml}libmspack"
  )
  cd \
    "${_tarname}-gtk3"
  make \
    DESTDIR="${pkgdir}" \
    libdir="${_lib}" \
    install
  rm \
    -r \
    "${pkgdir}/usr/include" \
    "${pkgdir}/usr/share" \
    "${pkgdir}${_lib}/libwx_base"* \
    "${pkgdir}/usr/bin/wxrc"* || \
    true
  mv \
    "${pkgdir}/usr/bin/wx-config" \
    "${pkgdir}/usr/bin/wx-config32-gtk3-3.0" || \
    true
  install \
    -Dm644 \
    "${srcdir}/${_pkg}-license.txt" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
