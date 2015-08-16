# Maintainer: Ben Jeurissen <ben.jeurissen@ua.ac.be>
pkgname=mrtrix-svn
pkgver=365
pkgrel=1
pkgdesc="A software tool to perform tractography through crossing fibres"
arch=('x86_64' 'i686')
url="http://www.nitrc.org/projects/mrtrix/"
license=('GPL3')
depends=('gtkmm' 'gtkglext' 'gsl' 'mesa')
makedepends=('subversion' 'python2')
backup=(etc/mrtrix.conf)

_svntrunk=http://mrtrix.googlecode.com/svn/trunk/
_svnmod=mrtrix

build() {
  cd "$srcdir"
  msg "Connecting to SVN server...."

  if [[ -d "$_svnmod/.svn" ]]; then
    (cd "$_svnmod" && svn up -r "$pkgver")
  else
    svn co "$_svntrunk" --config-dir ./ -r "$pkgver" "$_svnmod"
  fi

  msg "SVN checkout done or server timeout"
  msg "Starting build..."

  rm -rf "$srcdir/$_svnmod-build"
  cp -r "$srcdir/$_svnmod" "$srcdir/$_svnmod-build"
  cd "$srcdir/$_svnmod-build"

  python2 ./build
} 

package() {
  cd "$srcdir/$_svnmod-build"
  echo "NumberOfThreads: 1" > mrtrix.conf
  install -d "$pkgdir/usr/bin"
  install -D bin/* "$pkgdir/usr/bin"
  install -d "$pkgdir/usr/lib"
  install -D lib/*.so "$pkgdir/usr/lib"
  install -d "$pkgdir/etc"
  install -D mrtrix.conf "$pkgdir/etc"
}
