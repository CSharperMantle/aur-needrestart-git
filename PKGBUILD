# Maintainer: csmantle <aur at csmantle dot top>
# Contributor: Mark Wagie <mark dot wagie at proton dot me>
pkgname=needrestart-git
pkgver=3.11.r0.g6d7a76b
pkgrel=3
pkgdesc="Restart daemons after library updates."
arch=('any')
url="https://github.com/liske/needrestart"
license=('GPL-2.0-or-later')
depends=(
  'perl-module-find'
  'perl-term-readkey'
  'perl-proc-processtable'
  'perl-sort-naturally'
  'perl-module-scandeps'
  'perl-libintl-perl'
)
makedepends=('git')
optdepends=('iucode-tool: for outdated Intel microcode detection')
provides=("${pkgname%-git}")
conflicts=("${pkgname%-git}")
backup=("etc/${pkgname%-git}/${pkgname%-git}.conf"
        "etc/${pkgname%-git}/notify.conf")
source=('git+https://github.com/liske/needrestart.git'
        'needrestart.hook')
sha256sums=('SKIP'
            'e5c6696a281f5445a3b7e2b7d1055f9189a2c39d4940721aa0c2718780f15f63')

pkgver() {
  cd "$srcdir"/"${pkgname%-git}"
  git describe --long --tags --abbrev=7 | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
  cd "$srcdir"/"${pkgname%-git}"
  find . -type f -exec sed -i 's/sbin/bin/g' {} \;
}

build() {
  cd "$srcdir"/"${pkgname%-git}"
  unset PERL5LIB PERL_LOCAL_LIB_ROOT PERL_MB_OPT PERL_MM_OPT
  export PERL_MM_USE_DEFAULT=1 PERL_AUTOINSTALL=--skipdeps
  make
}

package() {
  cd "$srcdir"/"${pkgname%-git}"
  unset PERL5LIB PERL_LOCAL_LIB_ROOT PERL_MB_OPT PERL_MM_OPT
  make DESTDIR="$pkgdir" install

  install -vDm444 "$srcdir"/needrestart.hook "$pkgdir"/usr/share/libalpm/hooks/needrestart.hook
  install -vDm644 "$srcdir"/"${pkgname%-git}"/man/needrestart.1 "$pkgdir"/usr/share/man/man1/needrestart.1

  # remove empty dirs; '!emptydirs' doesn't remove them
  rm -rf "$pkgdir"/usr/lib/perl5/
}
