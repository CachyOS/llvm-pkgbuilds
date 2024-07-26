# Maintainer: Frederik Schwan <freswa at archlinux dot org>

pkgname=wasi-libc
pkgver=0+389+3f43ea9a
_commit=3f43ea9abb24ed8d24d760989e1d87ea385f8eaa  # tags/wasi-sdk-23
pkgrel=1
epoch=1
pkgdesc='WASI libc implementation for WebAssembly'
url='https://github.com/WebAssembly/wasi-libc'
arch=('any')
license=('Apache-2.0 WITH LLVM-exception AND Apache-2.0 AND MIT')
makedepends=(
  'clang'
  'git'
  'llvm'
)
options=('staticlibs')
source=("git+https://github.com/WebAssembly/wasi-libc.git#commit=${_commit}")
b2sums=('8d011e0b4b5abe80dfbc312b50a9f9fc8799d2443bab12cc24ddafbd582c0bf30638d38a9c9e167626f0b3c2f0e11adfc625999543f95a32c48a00a70ac664df')

pkgver() {
  cd ${pkgname}
  printf "0+%s+%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build() {
  local target= make_options=(
    WASM_CC=/usr/bin/clang
    WASM_AR=/usr/bin/llvm-ar
    WASM_NM=/usr/bin/llvm-nm
  )
  local -A targets=(
    wasm32-wasi           ''
    wasm32-wasip1         ''
    wasm32-wasip1-threads 'THREAD_MODEL=posix'
    wasm32-wasip2         'WASI_SNAPSHOT=p2'
  )

  cd ${pkgname}

  for target in "${!targets[@]}"; do
    make "${make_options[@]}" TARGET_TRIPLE="$target" ${targets[$target]}
  done
}

package() {
  cd ${pkgname}
  install -dm755 "${pkgdir}"/usr/share
  cp -dr --preserve=mode,timestamp sysroot "${pkgdir}"/usr/share/wasi-sysroot
  install -Dm644 LICENSE* -t "${pkgdir}"/usr/share/licenses/${pkgname}
}

# vim:set sw=2 sts=-1 et:
