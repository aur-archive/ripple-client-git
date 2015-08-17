# Maintainer: Andy Weidenbaum <archbaum@gmail.com>

pkgname=ripple-client-git
pkgver=20150507
pkgrel=1
pkgdesc="Web UI for the Ripple payment network"
arch=('i686' 'x86_64')
depends=('nodejs')
makedepends=('git' 'nodejs-bower' 'nodejs-grunt-cli' 'npm')
groups=('ripple')
url="https://github.com/ripple/ripple-client"
license=('MIT')
source=(git+https://github.com/ripple/ripple-client)
sha256sums=('SKIP')
provides=('ripple-client')
conflicts=('ripple-client')
options=('!strip')
install=ripple-client.install

pkgver() {
  cd ${pkgname%-git}
  git log -1 --format="%cd" --date=short | sed "s|-||g"
}

prepare() {
  cd ${pkgname%-git}

  msg 'Creating default config file...'
  cp -dpr --no-preserve=ownership src/js/config-example.js src/js/config.js
}

build() {
  cd ${pkgname%-git}

  msg 'Installing NPM dependencies...'
  npm install --python=python2

  msg 'Fetching Web assets...'
  bower install --allow-root --config.interactive=false

  msg 'Building...'
  grunt
}

package() {
  cd ${pkgname%-git}

  msg 'Installing license...'
  install -Dm 644 "LICENSE" "$pkgdir/usr/share/licenses/ripple-client/LICENSE"

  msg 'Installing documentation...'
  install -Dm 644 "CONTRIBUTING.md" "$pkgdir/usr/share/doc/ripple-client/CONTRIBUTING.md"
  install -Dm 644 "README.md" "$pkgdir/usr/share/doc/ripple-client/README.md"

  msg 'Installing appdirs...'
  install -dm 755 "$pkgdir/usr/share/ripple-client"
  for _appdir in `find . -mindepth 1 -maxdepth 1 -type d`; do
    cp -dpr --no-preserve=ownership "$_appdir" "$pkgdir/usr/share/ripple-client/$_appdir"
  done

  msg 'Installing appfiles...'
  for _appfile in `find . -mindepth 1 -maxdepth 1 -type f`; do
    cp -dpr --no-preserve=ownership "$_appfile" "$pkgdir/usr/share/ripple-client/$_appfile"
  done

  msg 'Cleaning up pkgdir...'
  find "$pkgdir" -type f -name .gitignore -exec rm -r '{}' +
  find "$pkgdir" -type d -name .git -exec rm -r '{}' +
}
