# even FURTHER modified PKGBUILD based on gamescope-git + mesa-git
# Maintainer - Matthew Schwartz <matthew.schwartz@linux.dev>

# PKGBUILD based on the official Arch gamescope PKGBUILD
# Maintainer - Sid Pranjale <sidpranjale127@protonmail.com>

# Maintainer: Sefa Eyeoglu <contact@scrumplex.net>
# Maintainer: Bouke Sybren Haarsma <boukehaarsma23 at gmail dot com>
# Maintainer: Maxime Gauduin <alucryd@archlinux.org>
# Maintainer: Giancarlo Razzolini <grazzolini@archlinux.org>
# Contributor: Samuel "scrufulufugus" Monson <smonson@irbash.net>
# Contributor: PedroHLC <root@pedrohlc.com>

_lib32=true  # Toggle 32-bit WSI layer build.

pkgbase="gamescope-git"
pkgver=3.16.1.r38.gef1e8dbe
pkgrel=1
pkgdesc="SteamOS session compositing window manager (64-bit) and optional 32-bit WSI layer"
arch=('x86_64')
url="https://github.com/ValveSoftware/gamescope"
license=('BSD-2-Clause')

# If _lib32=true, we produce two packages. Otherwise, just 'gamescope-git'.
pkgname=('gamescope-git')
if [ "$_lib32" == "true" ]; then
  pkgname+=('lib32-gamescope-git')
fi

##############################################################################
# Dependencies
##############################################################################
_common_makedepends=(
  'benchmark'
  'cmake'
  'git'
  'glslang'
  'meson'
  'ninja'
  'vulkan-headers'
  'wayland-protocols'
)

# For 32-bit cross-compile:
_lib32_makedepends=(
  'gcc-multilib'
  'lib32-glm'
)

makedepends=("${_common_makedepends[@]}")
if [ "$_lib32" == "true" ]; then
  makedepends+=("${_lib32_makedepends[@]}")
fi

source=(
  "git+https://github.com/ValveSoftware/gamescope.git"              # $srcdir/gamescope
  "git+https://github.com/Joshua-Ashton/wlroots.git"                # $srcdir/wlroots
  "git+https://gitlab.freedesktop.org/emersion/libliftoff.git"      # $srcdir/libliftoff
  "git+https://github.com/Joshua-Ashton/vkroots.git"                # $srcdir/vkroots
  "git+https://gitlab.freedesktop.org/emersion/libdisplay-info.git" # $srcdir/libdisplay-info
  "git+https://github.com/ValveSoftware/openvr.git"                 # $srcdir/openvr
  "git+https://github.com/Joshua-Ashton/reshade.git"                # $srcdir/reshade
  "git+https://github.com/Joshua-Ashton/GamescopeShaders.git#tag=v0.1" # $srcdir/GamescopeShaders
  "git+https://github.com/KhronosGroup/SPIRV-Headers.git"           # $srcdir/SPIRV-Headers
)
b2sums=('SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP')

##############################################################################
# Package: gamescope-git (64-bit)
##############################################################################
# We'll assign final depends/conflicts/provides in the split-package function
# or define them in "package_gamescope-git()".
##############################################################################

##############################################################################
# Package: lib32-gamescope-git (32-bit WSI layer)
##############################################################################
# Only if _lib32=true
##############################################################################
# We'll define the function unconditionally, but it only runs if
# the user actually requested it (or if _lib32=true and they build all).
##############################################################################

##############################################################################
# Functions
##############################################################################

prepare() {
  cd "$srcdir/gamescope"

  # Add custom patches if needed
  for src in "${source[@]}"; do
      src="${src%%::*}"
      src="${src##*/}"
      [[ $src = *.patch ]] || continue
      echo "Applying patch $src..."
      git apply -v "../$src"
  done

  meson subprojects download

  git submodule init subprojects/wlroots
  git config submodule.subprojects/wlroots.url ../wlroots

  git submodule init subprojects/libliftoff
  git config submodule.subprojects/libliftoff.url ../libliftoff

  git submodule init subprojects/vkroots
  git config submodule.subprojects/vkroots.url ../vkroots

  git submodule init subprojects/libdisplay-info
  git config submodule.subprojects/libdisplay-info.url ../libdisplay-info

  git submodule init subprojects/openvr
  git config submodule.subprojects/openvr.url ../openvr

  git submodule init src/reshade
  git config submodule.src/reshade.url ../reshade

  git submodule init thirdparty/SPIRV-Headers
  git config submodule.thirdparty/SPIRV-Headers.url ../SPIRV-Headers

  git -c protocol.file.allow=always submodule update
}

pkgver() {
  cd "$srcdir/gamescope"
  git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

build() {
  # 1) 64-bit gamescope build
  msg2 "Building 64-bit gamescope..."
  rm -rf "$srcdir"/build64
  meson setup "$srcdir/build64" "$srcdir/gamescope" \
    -Dforce_fallback_for=stb,wlroots,vkroots,libliftoff,glm,libdisplay-info \
    --auto-features=enabled \
    --prefix=/usr \
    --buildtype=release
  meson compile -C "$srcdir/build64"

  # 2) If enabled, build 32-bit WSI-only version
  if [ "$_lib32" == "true" ]; then
    msg2 "Building 32-bit WSI layer (lib32-gamescope)..."
    rm -rf "$srcdir"/build32

    export CC="gcc -m32"
    export CXX="g++ -m32"
    export PKG_CONFIG="i686-pc-linux-gnu-pkg-config"

    meson setup "$srcdir/build32" "$srcdir/gamescope" \
      --libdir=/usr/lib32 \
      -Denable_gamescope=false \
      -Denable_gamescope_wsi_layer=true \
      -Denable_openvr_support=false \
      -Dpipewire=disabled \
      --buildtype=release \
      --prefix=/usr

    meson compile -C "$srcdir/build32"
  fi
}

##############################################################################
# Package: gamescope-git
##############################################################################
package_gamescope-git() {
  depends=(
    'gcc-libs'
    'glibc'
    'glm'
    'hwdata'
    'lcms2'
    'libavif'
    'libcap.so'
    'libdecor'
    'libdrm'
    'libinput'
    'libpipewire-0.3.so'
    'libx11'
    'libxcb'
    'libxcomposite'
    'libxdamage'
    'libxext'
    'libxfixes'
    'libxkbcommon'
    'libxmu'
    'libxrender'
    'libxres'
    'libxtst'
    'libxxf86vm'
    'luajit'
    'seatd'
    'sdl2'
    'vulkan-icd-loader'
    'wayland'
    'xcb-util-wm'
    'xcb-util-errors'
    'xorg-server-xwayland'
  )
  provides=('gamescope')
  conflicts=('gamescope')

  DESTDIR="$pkgdir" meson install -C "$srcdir/build64" --skip-subprojects

  install -d "$pkgdir/usr/share/gamescope/reshade"
  cp -r "$srcdir/GamescopeShaders/"* "$pkgdir/usr/share/gamescope/reshade/"
  chmod -R 755 "$pkgdir/usr/share/gamescope"

  install -Dm644 "$srcdir/gamescope/LICENSE" \
    "$pkgdir/usr/share/licenses/gamescope-git/LICENSE"
}

##############################################################################
# Package: lib32-gamescope-git
##############################################################################
if [ "$_lib32" == "true" ]; then
package_lib32-gamescope-git() {
  depends=(
    'lib32-wayland'
    'lib32-libx11'
    'lib32-libxcb'
    'lib32-vulkan-icd-loader'
  )
  provides=('lib32-gamescope')
  conflicts=('lib32-gamescope')

  DESTDIR="$pkgdir" meson install -C "$srcdir/build32" --skip-subprojects

  # Remove all the non-WSI bits
  rm -rf "$pkgdir/usr/share/gamescope" \
         "$pkgdir/usr/include" \
         "$pkgdir/usr/lib/libwlroots"* \
         "$pkgdir/usr/lib32/libwlroots"* \
         "$pkgdir/usr/lib/pkgconfig" \
         "$pkgdir/usr/lib32/pkgconfig"

  install -Dm644 "$srcdir/gamescope/LICENSE" \
    "$pkgdir/usr/share/licenses/lib32-gamescope-git/LICENSE"
}
fi

# vim: ts=2 sw=2 et:
