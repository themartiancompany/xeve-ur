# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Daniel Bermond <dbermond@archlinux.org>

_os="$( \
  uname \
    -o)"
if [[ "${_os}" == "GNU/Linux" ]]; then
  _libc="glibc"
elif [[ "${_os}" == "Android" ]]; then
  _libc="ndk-sysroot"
fi
_proj="mpeg5"
_pkg=xeve
pkgname="${_pkg}"
pkgver=0.5.1
pkgrel=1
pkgdesc='MPEG-5 EVC (Essential Video Coding) encoder'
arch=(
  'x86_64'
  'arm'
  'aarch64'
  'i686'
  'armv7l'
  'armv6l'
  'mips'
  'pentium4'
  'powerpc'
)
_http="https://github.com"
_ns="${_proj}"
url="${_http}/${_ns}/${_pkg}"
license=(
  'BSD-3-Clause'
)
depends=(
  "${_libc}"
)
makedepends=(
  'cmake'
)
options=(
  '!emptydirs'
)
_patches=(
  "010-${_pkg}-disable-werror.patch"
  "020-${_pkg}-fix-pkg-config.patch"
)
source=(
  "${url}/archive/v${pkgver}/${pkgname}-${pkgver}.tar.gz"
  "${_patches[@]}"
)
sha256sums=(
  '238c95ddd1a63105913d9354045eb329ad9002903a407b5cf1ab16bad324c245'
  '8c4b607f34a5d39e824f86d00ab101849595cb49a2f67eed131487d658ec7206'
  '68ae77132ec2b3dd8de641d16f3d7cc0de819ddb116484809445666b4d215187'
)

prepare() {
  local \
    _patch
  printf \
    '%s\n' \
    "v${pkgver}" > \
    "${_pkg}-${pkgver}/version.txt"
  for _patch in "${_patches[@]}"; do
  patch \
    -d \
      "${_pkg}-${pkgver}" \
    -Np1 \
    -i \
    "${srcdir}/${_patch}"
  done
}

build() {
  local \
    _cmake_opts=()
  _cmake_opts=(
    -DCMAKE_BUILD_TYPE:STRING='None'
    -DCMAKE_INSTALL_PREFIX:PATH='/usr'
    -Wno-dev
  )
  # https://github.com/mpeg5/xeve/issues/108
  export \
    CFLAGS+=' -mno-avx'
  cmake \
    -B \
      "build" \
    -S \
      "${_pkg}-${pkgver}" \
    -G \
      'Unix Makefiles' \
    "${_cmake_opts[@]}"
  cmake \
    --build \
    build
}

package() {
  DESTDIR="${pkgdir}" \
  cmake \
    --install \
      "build"
  install \
    -Dm644 \
    "${_pkg}-${pkgver}/COPYING" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  rm \
    "${pkgdir}/usr/lib/${_pkg}/lib${_pkg}.a"
}
