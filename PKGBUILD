# Maintainer: MicLeh <[EMAIL_ADDRESS]>
pkgname=bluej-standalone-bin
pkgver=5.5.0
pkgrel=1
pkgdesc="Standalone repackaging of bluej IDE with bundled JDK and JavaFX"
arch=('x86_64')
url="https://www.bluej.org"
license=('GPL2' 'Apache' 'BSD' 'CDDL' 'CPL' 'custom')
provides=('bluej-standalone-bin')
conflicts=('bluej')

depends=(
  'bash'
  'gtk3'
  'libglvnd'
  'alsa-lib'
  'libxtst'
  'libxi'
  'libxrender'
  'libxrandr'
  'libxext'
  'fontconfig'
  'freetype2'
)
makedepends=('unzip')

source=(
  'bluej'
  'bluej-module'
  'bluej.desktop'
  'bluej.xml'
  'BlueJ-generic-5.5.0.jar::https://github.com/k-pet-group/BlueJ-Greenfoot/releases/download/BLUEJ-RELEASE-5.5.0/BlueJ-generic-5.5.0.jar'
  'openjdk-21.0.2_linux-x64_bin.tar.gz::https://download.java.net/java/GA/jdk21.0.2/f2283984656d49d69e91c558476027ac/13/GPL/openjdk-21.0.2_linux-x64_bin.tar.gz'
  'openjfx-21.0.10_linux-x64_bin-sdk.zip::https://download2.gluonhq.com/openjfx/21.0.10/openjfx-21.0.10_linux-x64_bin-sdk.zip'
)

b2sums=('07779f84b32e005f20624b1b01a29ced52f89129a7b3f0a2501bd7cd042cca23717ec0bf2e626a2efecea13c197199b17d61f9d34a5e9977f8a5615e3c52ac97'
        '8c3f45dbd598dd34084930799479801f6033dc291a9243fb1d51e0e36d87ca0488ab2e4049ad455fcf323c69429d4613f1b9ac764c0632449ef92d4fb276f648'
        '48374b148326865e4c231bdd86703927aebb0139ae2f7cba98ea62c8af329fdf48a2275225ecbd100f51d30ac7abd0dcc2ba13fa66a785fc027726b78ca098c6'
        '69f2c4d16d34f9508da44e95357e84470593e9af4116f08b47c7b9d799f91505c5657ce2b52c04e79d80528b148509416b3d90460bc2ac544a21eece002e4635'
        '591ca0ff128d533b217083086bd3a68f8bded05cd62a388c98bd5feee9abe329568f1199a9cf1ebbee17186ae4bc7c55315d7c501cc01cdf84fd7a029ba27811'
        '4545a0f9fd70fecdbfc21e850221259d3125890cc16f123e1d353bcdaaa1b7e60ed84e473a714cf0296cad0485eab62e0dde831ca8e59173d8d3e698548d2763'
        '460315cc9abd65f84e17252324ff4a14b7249d71429da9ec298a01e1cc88a4b7dc43c04c38ad16e6d302d2867e86122c271f2d292e023dff8f919a421786de55')

noextract=(
  'BlueJ-generic-5.5.0.jar'
  'openjfx-21.0.10_linux-x64_bin-sdk.zip'
)

prepare() {
  cd "$srcdir"

  unzip -o "BlueJ-generic-5.5.0.jar" bluej-dist.jar

  mkdir -p bluej-app
  unzip -o bluej-dist.jar -d bluej-app

  unzip -o "openjfx-21.0.10_linux-x64_bin-sdk.zip"
}

package() {
  cd "$srcdir"

  # --- bluej application files ---
  install -dm755 "${pkgdir}/opt/bluej"
  cp -a bluej-app/. "${pkgdir}/opt/bluej/"

  # --- Bundled JDK ---
  cp -a "${srcdir}/jdk-21.0.2" "${pkgdir}/opt/bluej/jdk"
  find "${pkgdir}/opt/bluej/jdk" -type f -name '*.so' -exec chmod 755 {} +

  # --- Bundled JavaFX SDK ---
  cp -a "${srcdir}/javafx-sdk-21.0.10" "${pkgdir}/opt/bluej/javafx"

  # --- Launchers (classpath/default and module variant) ---
  install -Dm755 "${srcdir}/bluej"         "${pkgdir}/usr/bin/bluej"
  install -Dm755 "${srcdir}/bluej-module"  "${pkgdir}/usr/bin/bluej-module"

  # --- Desktop entry ---
  install -Dm644 "${srcdir}/bluej.desktop" \
    "${pkgdir}/usr/share/applications/bluej.desktop"

  # --- MIME type definition ---
  install -Dm644 "${srcdir}/bluej.xml" \
    "${pkgdir}/usr/share/mime/packages/bluej.xml"

  # --- Icon (hicolor theme) ---
  install -Dm644 "bluej-app/lib/images/bluej-icon-256.png" \
    "${pkgdir}/usr/share/icons/hicolor/256x256/apps/bluej.png"

  # --- Licenses ---
  install -dm755 "${pkgdir}/usr/share/licenses/${pkgname}"

  # bluej license
  install -Dm644 "bluej-app/LICENSE.txt" \
    "${pkgdir}/usr/share/licenses/${pkgname}/bluej_LICENSE"

  # OpenJDK legal notices (if present in this JDK version)
  if [[ -d "${srcdir}/jdk-21.0.2/legal" ]]; then
    cp -a "${srcdir}/jdk-21.0.2/legal" \
      "${pkgdir}/usr/share/licenses/${pkgname}/jdk-legal"
  fi

  # JavaFX legal notices (if present)
  if [[ -d "${srcdir}/javafx-sdk-21.0.10/legal" ]]; then
    cp -a "${srcdir}/javafx-sdk-21.0.10/legal" \
      "${pkgdir}/usr/share/licenses/${pkgname}/javafx-legal"
  fi
}
