# Original Maintainers
# Maintainer: Laurent Carlier <lordheavym@gmail.com>
# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: Jan de Groot <jgc@archlinux.org>
# Contributor: Andreas Radke <andyrtr@archlinux.org>

pkgbase=handheld-mesa
pkgname=(
  'handheld-vulkan-mesa-layers'
  'handheld-opencl-clover-mesa'
  'handheld-opencl-rusticl-mesa'
  'handheld-vulkan-intel'
  'handheld-vulkan-nouveau'
  'handheld-vulkan-radeon'
  'handheld-vulkan-swrast'
  'handheld-vulkan-virtio'
  'handheld-libva-mesa-driver'
  'handheld-mesa-vdpau'
  'handheld-mesa'
)
pkgver=24.1.0
pkgrel=1
epoch=1
pkgdesc="Open-source OpenGL drivers"
url="https://www.mesa3d.org/"
arch=('x86_64')
license=('MIT AND BSD-3-Clause AND SGI-B-2.0')
makedepends=(
  'clang'
  'expat'
  'gcc-libs'
  'glibc'
  'libdrm'
  'libelf'
  'libglvnd'
  'libva'
  'libvdpau'
  'libx11'
  'libxcb'
  'libxext'
  'libxfixes'
  'libxml2'
  'libxrandr'
  'libxshmfence'
  'libxxf86vm'
  'llvm'
  'llvm-libs'
  'lm_sensors'
  'rust'
  'spirv-llvm-translator'
  'spirv-tools'
  'systemd-libs'
  'vulkan-icd-loader'
  'wayland'
  'xcb-util-keysyms'
  'zlib'
  'zstd'

  # shared between mesa and lib32-mesa
  'cbindgen'
  'clang'
  'cmake'
  'elfutils'
  'glslang'
  'libclc'
  'meson'
  'python-mako'
  'python-packaging'
  'python-ply'
  'rust-bindgen'
  'wayland-protocols'
  'xorgproto'

  # valgrind deps
  'valgrind'

  # d3d12 deps
  'directx-headers'

  # gallium-omx deps
  'libomxil-bellagio'
)
options=(
  # GCC 14 LTO causes segfault in LLVM under si_llvm_optimize_module
  !lto
)
source=(
  https://mesa.freedesktop.org/archive/mesa-${pkgver}.tar.xz{,.sig}
  LICENSE

  gnome-shell-glthread-disable.patch
  # https://gitlab.com/evlaV/mesa/
  valve.patch
)
validpgpkeys=(
  '8703B6700E7EE06D7A39B8D6EDAE37B02CEB490D'  # Emil Velikov <emil.l.velikov@gmail.com>
  '946D09B5E4C9845E63075FF1D961C596A7203456'  # Andres Gomez <tanty@igalia.com>
  'E3E8F480C52ADD73B278EE78E1ECBE07D7D70895'  # Juan Antonio Suárez Romero (Igalia, S.L.) <jasuarez@igalia.com>
  'A5CC9FEC93F2F837CB044912336909B6B25FADFA'  # Juan A. Suarez Romero <jasuarez@igalia.com>
  '71C4B75620BC75708B4BDB254C95FAAB3EB073EC'  # Dylan Baker <dylan@pnwbakers.com>
  '57551DE15B968F6341C248F68D8E31AFC32428A6'  # Eric Engestrom <eric@engestrom.ch>
)

# Rust crates for NVK, used as Meson subprojects
declare -A _crates=(
   paste          1.0.14
   proc-macro2    1.0.70
   quote          1.0.33
   syn            2.0.39
   unicode-ident  1.0.12
)

