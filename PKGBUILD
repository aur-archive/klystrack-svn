# Contributor: Eric Forgeot < http://anamnese.online.fr >

pkgname=klystrack-svn
pkgrealname=klystrack
pkgver=1153
pkgrel=1
pkgdesc="A chiptune tracker"
arch=('i686' 'x86_64')
url="http://code.google.com/p/klystrack/"
license=('MIT')
depends=('sdl')
makedepends=('klystron')
conflicts=('klystrack')
provides=('klystrack')

_svntrunk='http://klystrack.googlecode.com/svn/trunk/'
_svnmod='klystrack-read-only'




build() {
	cd $startdir/
	cp -fr /usr/share/klystron $startdir/
	
if [ -d "${_svnmod}/.svn" ]; then
    (cd "$_svnmod" && svn up -r $pkgver)
  else
    svn co "$_svntrunk" --config-dir ./ -r $pkgver $_svnmod
  fi

  msg 'SVN checkout done or server timeout'
  msg 'Starting make...'

  rm -rf "${_svnmod}-build"
  cp -r "$_svnmod" "${_svnmod}-build"
  cd "${_svnmod}-build"
  
	sed -i -e "s|Uint16|Uint64|g" $startdir/klystron/src/snd/freqs.c
	sed -i -e "s|Uint16|Uint64|g" $startdir/klystron/src/snd/freqs.h
	make
	mkdir -p $startdir/pkg/usr/share/klystrack
	mkdir -p $startdir/pkg/usr/lib/klystrack
	mkdir -p $startdir/pkg/usr/bin
	
}

package() {
  
   #desktop icons
	mkdir -p $pkgdir/usr/share/pixmaps
	mkdir -p $pkgdir/usr/share/applications

	cp $startdir/$_svnmod-build/icon/256x256.png $pkgdir/usr/share/pixmaps/$pkgrealname.png
	
	install -D -m644 $startdir/$pkgrealname.desktop $pkgdir/usr/share/applications/$pkgrealname.desktop

	#deploy 
  cp -fr $startdir/${_svnmod}-build/bin.debug/klystrack $startdir/pkg/usr/bin/klystrack
  
  chmod +x $startdir/pkg/usr/bin/klystrack
  
  #ln -s /usr/share/klystrack/klystrack $pkgdir/usr/bin/klystrack
  
  for i in res key 
  do
    cp -fr $startdir/${_svnmod}-build/$i $startdir/pkg/usr/lib/klystrack
    rm -fr $startdir/pkg/usr/lib/klystrack/$i/.svn
  done

  for i in doc icon examples  
  do
    cp -fr $startdir/${_svnmod}-build/$i $startdir/pkg/usr/share/klystrack
    rm -fr $startdir/pkg/usr/share/klystrack/$i/.svn
  done  
  
  
  	rm -fr $startdir/pkg/usr/share/klystrack/examples/instruments/.svn
  	rm -fr $startdir/pkg/usr/share/klystrack/examples/songs/.svn
  



}
