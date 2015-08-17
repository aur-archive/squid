# $Id: PKGBUILD 161334 2012-06-09 22:30:24Z dreisner $
# Contributor: Kevin Piche <kevin@archlinux.org>
# Contributor: Tom Newsom <Jeepster@gmx.co.uk>
# Maintainer: Mark Coolen <mark.coolen@gmail.com>

pkgname="squid"
pkgver="3.2.3"
pkgrel="1"
pkgdesc="A full-featured Web proxy cache server."
arch=('i686' 'x86_64')
url="http://www.squid-cache.org"
depends=('openssl' 'pam' 'cron' 'perl' 'libltdl')
makedepends=('libcap')
license=('GPL')
backup=('etc/squid/squid.conf'
        'etc/squid/mime.conf'
        'etc/conf.d/squid')
install=squid.install

#source=("http://www.squid-cache.org/Versions/v3/3.2/$pkgname-$pkgver.tar.bz2"
#source=( "$pkgname-$pkgver"'.tar.bz2'
source=("http://www.squid-cache.org/Versions/v3/3.2/$pkgname-$pkgver.tar.bz2"
        'squid'
        'squid.conf.d'
        'squid.pam'
        'squid.cron'
        'squid.service')

md5sums=('b26171dfd397defd9ee113d555691b86'
         '02f7b5bd793f778e40834fd6457d2199'
         '2383772ef94efddc7b920628bc7ac5b0'
         '270977cdd9b47ef44c0c427ab9034777'
         'b499c2b725aefd7bd60bec2f1a9de392'
         '20e00e1aa1198786795f3da32db3c1d8')

build() {
  cd "$pkgname-$pkgver"

  # gcc 4.6 doesn't support -fhuge-objects.
  sed '/^    HUGE_OBJECT_FLAG=/ s/"-fhuge-objects"//' -i configure

  # fix cache_dir, cache_dir size, and effective group.
  sed '/^DEFAULT_SWAP_DIR/ s@/cache@/cache/squid@' -i src/Makefile.in
  sed '/^#cache_dir/ s/100/256/
       /^NAME: cache_effective_group/ {n;n;s/none/proxy/}' -i src/cf.data.pre

./configure \
      --prefix=/usr \
      --datadir=/usr/share/squid \
      --sysconfdir=/etc/squid \
      --libexecdir=/usr/lib/squid \
      --localstatedir=/var \
      --with-logdir=/var/log/squid \
      --with-pidfile=/run/squid.pid \
      --enable-auth \
      --enable-auth-basic \
      --enable-auth-ntlm \
      --enable-auth-digest \
      --enable-auth-negotiate \
      --enable-removal-policies="lru,heap" \
      --enable-storeio="aufs,ufs,diskd" \
      --enable-delay-pools \
      --enable-arp-acl \
      --enable-ssl \
      --enable-snmp \
      --enable-linux-netfilter \
      --enable-ident-lookups \
      --enable-useragent-log \
      --enable-cache-digests \
      --enable-referer-log \
      --enable-arp-acl \
      --enable-htcp \
      --enable-carp \
      --enable-epoll \
      --with-filedescriptors=4096 \
      --with-large-files \
      --enable-arp-acl \
      --with-default-user=proxy \
      --enable-async-io \
      --enable-truncate



  make
}

package() {
  make -C "$pkgname-$pkgver" DESTDIR="$pkgdir" install

  install -Dm755 "$srcdir"/squid "$pkgdir"/etc/rc.d/squid
  install -Dm755 "$srcdir"/squid.cron "$pkgdir"/etc/cron.weekly/squid
  install -Dm644 "$srcdir"/squid.conf.d "$pkgdir"/etc/conf.d/squid
  install -Dm644 "$srcdir"/squid.pam "$pkgdir"/etc/pam.d/squid

  install -Dm644 "$srcdir/squid.service" "$pkgdir/usr/lib/systemd/system/squid.service"

  # random unneeded empty dir...
  rmdir "$pkgdir/usr/include"
}

# vim: ts=2 sw=2 et ft=sh
