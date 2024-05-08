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
  'handheld-vulkan-radeon'
  'handheld-vulkan-swrast'
  'handheld-vulkan-virtio'
  'handheld-libva-mesa-driver'
  'handheld-mesa-vdpau'
  'handheld-mesa'
)
pkgver=24.0.6
pkgrel=1
epoch=1
pkgdesc="An open-source implementation of the OpenGL specification"
url="https://www.mesa3d.org/"
arch=('x86_64')
license=('MIT AND BSD-3-Clause AND SGI-B-2.0')
makedepends=(
  'clang'
  'expat'
  'libdrm'
  'libelf'
  'libglvnd'
  'libunwind'
  'libva'
  'libvdpau'
  'libx11'
  'libxdamage'
  'libxml2'
  'libxrandr'
  'libxshmfence'
  'libxxf86vm'
  'llvm'
  'lm_sensors'
  'rust'
  'spirv-llvm-translator'
  'spirv-tools'
  'systemd'
  'vulkan-icd-loader'
  'wayland'
  'xcb-util-keysyms'
  'zstd'

  # shared between mesa and lib32-mesa
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
source=(
  https://mesa.freedesktop.org/archive/mesa-${pkgver}.tar.xz{,.sig}
  LICENSE

  gnome-shell-glthread-disable.patch
  # https://gitlab.com/evlaV/mesa/
  valve.patch

  # Fix chrome acceleration on AMD (Z1E etc)
  # Needs extra flags and only works under xwayland
  # --use-gl=angle
  # --enable-features=Vulkan,DefaultANGLEVulkan,VulkanFromANGLE,VaapiVideoDecoder,VaapiIgnoreDriverChecks
  # https://gitlab.freedesktop.org/mesa/mesa/-/merge_requests/26165
  amd-fix-chrome-accel.patch
)
sha256sums=('8b7a92dbe6468c18f2383700135b5fe9de836cdf0cc8fd7dbae3c7110237d604'
            'SKIP'
            '7052ba73bb07ea78873a2431ee4e828f4e72bda7d176d07f770fa48373dec537'
            '045c24124eea35342b1eb7638e84e4781a200eddc223ac2a3eb5a09e056c67ca'
            '3d57ed154fcb472ccd094c61178188ea0ec61983087ce2c1e02b093538e8a560'
            '1227006afd3524a219bde1f8443b9010639b1387f46bc2fb467892d151637ea3')
b2sums=('531482c1a01ec4c0bd6b2f0857640c14fcc2ba70e85a14e4edf0f44ba466c12e39ad067690e6a571a0744ae06080943b1328a29263225cb7570f119464e17fbe'
        'SKIP'
        '1ecf007b82260710a7bf5048f47dd5d600c168824c02c595af654632326536a6527fbe0738670ee7b921dd85a70425108e0f471ba85a8e1ca47d294ad74b4adb'
        '09792246c414433fd370732fba6d36561977ef18962c8853fec9cbb2c744d33eaaa8bb92f969710daa56c6cb852b6ed7b8c48bb07f4759d4e450eeb9363a94ad'
        '6fec5f5266b8b7b3528c827e1fc0dd3fcbd7b3e6c0e1bfbe881689b6048c35998f5b33299c14696ccb35a3c3d46b52427c1c0e2ffbb753b7d5b4a991ababd475'
        '7f3f7434d61327823921b37ccb7d3a987f391622a2fd02c9c3816931285780df1fb84111736b40cddb478239ff4a13b47ef76b8c1ab37dab19292b36c6387aa4')
validpgpkeys=('8703B6700E7EE06D7A39B8D6EDAE37B02CEB490D'  # Emil Velikov <emil.l.velikov@gmail.com>
              '946D09B5E4C9845E63075FF1D961C596A7203456'  # Andres Gomez <tanty@igalia.com>
              'E3E8F480C52ADD73B278EE78E1ECBE07D7D70895'  # Juan Antonio Suárez Romero (Igalia, S.L.) <jasuarez@igalia.com>
              'A5CC9FEC93F2F837CB044912336909B6B25FADFA'  # Juan A. Suarez Romero <jasuarez@igalia.com>
              '71C4B75620BC75708B4BDB254C95FAAB3EB073EC'  # Dylan Baker <dylan@pnwbakers.com>
              '57551DE15B968F6341C248F68D8E31AFC32428A6') # Eric Engestrom <eric@engestrom.ch>

prepare() {
  cd mesa-$pkgver

  # Include package release in version string so Chromium invalidates
  # its GPU cache; otherwise it can cause pages to render incorrectly.
  # https://bugs.launchpad.net/ubuntu/+source/chromium-browser/+bug/2020604
  echo "$pkgver-arch$epoch.$pkgrel" > VERSION
  
  local src
  for src in "${source[@]}"; do
    src="${src%%::*}"
    src="${src##*/}"
    src="${src%.zst}"
    [[ $src = *.patch ]] || continue
    echo "----- Applying patch $src..."
    patch -Np1 < "../$src"
  done
}

build() {
  local meson_options=(
    -D android-libbacktrace=disabled
    -D b_ndebug=true
    -D dri3=enabled
    -D egl=enabled
    -D gallium-drivers=r300,r600,radeonsi,nouveau,virgl,svga,swrast,i915,iris,crocus,zink,d3d12
    -D gallium-extra-hud=true
    -D gallium-nine=true
    -D gallium-omx=bellagio
    -D gallium-opencl=icd
    -D gallium-rusticl=true
    -D gallium-va=enabled
    -D gallium-vdpau=enabled
    -D gallium-xa=enabled
    -D gbm=enabled
    -D gles1=disabled
    -D gles2=enabled
    -D glvnd=true
    -D glx=dri
    -D intel-clc=enabled
    -D libunwind=enabled
    -D llvm=enabled
    -D lmsensors=enabled
    -D microsoft-clc=disabled
    -D osmesa=true
    -D platforms=x11,wayland
    -D rust_std=2021
    -D shared-glapi=enabled
    -D valgrind=enabled
    -D video-codecs=all
    -D vulkan-drivers=amd,intel,intel_hasvk,swrast,virtio
    -D vulkan-layers=device-select,intel-nullhw,overlay
  )

  # Build only minimal debug info to reduce size
  CFLAGS+=' -g1'
  CXXFLAGS+=' -g1'

  arch-meson mesa-$pkgver build "${meson_options[@]}"
  meson configure build # Print config
  meson compile -C build

  # fake installation to be seperated into packages
  # outside of fakeroot but mesa doesn't need to chown/mod
  DESTDIR="${srcdir}/fakeinstall" meson install -C build
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

  install -m644 -Dt "${pkgdir}/usr/share/licenses/${pkgname}" LICENSE
}

package_handheld-opencl-clover-mesa() {
  pkgdesc="OpenCL support with clover for mesa drivers"
  depends=(
    'clang'
    'expat'
    'libdrm'
    'libelf'
    'spirv-llvm-translator'
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

  install -m644 -Dt "${pkgdir}/usr/share/licenses/${pkgname}" LICENSE
}

package_handheld-opencl-rusticl-mesa() {
  pkgdesc="OpenCL support with rusticl for mesa drivers"
  depends=(
    'clang'
    'expat'
    'libdrm'
    'libelf'
    'lm_sensors'
    'spirv-llvm-translator'
    'zstd'

    'libclc'
  )
  optdepends=('opencl-headers: headers necessary for OpenCL development')
  provides=('opencl-driver' 'opencl-rusticl-mesa')
  replaces=("opencl-mesa<=23.1.4-1")
  conflicts=('opencl-mesa' 'opencl-rusticl-mesa')

  _install fakeinstall/etc/OpenCL/vendors/rusticl.icd
  _install fakeinstall/$_libdir/libRusticlOpenCL*

  install -m644 -Dt "${pkgdir}/usr/share/licenses/${pkgname}" LICENSE
}

package_handheld-vulkan-intel() {
  pkgdesc="Intel's Vulkan mesa driver"
  depends=(
    'libdrm'
    'libx11'
    'libxshmfence'
    'systemd'
    'wayland'
    'xcb-util-keysyms'
    'zstd'
  )
  optdepends=('vulkan-mesa-layers: additional vulkan layers')
  provides=('vulkan-driver' 'vulkan-intel')
  conflicts=('vulkan-intel')

  _install fakeinstall/usr/share/vulkan/icd.d/intel_*.json
  _install fakeinstall/$_libdir/libvulkan_intel*.so

  install -m644 -Dt "${pkgdir}/usr/share/licenses/${pkgname}" LICENSE
}

package_handheld-vulkan-radeon() {
  pkgdesc="Radeon's Vulkan mesa driver"
  depends=(
    'libdrm'
    'libelf'
    'libx11'
    'libxshmfence'
    'llvm-libs'
    'systemd'
    'wayland'
    'xcb-util-keysyms'
    'zstd'
  )
  optdepends=('vulkan-mesa-layers: additional vulkan layers')
  provides=('vulkan-driver' 'vulkan-radeon')
  conflicts=('vulkan-radeon')

  _install fakeinstall/usr/share/drirc.d/00-radv-defaults.conf
  _install fakeinstall/usr/share/vulkan/icd.d/radeon_icd*.json
  _install fakeinstall/$_libdir/libvulkan_radeon.so

  install -m644 -Dt "${pkgdir}/usr/share/licenses/${pkgname}" LICENSE
}

package_handheld-vulkan-swrast() {
  pkgdesc="Vulkan software rasteriser driver"
  depends=(
    'libdrm'
    'libunwind'
    'libx11'
    'libxshmfence'
    'llvm-libs'
    'systemd'
    'wayland'
    'xcb-util-keysyms'
    'zstd'
  )
  optdepends=('vulkan-mesa-layers: additional vulkan layers')
  conflicts=('vulkan-mesa' 'vulkan-swrast')
  replaces=('vulkan-mesa')
  provides=('vulkan-driver' 'vulkan-swrast')

  _install fakeinstall/usr/share/vulkan/icd.d/lvp_icd*.json
  _install fakeinstall/$_libdir/libvulkan_lvp.so

  install -m644 -Dt "${pkgdir}/usr/share/licenses/${pkgname}" LICENSE
}

package_handheld-vulkan-virtio() {
  pkgdesc="Venus Vulkan mesa driver for Virtual Machines"
  depends=(
    'libdrm'
    'libx11'
    'libxshmfence'
    'systemd'
    'wayland'
    'xcb-util-keysyms'
    'zstd'
  )
  optdepends=('vulkan-mesa-layers: additional vulkan layers')
  provides=('vulkan-driver' 'vulkan-virtio')
  conflicts=('vulkan-virtio')

  _install fakeinstall/usr/share/vulkan/icd.d/virtio_icd*.json
  _install fakeinstall/$_libdir/libvulkan_virtio.so

  install -m644 -Dt "${pkgdir}/usr/share/licenses/${pkgname}" LICENSE
}

package_handheld-libva-mesa-driver() {
  pkgdesc="VA-API drivers"
  depends=(
    'expat'
    'libdrm'
    'libelf'
    'libx11'
    'libxshmfence'
    'llvm-libs'
    'zstd'
  )
  provides=('libva-driver' 'libva-mesa-driver')
  conflicts=('libva-mesa-driver')

  _install fakeinstall/$_libdir/dri/*_drv_video.so

  install -m644 -Dt "${pkgdir}/usr/share/licenses/${pkgname}" LICENSE
}

package_handheld-mesa-vdpau() {
  pkgdesc="VDPAU drivers"
  depends=(
    'expat'
    'libdrm'
    'libelf'
    'libx11'
    'libxshmfence'
    'llvm-libs'
    'zstd'
  )
  provides=('vdpau-driver' 'mesa-vdpau')
  conflicts=('mesa-vdpau')

  _install fakeinstall/$_libdir/vdpau

  install -m644 -Dt "${pkgdir}/usr/share/licenses/${pkgname}" LICENSE
}

package_handheld-mesa() {
  depends=(
    'libdrm'
    'libelf'
    'libglvnd'
    'libunwind'
    'libxdamage'
    'libxshmfence'
    'libxxf86vm'
    'llvm-libs'
    'lm_sensors'
    'vulkan-icd-loader'
    'wayland'
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

  install -m644 -Dt "${pkgdir}/usr/share/licenses/${pkgname}" LICENSE
}
