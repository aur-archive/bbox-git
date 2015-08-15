# Contributor: Florian Bäuerle <florian.bae@gmail.com>

pkgname=bbox-git
pkgver=20120403
pkgrel=4
pkgdesc="...something like dropbox based on subversion. (Git-Version, not precompiled, works w/o multilib)"
arch=('i686' 'x86_64')
url="http://bbox.nois3lab.it/"
license=('unknown')
depends=('qt' 'subversion')

_gitroot="https://github.com/bakulf/bbox.git"
_gitname="bbox"

build() {
  msg "Connecting to GIT server...."

  if [ -d $startdir/src/$_gitname ] ; then
    cd $_gitname && git pull origin
    msg "The local files are updated."
  else
    git clone $_gitroot
  fi

  msg "GIT checkout done or server timeout"
}

package() {
    cd $srcdir/$_gitname
    qmake
    make
    patch ./src/Makefile <<EOF
204c204
< DESTDIR       = 
---
> DESTDIR       = \$(DESTDIR)
807,809c807,809
< 	@\$(CHK_DIR_EXISTS) \$(INSTALL_ROOT)/usr/bin/ || \$(MKDIR) \$(INSTALL_ROOT)/usr/bin/ 
< 	-\$(INSTALL_PROGRAM) "\$(QMAKE_TARGET)" "\$(INSTALL_ROOT)/usr/bin/\$(QMAKE_TARGET)"
< 	-\$(STRIP) "\$(INSTALL_ROOT)/usr/bin/\$(QMAKE_TARGET)"
---
> 	@\$(CHK_DIR_EXISTS) \$(DESTDIR)/usr/bin/ || \$(MKDIR) \$(DESTDIR)/usr/bin/ 
> 	-\$(INSTALL_PROGRAM) "\$(QMAKE_TARGET)" "\$(DESTDIR)/usr/bin/\$(QMAKE_TARGET)"
> 	-\$(STRIP) "\$(DESTDIR)/usr/bin/\$(QMAKE_TARGET)"
812,813c812,813
< 	-\$(DEL_FILE) "\$(INSTALL_ROOT)/usr/bin/\$(QMAKE_TARGET)"
< 	-\$(DEL_DIR) \$(INSTALL_ROOT)/usr/bin/ 
---
> 	-\$(DEL_FILE) "\$(DESTDIR)/usr/bin/\$(QMAKE_TARGET)"
> 	-\$(DEL_DIR) \$(DESTDIR)/usr/bin/ 

EOF
    make DESTDIR=$pkgdir PREFIX=/usr install
    mkdir $pkgdir/usr/share/
    mkdir $pkgdir/usr/share/applications
    echo "[Desktop Entry]
Encoding=UTF-8
Name=BBox
Comment=...something like dropbox based on subversion.
Exec=bbox.n3
Icon=bbox.n3
Terminal=false
Type=Application
Categories=Network;
StartupNotify=true"> $pkgdir/usr/share/applications/bbox.desktop
}

