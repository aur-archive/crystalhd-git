pkgname=crystalhd-git
pkgver=20130503
pkgrel=2
pkgdesc="Broadcom CrystalHD kernel module"
arch=('i686' 'x86_64')
url="http://git.linuxtv.org/git/jarod/crystalhd.git"
license=('GPL2')
depends=('linux>=3.0' 'libcrystalhd-git')
makedepends=('autoconf' 'linux-headers>=3.0' 'make' 'git')
install='crystalhd-git.install'
source=(devinitFix.patch)
conflicts=('crystalhd')
md5sums=('d02e23700e9e765dcb7642b2ecce2ece')
_gitroot='git://linuxtv.org/jarod/crystalhd.git'
_gitname='crystalhd'

package() {

    cd $startdir/src

    msg "Connecting to the GIT server...."

    if [ -d $startdir/src/$_gitname ] ; then
        cd $_gitname && git pull origin
        msg "The local files are updated."
    else
        git clone $_gitroot
        cd $_gitname
    fi

    patch -p0 < $startdir/src/devinitFix.patch

    cd driver/linux

    autoreconf -vif
    ./configure --prefix=/usr
    sed -i 's/'-Werror'/''/g' Makefile 

    make CONFIG_DEBUG_SECTION_MISMATCH=y V=1 || return 1
    
    mkdir -p $pkgdir/etc/udev/rules.d
    mkdir -p $pkgdir/usr/lib/modules/$(uname -r)/kernel/drivers/video/broadcom

    cp -f 20-crystalhd.rules $pkgdir/etc/udev/rules.d/
    install -d $pkgdir/usr/lib/modules/$(uname -r)/kernel/drivers/video/broadcom
    install -m 0644 crystalhd.ko $pkgdir/usr/lib/modules/$(uname -r)/kernel/drivers/video/broadcom
}

