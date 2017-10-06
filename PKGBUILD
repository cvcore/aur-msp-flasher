# Maintainer: Peter Ivanov <ivanovp@gmail.com>

pkgname=msp-flasher
pkgver=1_03_15_00
pkgver_short=$(echo $pkgver | rev | cut -c 4- | rev | sed -e 's/\_0/\_/g' -e 's/\_/\./g') #x.y.zz without leading zeros
pkgrel=1
pkgdesc="Flasher for TI MSP430 processor"
arch=('i686' 'x86_64')
url="http://www.ti.com/tool/msp430-flasher"
license=('GPL')
depends=('elfutils' 'libmpc' 'zlib')
options=(!strip !emptydirs !libtool staticlibs !upx)
PKGEXT=".pkg.tar"
install=msp-flasher.install


if [ $CARCH = i686 ]; then
   _installer=MSPFlasher-$pkgver-linux-installer.zip
   _installer_run=MSPFlasher-$pkgver_short-linux-installer.run
elif [ $CARCH = x86_64 ]; then
    _installer=MSPFlasher-$pkgver-linux-x64-installer.zip
    _installer_run=MSPFlasher-$pkgver_short-linux-x64-installer.run
fi

source_i686=("http://software-dl.ti.com/msp430/msp430_public_sw/mcu/msp430/MSP430Flasher/$pkgver/exports/$_installer" "${pkgname}.sh")
source_x86_64=("http://software-dl.ti.com/msp430/msp430_public_sw/mcu/msp430/MSP430Flasher/$pkgver/exports/$_installer" "${pkgname}.sh")

md5sums_i686=("$(curl http://software-dl.ti.com/msp430/msp430_public_sw/mcu/msp430/MSP430Flasher/latest/exports/md5sum.txt | grep linux-installer | awk '{print $1}')" 'a3c04211e27f2f5f91e3b0b9b05f9e4b')
md5sums_x86_64=("$(curl http://software-dl.ti.com/msp430/msp430_public_sw/mcu/msp430/MSP430Flasher/latest/exports/md5sum.txt | grep linux-x64-installer | awk '{print $1}')" 'a3c04211e27f2f5f91e3b0b9b05f9e4b')

_install_dir=/opt/ti/$pkgname

build() {
  chmod +x $_installer_run
}

package() {
  msg "Running TI's installer..."
  ${srcdir}/$_installer_run --mode unattended --prefix $pkgdir$_install_dir
  msg "Correcting directory permissions..."
  find $pkgdir$_install_dir -type d -print0|xargs -0 chmod 755
  mkdir -p $pkgdir/usr/lib
  ln -s $pkgdir$_install_dir/libmsp430.so $pkgdir/usr/lib

  install -Dm755 "${srcdir}/${pkgname}.sh" "${pkgdir}/etc/profile.d/${pkgname}.sh"
}
