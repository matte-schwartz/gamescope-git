# Maintainer: Pierre-Loup A. Griffais <pgriffais@valvesoftware.com>
# Maintainer: Matthew Schwartz <matthew.schwartz@linux.dev>
# Maintainer: Sefa Eyeoglu <contact@scrumplex.net>
# Maintainer: Bouke Sybren Haarsma <boukehaarsma23 at gmail dot com>
# Maintainer: Maxime Gauduin <alucryd@archlinux.org>
# Maintainer: Giancarlo Razzolini <grazzolini@archlinux.org>
# Contributor: Samuel "scrufulufugus" Monson <smonson@irbash.net>
# Contributor: PedroHLC <root@pedrohlc.com>
# Contributor: Sid Pranjale <sidpranjale127@protonmail.com>

_lib32=true  # Toggle 32-bit WSI layer build. Set to "false" if you don't need lib32.

pkgbase="gamescope-git"
pkgname=("gamescope-git")
if [ "$_lib32" == "true" ]; then
  pkgname+=("lib32-gamescope-git")
fi

pkgdesc="Gaming shell (compositing Wayland/X11 window manager) from Valve, with SteamOS session files, built from git."
pkgver=0.0.0.r0.g00000000  # Will be auto-set by pkgver() function.
pkgrel=1
arch=('x86_64')
url="https://github.com/ValveSoftware/gamescope"
license=('MIT')  # or ('MIT' 'BSD-2-Clause'), adjust as appropriate
install=gamescope.install  # If you have an .install script, place it in source=() as well.

##############################################################################
# Dependencies
##############################################################################
depends=(
  'xorg-xwayland'
  'libavif'
  'aom'
  'rav1e'
  'libxres'
  'xcb-util-errors'
  'freerdp'
  'xcb-util-wm'
  'libxcomposite'
  'pixman'
  'libinput'
  'seatd'
  'pipewire'
  'libxmu'
  'libxcursor'
  'powerbuttond'
  'libdecor'
  'libei'
  'luajit'
  # The standard dependencies from the -git approach:
  'gcc-libs'
  'glibc'
  'glm'
  'hwdata'
  'lcms2'
  'libcap'
  'libdrm'
  'libx11'
  'libxcb'
  'libxdamage'
  'libxext'
  'libxfixes'
  'libxkbcommon'
  'libxrender'
  'libxtst'
  'libxxf86vm'
  'sdl2'
  'vulkan-icd-loader'
  'wayland'
)

# Will conflict/provide the official non-git or other variants:
provides=('gamescope')
conflicts=('gamescope')

_common_makedepends=(
  'benchmark'
  'cmake'
  'git'
  'glslang'
  'meson'
  'ninja'
  'vulkan-headers'
  'wayland-protocols'
  'openssh'  # from first PKGBUILD
)

_lib32_makedepends=(
  'gcc-multilib'
  'lib32-glm'
)

makedepends=("${_common_makedepends[@]}")
if [ "$_lib32" == "true" ]; then
  makedepends+=("${_lib32_makedepends[@]}")
fi