for _crate in "${!_crates[@]}"; do
  source+=($_crate-${_crates[$_crate]}.tar.gz::https://crates.io/api/v1/crates/$_crate/${_crates[$_crate]}/download)
done

sha256sums=('b7eac8c79244806b1c276eeeacc329e4a5b31a370804c4b0c7cd16837783f78b'
            'SKIP'
            '7052ba73bb07ea78873a2431ee4e828f4e72bda7d176d07f770fa48373dec537'
            '045c24124eea35342b1eb7638e84e4781a200eddc223ac2a3eb5a09e056c67ca'
            '3d57ed154fcb472ccd094c61178188ea0ec61983087ce2c1e02b093538e8a560'
            '39278fbbf5fb4f646ce651690877f89d1c5811a3d4acb27700c1cb3cdb78fd3b'
            '3354b9ac3fae1ff6755cb6db53683adb661634f67557942dea4facebec0fee4b'
            '5267fca4496028628a95160fc423a33e8b2e6af8a5302579e322e4b520293cae'
            'de3145af08024dea9fa9914f381a17b8fc6034dfb00f3a84013f7ff43f29ed4c'
            '23e78b90f2fcf45d3e842032ce32e3f2d1545ba6636271dcbf24fa306d87be7a')
b2sums=('1558d20d426162bfe8cccf96107ddbf1373c8322f87f48daec73e23b283e00f95d6efd073cad9b92065928af4b9b4a339ff2d204412070eca903f77ca366e619'
        'SKIP'
        '1ecf007b82260710a7bf5048f47dd5d600c168824c02c595af654632326536a6527fbe0738670ee7b921dd85a70425108e0f471ba85a8e1ca47d294ad74b4adb'
        '09792246c414433fd370732fba6d36561977ef18962c8853fec9cbb2c744d33eaaa8bb92f969710daa56c6cb852b6ed7b8c48bb07f4759d4e450eeb9363a94ad'
        '6fec5f5266b8b7b3528c827e1fc0dd3fcbd7b3e6c0e1bfbe881689b6048c35998f5b33299c14696ccb35a3c3d46b52427c1c0e2ffbb753b7d5b4a991ababd475'
        'fff0dec06b21e391783cc136790238acb783780eaedcf14875a350e7ceb46fdc100c8b9e3f09fb7f4c2196c25d4c6b61e574c0dad762d94533b628faab68cf5c'
        '4cede03c08758ccd6bf53a0d0057d7542dfdd0c93d342e89f3b90460be85518a9fd24958d8b1da2b5a09b5ddbee8a4263982194158e171c2bba3e394d88d6dac'
        '77c4b166f1200e1ee2ab94a5014acd334c1fe4b7d72851d73768d491c56c6779a0882a304c1f30c88732a6168351f0f786b10516ae537cff993892a749175848'
        '35e8548611c51ee75f4d04926149e5e54870d7073d9b635d550a6fa0f85891f57f326bdbcff3dd8618cf40f8e08cf903ef87d9c034d5921d8b91e1db842cdd7c'
        '2cff6626624d03f70f1662af45a8644c28a9f92e2dfe38999bef3ba4a4c1ce825ae598277e9cb7abd5585eebfb17b239effc8d0bbf1c6ac196499f0d288e5e01')

prepare() {
  cd mesa-$pkgver

  # Include package release in version string so Chromium invalidates
  # its GPU cache; otherwise it can cause pages to render incorrectly.
  # https://bugs.launchpad.net/ubuntu/+source/chromium-browser/+bug/2020604
  echo "$pkgver-arch$epoch.$pkgrel" >VERSION
  
  local src
  for src in "${source[@]}"; do
    src="${src%%::*}"
    src="${src##*/}"
    src="${src%.zst}"
    [[ $src = *.patch ]] || continue
    echo "----- Applying patch ./$src"
    # Allow going to reject path through vs code terminal
    patch -Np1 < "../$src" | sed -e "s/saving rejects to file /saving rejects to file ${PWD//\//\\/}\//g"
    if [ ${PIPESTATUS[0]} -ne 0 ]; then
      exit ${PIPESTATUS[0]}
    fi
  done
}

build() {
  local meson_options=(
    -D android-libbacktrace=disabled
    -D b_ndebug=true
    -D gallium-drivers=r300,r600,radeonsi,nouveau,virgl,svga,swrast,i915,iris,crocus,zink,d3d12
    -D gallium-extra-hud=true
    -D gallium-nine=true
    -D gallium-omx=bellagio
    -D gallium-opencl=icd
    -D gallium-rusticl=true
    -D gles1=disabled
    -D glx=dri
    -D intel-clc=enabled
    -D intel-rt=enabled
    -D libunwind=disabled
    -D microsoft-clc=disabled
    -D osmesa=true
    -D platforms=x11,wayland
    -D valgrind=enabled
    -D video-codecs=all
    -D vulkan-drivers=amd,intel,intel_hasvk,swrast,virtio,nouveau
    -D vulkan-layers=device-select,intel-nullhw,overlay
  )

  # Build only minimal debug info to reduce size
  CFLAGS+=' -g1'
  CXXFLAGS+=' -g1'

  # Inject subproject packages
  export MESON_PACKAGE_CACHE_DIR="$srcdir"

  arch-meson mesa-$pkgver build "${meson_options[@]}"
  meson configure build --no-pager # Print config
  meson compile -C build

  # fake installation to be seperated into packages
  # outside of fakeroot but mesa doesn't need to chown/mod
  meson install -C build --destdir "$srcdir/fakeinstall"
}

_install() {
  local src f dir
  for src; do
    f="${src#fakeinstall/}"
    dir="${pkgdir}/${f%/*}"
    install -m755 -d "${dir}"
    mv -v "${src}" "${dir}/"
  done
}

_libdir=usr/lib

package_handheld-vulkan-mesa-layers() {
  pkgdesc="Mesa's Vulkan layers"
  depends=(
    'gcc-libs'
    'glibc'
    'libdrm'
    'libxcb'
    'wayland'

    'python'
  )
  provides=('vulkan-mesa-layers')
  conflicts=('vulkan-mesa-layer' 'vulkan-mesa-layers')
  replaces=('vulkan-mesa-layer')

  _install fakeinstall/usr/share/vulkan/explicit_layer.d
  _install fakeinstall/usr/share/vulkan/implicit_layer.d
  _install fakeinstall/$_libdir/libVkLayer_*.so
  _install fakeinstall/usr/bin/mesa-overlay-control.py

  install -Dm644 mesa-$pkgver/docs/license.rst -t "$pkgdir/usr/share/licenses/$pkgname"
}

package_handheld-opencl-clover-mesa() {
  pkgdesc="Open-source OpenCL drivers - Clover variant"
  depends=(
    'clang'
    'expat'
    'gcc-libs'
    'glibc'
    'libdrm'
    'libelf'
    'llvm-libs'
    'spirv-llvm-translator'
    'spirv-tools'
    'zlib'
    'zstd'

    'libclc'
  )
  optdepends=('opencl-headers: headers necessary for OpenCL development')
  provides=('opencl-driver' 'opencl-clover-mesa')
  replaces=("opencl-mesa<=23.1.4-1")
  conflicts=('opencl-mesa' 'opencl-clover-mesa')

  _install fakeinstall/etc/OpenCL/vendors/mesa.icd
  _install fakeinstall/$_libdir/libMesaOpenCL*
  _install fakeinstall/$_libdir/gallium-pipe

  install -Dm644 mesa-$pkgver/docs/license.rst -t "$pkgdir/usr/share/licenses/$pkgname"
}

package_handheld-opencl-rusticl-mesa() {
  pkgdesc="Open-source OpenCL drivers - RustICL variant"
  depends=(
    'clang'
    'expat'
    'gcc-libs'
    'glibc'
    'libdrm'
    'libelf'
    'llvm-libs'
    'spirv-llvm-translator'
    'spirv-tools'
    'zlib'
    'zstd'

    'libclc'
  )
  optdepends=('opencl-headers: headers necessary for OpenCL development')
  provides=('opencl-driver' 'opencl-rusticl-mesa')
  replaces=("opencl-mesa<=23.1.4-1")
  conflicts=('opencl-mesa' 'opencl-rusticl-mesa')

  _install fakeinstall/etc/OpenCL/vendors/rusticl.icd
  _install fakeinstall/$_libdir/libRusticlOpenCL*

  install -Dm644 mesa-$pkgver/docs/license.rst -t "$pkgdir/usr/share/licenses/$pkgname"
}

package_handheld-vulkan-intel() {
  pkgdesc="Open-source Vulkan driver for Intel GPUs"
  depends=(
    'expat'
    'gcc-libs'
    'glibc'
    'libdrm'
    'libx11'
    'libxcb'
    'libxshmfence'
    'systemd-libs'
    'vulkan-icd-loader'
    'wayland'
    'xcb-util-keysyms'
    'zlib'
    'zstd'
  )
  optdepends=('vulkan-mesa-layers: additional vulkan layers')
  provides=('vulkan-driver' 'vulkan-intel')
  conflicts=('vulkan-intel')

  _install fakeinstall/usr/share/vulkan/icd.d/intel_*.json
  _install fakeinstall/$_libdir/libvulkan_intel*.so

  install -Dm644 mesa-$pkgver/docs/license.rst -t "$pkgdir/usr/share/licenses/$pkgname"
}

package_handheld-vulkan-nouveau() {
  pkgdesc="Open-source Vulkan driver for Nvidia GPUs"
  depends=(
    'expat'
    'gcc-libs'
    'glibc'
    'libdrm'
    'libx11'
    'libxcb'
    'libxshmfence'
    'systemd-libs'
    'vulkan-icd-loader'
    'wayland'
    'xcb-util-keysyms'
    'zlib'
    'zstd'
  )
  optdepends=('vulkan-mesa-layers: additional vulkan layers')
  provides=('vulkan-driver' 'vulkan-nouveau')
  conflicts=('vulkan-nouveau')

  _install fakeinstall/usr/share/vulkan/icd.d/nouveau_*.json
  _install fakeinstall/$_libdir/libvulkan_nouveau*.so

  install -Dm644 mesa-$pkgver/docs/license.rst -t "$pkgdir/usr/share/licenses/$pkgname"
}


package_handheld-vulkan-radeon() {
  pkgdesc="Open-source Vulkan driver for AMD GPUs"
  depends=(
    'expat'
    'gcc-libs'
    'glibc'
    'libdrm'
    'libelf'
    'libx11'
    'libxcb'
    'libxshmfence'
    'llvm-libs'
    'systemd-libs'
    'vulkan-icd-loader'
    'wayland'
    'xcb-util-keysyms'
    'zlib'
    'zstd'
  )
  optdepends=('vulkan-mesa-layers: additional vulkan layers')
  provides=('vulkan-driver' 'vulkan-radeon')
  conflicts=('vulkan-radeon')

  _install fakeinstall/usr/share/drirc.d/00-radv-defaults.conf
  _install fakeinstall/usr/share/vulkan/icd.d/radeon_icd*.json
  _install fakeinstall/$_libdir/libvulkan_radeon.so

  install -Dm644 mesa-$pkgver/docs/license.rst -t "$pkgdir/usr/share/licenses/$pkgname"
}

package_handheld-vulkan-swrast() {
  pkgdesc="Open-source Vulkan driver for CPUs (Software Rasterizer)"
  depends=(
    'expat'
    'gcc-libs'
    'glibc'
    'libdrm'
    'libx11'
    'libxcb'
    'libxshmfence'
    'llvm-libs'
    'systemd-libs'
    'vulkan-icd-loader'
    'wayland'
    'xcb-util-keysyms'
    'zlib'
    'zstd'
  )
  optdepends=('vulkan-mesa-layers: additional vulkan layers')
  conflicts=('vulkan-mesa' 'vulkan-swrast')
  replaces=('vulkan-mesa')
  provides=('vulkan-driver' 'vulkan-swrast')

  _install fakeinstall/usr/share/vulkan/icd.d/lvp_icd*.json
  _install fakeinstall/$_libdir/libvulkan_lvp.so

  install -Dm644 mesa-$pkgver/docs/license.rst -t "$pkgdir/usr/share/licenses/$pkgname"
}

package_handheld-vulkan-virtio() {
  pkgdesc="Open-source Vulkan driver for Virtio-GPU (Venus)"
  depends=(
    'expat'
    'gcc-libs'
    'glibc'
    'libdrm'
    'libx11'
    'libxcb'
    'libxshmfence'
    'systemd-libs'
    'vulkan-icd-loader'
    'wayland'
    'xcb-util-keysyms'
    'zlib'
    'zstd'
  )
  optdepends=('vulkan-mesa-layers: additional vulkan layers')
  provides=('vulkan-driver' 'vulkan-virtio')
  conflicts=('vulkan-virtio')

  _install fakeinstall/usr/share/vulkan/icd.d/virtio_icd*.json
  _install fakeinstall/$_libdir/libvulkan_virtio.so

  install -Dm644 mesa-$pkgver/docs/license.rst -t "$pkgdir/usr/share/licenses/$pkgname"
}

package_handheld-libva-mesa-driver() {
  pkgdesc="Open-source VA-API drivers"
  depends=(
    'expat'
    'gcc-libs'
    'glibc'
    'libdrm'
    'libelf'
    'libx11'
    'libxcb'
    'libxshmfence'
    'llvm-libs'
    'zlib'
    'zstd'
  )
  provides=('libva-driver' 'libva-mesa-driver')
  conflicts=('libva-mesa-driver')

  _install fakeinstall/$_libdir/dri/*_drv_video.so

  install -Dm644 mesa-$pkgver/docs/license.rst -t "$pkgdir/usr/share/licenses/$pkgname"
}

package_handheld-mesa-vdpau() {
  pkgdesc="Open-source VDPAU drivers"
  depends=(
    'expat'
    'gcc-libs'
    'glibc'
    'libdrm'
    'libelf'
    'libx11'
    'libxcb'
    'libxshmfence'
    'llvm-libs'
    'zlib'
    'zstd'
  )
  provides=('vdpau-driver' 'mesa-vdpau')
  conflicts=('mesa-vdpau')

  _install fakeinstall/$_libdir/vdpau

  install -Dm644 mesa-$pkgver/docs/license.rst -t "$pkgdir/usr/share/licenses/$pkgname"
}

package_handheld-mesa() {
  depends=(
    'expat'
    'gcc-libs'
    'glibc'
    'libdrm'
    'libelf'
    'libglvnd'
    'libx11'
    'libxcb'
    'libxext'
    'libxfixes'
    'libxshmfence'
    'libxxf86vm'
    'llvm-libs'
    'lm_sensors'
    'wayland'
    'zlib'
    'zstd'

    'libomxil-bellagio'
  )
  optdepends=(
    'opengl-man-pages: for the OpenGL API man pages'
  )
  provides=(
    'mesa'
    'mesa-libgl'
    'opengl-driver'
  )
  conflicts=('mesa-libgl' 'mesa')
  replaces=('mesa-libgl')

  _install fakeinstall/usr/share/drirc.d/00-mesa-defaults.conf
  _install fakeinstall/usr/share/glvnd/egl_vendor.d/50_mesa.json

  # ati-dri, nouveau-dri, intel-dri, svga-dri, swrast, swr
  _install fakeinstall/$_libdir/dri/*_dri.so

  _install fakeinstall/$_libdir/bellagio
  _install fakeinstall/$_libdir/d3d
  _install fakeinstall/$_libdir/lib{gbm,glapi}.so*
  _install fakeinstall/$_libdir/libOSMesa.so*
  _install fakeinstall/$_libdir/libxatracker.so*

  _install fakeinstall/usr/include
  _install fakeinstall/$_libdir/pkgconfig

  # libglvnd support
  _install fakeinstall/$_libdir/libGLX_mesa.so*
  _install fakeinstall/$_libdir/libEGL_mesa.so*

  # indirect rendering
  ln -sr "$pkgdir"/$_libdir/libGLX_{mesa,indirect}.so.0

  # make sure there are no files left to install
  find fakeinstall -depth -print0 | xargs -0 rmdir

  install -Dm644 mesa-$pkgver/docs/license.rst -t "$pkgdir/usr/share/licenses/$pkgname"
}

# vim:set sw=2 sts=-1 et:
