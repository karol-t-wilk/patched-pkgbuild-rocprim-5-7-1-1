# Maintainer: Torsten Keßler <tpkessler at archlinux dot org>
# Contributor: Markus Näther <naetherm@informatik.uni-freiburg.de>
pkgname=rocprim-patched
pkgver=5.7.1
pkgrel=1
pkgdesc='Header-only library providing HIP parallel primitives'
arch=('x86_64')
url='https://codedocs.xyz/ROCmSoftwarePlatform/rocPRIM'
_git='https://github.com/ROCmSoftwarePlatform/rocPRIM'
_patchhash='ab4070ad08cc2ab10bdfdbd54efe6c42d8003f6d'
_patch="https://github.com/karol-t-wilk/rocm-gfx10xx-patch/commit/${_patchhash}.patch"
license=('MIT')
depends=('hip')
provides=('rocprim=5.7.1-1')
conflicts=('rocprim=5.7.1-1')
makedepends=('rocm-cmake')
source=("$_git/archive/rocm-$pkgver.tar.gz"
"$_patch")
sha256sums=('15d820a0f61aed60efbba88b6efe6942878b02d912f523f9cf8f33a4583d6cd7' '2c6caa236613d376dad9b2e411fc39c1cc5e9b694c10012fb6f8e297e3d30c79')
_dirname="$(basename "$_git")-$(basename "${source[0]}" ".tar.gz")"

prepare() {
	# apply DPP patch
	patch -d "$_dirname" -p1 -i "../${_patchhash}.patch"
}

build() {
  # -fcf-protection is not supported by HIP, see
  # https://rocm.docs.amd.com/en/latest/reference/rocmcc/rocmcc.html#support-status-of-other-clang-options

  CXXFLAGS="${CXXFLAGS} -fcf-protection=none" \
  cmake \
    -Wno-dev \
    -S "$_dirname" \
    -B build \
    -DCMAKE_BUILD_TYPE=None \
    -DCMAKE_CXX_COMPILER=/opt/rocm/bin/hipcc \
    -DCMAKE_INSTALL_PREFIX=/opt/rocm \
    -Damd_comgr_DIR=/opt/rocm/lib/cmake/amd_comgr
  cmake --build build
}

package() {
  DESTDIR="$pkgdir" cmake --install build

  install -Dm644 "$_dirname/LICENSE.txt" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