##############################################################################
# Sources
##############################################################################
source=(
  # SteamOS/extra session & systemd service files from first PKGBUILD:
  "galileo-mura-setup.service"
  "gamescope-session"
  "gamescope-wayland.desktop"
  "gamescope-mimeapps.list"
  "gamescope-session.service"
  "gamescope-session.target"
  "gamescope-portals.conf"
  "gamescope-xbindkeys.service"
  "gamescope-mangoapp.service"
  "ibus-gamescope.service"
  "powerbuttond.service"
  "start-gamescope-session"
  "steam-launcher"
  "steam-launcher.service"
  "steam-notif-daemon.service"
  "steam-short-session-tracker"
  "steam_http_loader.desktop"
  "steam-http-loader"
  # Main Gamescope repo and submodules (from second PKGBUILD):
  "git+https://github.com/ValveSoftware/gamescope.git"               # $srcdir/gamescope
  "git+https://github.com/Joshua-Ashton/wlroots.git"                 # $srcdir/wlroots
  "git+https://gitlab.freedesktop.org/emersion/libliftoff.git"       # $srcdir/libliftoff
  "git+https://github.com/Joshua-Ashton/vkroots.git"                 # $srcdir/vkroots
  "git+https://gitlab.freedesktop.org/emersion/libdisplay-info.git"  # $srcdir/libdisplay-info
  "git+https://github.com/ValveSoftware/openvr.git"                  # $srcdir/openvr
  "git+https://github.com/Joshua-Ashton/reshade.git"                 # $srcdir/reshade
  "git+https://github.com/Joshua-Ashton/GamescopeShaders.git#tag=v0.1"  # $srcdir/GamescopeShaders
  "git+https://github.com/KhronosGroup/SPIRV-Headers.git"            # $srcdir/SPIRV-Headers
  # If you have a gamescope.install file, add it here too:
  #"gamescope.install"
)
sha256sums=(
  'SKIP'  # galileo-mura-setup.service
  'SKIP'  # gamescope-session
  'SKIP'  # gamescope-wayland.desktop
  'SKIP'  # gamescope-mimeapps.list
  'SKIP'  # gamescope-session.service
  'SKIP'  # gamescope-session.target
  'SKIP'  # gamescope-portals.conf
  'SKIP'  # gamescope-xbindkeys.service
  'SKIP'  # gamescope-mangoapp.service
  'SKIP'  # ibus-gamescope.service
  'SKIP'  # powerbuttond.service
  'SKIP'  # start-gamescope-session
  'SKIP'  # steam-launcher
  'SKIP'  # steam-launcher.service
  'SKIP'  # steam-notif-daemon.service
  'SKIP'  # steam-short-session-tracker
  'SKIP'  # steam_http_loader.desktop
  'SKIP'  # steam-http-loader
  'SKIP'  # gamescope.git
  'SKIP'  # wlroots.git
  'SKIP'  # libliftoff.git
  'SKIP'  # vkroots.git
  'SKIP'  # libdisplay-info.git
  'SKIP'  # openvr.git
  'SKIP'  # reshade.git
  'SKIP'  # GamescopeShaders.git
  'SKIP'  # SPIRV-Headers.git
)

##############################################################################
# pkgver: auto-generate from git tag
##############################################################################
pkgver() {
  cd "${srcdir}/gamescope"
  # e.g. "v3.16.1-38-gef1e8dbe" -> "3.16.1.r38.gef1e8dbe"
  git describe --long --tags 2>/dev/null | sed 's/^v//; s/\([^-]*-g\)/r\1/; s/-/./g'
}

##############################################################################
# prepare
##############################################################################
prepare() {
  cd "${srcdir}/gamescope"

  # Initialize the submodules for wlroots, vkroots, etc.
  meson subprojects download

  git submodule init subprojects/wlroots
  git config submodule.subprojects/wlroots.url "${srcdir}/wlroots"

  git submodule init subprojects/libliftoff
  git config submodule.subprojects/libliftoff.url "${srcdir}/libliftoff"

  git submodule init subprojects/vkroots
  git config submodule.subprojects/vkroots.url "${srcdir}/vkroots"

  git submodule init subprojects/libdisplay-info
  git config submodule.subprojects/libdisplay-info.url "${srcdir}/libdisplay-info"

  git submodule init subprojects/openvr
  git config submodule.subprojects/openvr.url "${srcdir}/openvr"

  git submodule init src/reshade
  git config submodule.src/reshade.url "${srcdir}/reshade"

  git submodule init thirdparty/SPIRV-Headers
  git config submodule.thirdparty/SPIRV-Headers.url "${srcdir}/SPIRV-Headers"

  git -c protocol.file.allow=always submodule update
}

##############################################################################
# build
##############################################################################
build() {
  # 1) Build 64-bit version of Gamescope
  msg2 "Building 64-bit gamescope..."
  rm -rf "${srcdir}/build64"
  meson setup "${srcdir}/build64" "${srcdir}/gamescope" \
    -Dforce_fallback_for=stb,wlroots,vkroots,libliftoff,glm,libdisplay-info \
    --auto-features=enabled \
    --prefix=/usr \
    --buildtype=release
  meson compile -C "${srcdir}/build64"

  # 2) Optional: build 32-bit WSI layer
  if [ "$_lib32" == "true" ]; then
    msg2 "Building 32-bit WSI layer (lib32-gamescope-git)..."
    rm -rf "${srcdir}/build32"

    export CC="gcc -m32"
    export CXX="g++ -m32"
    export PKG_CONFIG="i686-pc-linux-gnu-pkg-config"

    meson setup "${srcdir}/build32" "${srcdir}/gamescope" \
      --libdir=/usr/lib32 \
      -Denable_gamescope=false \
      -Denable_gamescope_wsi_layer=true \
      -Denable_openvr_support=false \
      -Dpipewire=disabled \
      --buildtype=release \
      --prefix=/usr

    meson compile -C "${srcdir}/build32"
  fi
}

