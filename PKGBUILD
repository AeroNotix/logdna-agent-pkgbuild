pkgname=logdna-agent
pkgver=0.0.1
pkgrel=1
pkgdesc="A top-like tool for monitoring an Erlang node"
arch=('x86_64')
url="https://github.com/logdna/logdna-agent/"
license=('MIT')
makedepends=('npm')
provides=('logdna-agent')
source=('git://github.com/logdna/logdna-agent/')
md5sums=('SKIP')


package () {
  npm install -g --user root --prefix "$pkgdir"/usr $source
  find "$pkgdir/usr" -type d -exec chmod 755 {} +

  # Remove references to $pkgdir
  find "$pkgdir" -type f -name package.json -print0 | xargs -0 sed -i "/_where/d"

  # Remove references to $srcdir
  local tmppackage="$(mktemp)"
  local pkgjson="$pkgdir/usr/lib/node_modules/$pkgname/package.json"
  jq '.|=with_entries(select(.key|test("_.+")|not))' "$pkgjson" > "$tmppackage"
  mv "$tmppackage" "$pkgjson"
  chmod 644 "$pkgjson"
  mkdir -p $pkgdir/usr/bin/
  echo "#!/bin/sh

node /usr/lib/node_modules/logdna-agent/index.js \"$@\"
" >> "$pkgdir/usr/bin/logdna-agent"
  chmod +x "$pkgdir/usr/bin/logdna-agent"
}

# vim:set ts=2 sw=2 et:
