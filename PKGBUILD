# Maintainer: James Tappin <jtappinatgmaildotcom>

pkgname=gnudatalanguage-cvs
pkgver=20131230
pkgrel=1
pkgdesc="An IDL (Interactive Data Language) compatible incremental \
compiler (ie. runs IDL programs) (CVS version)"
arch=('i686' 'x86_64')
url="http://gnudatalanguage.sourceforge.net/"
license=('GPL')
depends=('python2' 'python2-numpy' 'plplot' 'gsl' 'readline' 'hdf5' 'netcdf' \
    'netcdf-cxx-legacy' 'wxgtk' 'fftw' 'udunits' 'pslib' 'grib_api' \
    'udunits' 'eigen3')
makedepends=('cmake' 'cvs')

options=('!makeflags')
source=(gdl.profile)
md5sums=('40aa5fd8278cd8e80425c62a577563cc')

_cvsroot=":pserver:anonymous:@gnudatalanguage.cvs.sourceforge.net:/cvsroot/gnudatalanguage/"
_cvsmod="gdl"
conflicts=('gnudatalanguage')
provides=('gnudatalanguage')

# So far as I can see, CVS does not provide a satisfactory 
# version/revision/commit for the repository.
pkgver() {
  date +%Y%m%d
}

build() {
  cd $srcdir
  msg "Connecting to $_cvsmod.sourceforge.net CVS server...."

#  cvs -d "$_cvsroot" login
  if [[ -d "$_cvsmod/CVS" ]]; then
    cd "$_cvsmod"
    cvs -z3 update -d
  else
    cvs -z3 -d "$_cvsroot"  co "$_cvsmod" 
    cd "$_cvsmod"
  fi

  msg "CVS checkout done or server timeout"
  msg "Starting build..."

  rm -rf "$srcdir/$_cvsmod-build"
  cp -r "$srcdir/$_cvsmod" "$srcdir/$_cvsmod-build"
  cd "$srcdir/$_cvsmod-build"

  # if [[ -d build ]]; then
  #     rm -r build
  # fi
  mkdir build
  cd build
  cmake -DCMAKE_INSTALL_PREFIX=/usr -DPYTHON=YES -DPYTHONVERSION=2.7 \
      -DPYTHON_EXECUTABLE=/usr/bin/python2.7 \
      -DMAGICK=NO -DFFTW=YES -DHDF5=YES -DHDF=NO -DGRIB=YES -DUDUNITS=YES \
      -DCMAKE_C_FLAGS="-I/usr/include/ImageMagick \
            -I/usr/include/python2.7 \
            -I/usr/lib/python2.7/site-packages/numpy/core/include" ..
  make
}
package() {
  cd $srcdir/gdl-build/build
  make DESTDIR=$pkgdir install

  install -D -m755 ../../gdl.profile $pkgdir/etc/profile.d/gdl.sh
}