##############################################################################
# package: gamescope-git (64-bit main package)
##############################################################################
package_gamescope-git() {
  # Standard meson install from the 64-bit build
  DESTDIR="$pkgdir" meson install -C "${srcdir}/build64" --skip-subprojects

  # Copy the ReShade/GamescopeShaders files:
  install -d "$pkgdir/usr/share/gamescope/reshade"
  cp -r "${srcdir}/GamescopeShaders/"* "$pkgdir/usr/share/gamescope/reshade/"
  chmod -R 755 "$pkgdir/usr/share/gamescope"

  # Now install your extra SteamOS session & systemd files:
  install -D -m 755 "${srcdir}/gamescope-session" \
                    "$pkgdir/usr/lib/steamos/gamescope-session"
  install -D -m 755 "${srcdir}/steam-launcher" \
                    "$pkgdir/usr/lib/steamos/steam-launcher"
  install -D -m 755 "${srcdir}/steam-short-session-tracker" \
                    "$pkgdir/usr/lib/steamos/steam-short-session-tracker"

  install -D -m 755 "${srcdir}/start-gamescope-session" \
                    "$pkgdir/usr/bin/start-gamescope-session"
  install -D -m 644 "${srcdir}/gamescope-wayland.desktop" \
                    "$pkgdir/usr/share/wayland-sessions/gamescope-wayland.desktop"

  # URL handler
  install -D -m 644 "${srcdir}/steam_http_loader.desktop" \
                    "$pkgdir/usr/share/applications/steam_http_loader.desktop"
  install -D -m 644 "${srcdir}/gamescope-mimeapps.list" \
                    "$pkgdir/usr/share/applications/gamescope-mimeapps.list"
  install -D -m 755 "${srcdir}/steam-http-loader" \
                    "$pkgdir/usr/bin/steam-http-loader"

  # Systemd user services/targets
  install -D -m 644 "${srcdir}/galileo-mura-setup.service" \
                    "$pkgdir/usr/lib/systemd/user/galileo-mura-setup.service"
  install -D -m 644 "${srcdir}/gamescope-session.service" \
                    "$pkgdir/usr/lib/systemd/user/gamescope-session.service"
  install -D -m 644 "${srcdir}/gamescope-session.target" \
                    "$pkgdir/usr/lib/systemd/user/gamescope-session.target"
  install -D -m 644 "${srcdir}/gamescope-mangoapp.service" \
                    "$pkgdir/usr/lib/systemd/user/gamescope-mangoapp.service"
  install -D -m 644 "${srcdir}/ibus-gamescope.service" \
                    "$pkgdir/usr/lib/systemd/user/ibus-gamescope.service"
  install -D -m 644 "${srcdir}/powerbuttond.service" \
                    "$pkgdir/usr/lib/systemd/user/powerbuttond.service"
  install -D -m 644 "${srcdir}/steam-launcher.service" \
                    "$pkgdir/usr/lib/systemd/user/steam-launcher.service"
  install -D -m 644 "${srcdir}/steam-notif-daemon.service" \
                    "$pkgdir/usr/lib/systemd/user/steam-notif-daemon.service"
  install -D -m 644 "${srcdir}/gamescope-xbindkeys.service" \
                    "$pkgdir/usr/lib/systemd/user/gamescope-xbindkeys.service"

  # Portals
  install -D -m 644 "${srcdir}/gamescope-portals.conf" \
                    "$pkgdir/usr/share/xdg-desktop-portal/gamescope-portals.conf"

  # Clean up unneeded bits if they appear:
  rm -rf "$pkgdir/usr/include"
  rm -rf "$pkgdir/usr/lib/libwlroots"*
  rm -rf "$pkgdir/usr/lib/pkgconfig"

  # License
  install -Dm644 "${srcdir}/gamescope/LICENSE" \
                 "$pkgdir/usr/share/licenses/gamescope-git/LICENSE"
}

##############################################################################
# package: lib32-gamescope-git (optional, 32-bit WSI only)
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

  DESTDIR="$pkgdir" meson install -C "${srcdir}/build32" --skip-subprojects

  # Remove anything not strictly needed for the 32-bit WSI:
  rm -rf "$pkgdir/usr/share/gamescope" \
         "$pkgdir/usr/include" \
         "$pkgdir/usr/lib/libwlroots"* \
         "$pkgdir/usr/lib32/libwlroots"* \
         "$pkgdir/usr/lib/pkgconfig" \
         "$pkgdir/usr/lib32/pkgconfig"

  install -Dm644 "${srcdir}/gamescope/LICENSE" \
                 "$pkgdir/usr/share/licenses/lib32-gamescope-git/LICENSE"
}
fi
