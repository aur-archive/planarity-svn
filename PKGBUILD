# Maintainer: Padfoot <padfoot at exemail dot com dot au>

pkgname=planarity-svn
pkgver=6432
pkgrel=1
pkgdesc="Multitouch Untangle game."
arch=('any')
url="https://www.libavg.de/site/projects/libavg/wiki/Planarity"
license=('GPL3')
depends=('libavg'
         'python2'
         'hicolor-icon-theme')
makedepends=('subversion'
             'gtk-update-icon-cache')
install='planarity.install'
source=("planarity.desktop")
md5sums=('91f586acd8e6fb811187e6dd48c76f1f')

_svntrunk=https://www.libavg.de/svn/trunk/avg_media/mtc/planarity
_svnmod=planarity

build() {
    cd "$srcdir"
    msg "Connecting to SVN server..."

    if [[ -d "$_svnmod/.svn" ]]; then
        (cd "$_svnmod" && svn up -r "$pkgver")
    else
        svn co "$_svntrunk" --config-dir ./ -r "$pkgver" "$_svnmod"
    fi

    msg "SVN checkout done or server timeout"
  
    rm -rf "$srcdir/$_svnmod-build"
    cp -r "$srcdir/$_svnmod" "$srcdir/$_svnmod-build"
    cd "$srcdir/$_svnmod-build"
  
    sed -i "s/python/python2/g" planarity/*.py
    sed -i "s/python/python2/g" scripts/planarity
}

package() {
    _python_libs=`python2 -c "from distutils import sysconfig; print sysconfig.get_python_lib()"`
    
    mkdir -p \
        "$pkgdir/usr/bin" \
        "${pkgdir}${_python_libs}" \
        "$pkgdir/usr/share/icons/hicolor/scalable/apps" \
        "$pkgdir/usr/share/applications" || return 1

    cd "$srcdir/$_svnmod-build"
    cp -r planarity "${pkgdir}${_python_libs}"
    cp planarity/media/vertex.png "$pkgdir/usr/share/icons/hicolor/scalable/apps/planarity.png"

    cd ./scripts
    install -D -m755 planarity "$pkgdir/usr/bin"
    
    cd "$srcdir"
    install -D -m755 planarity.desktop "$pkgdir/usr/share/applications" 
}
