# Maintainer: Grey Christoforo <first name at last name dot net>

## This PKGBUILD creates an Arch Linux package for the proprietary MATLAB application. A license from The MathWorks is needed in order to both build the package and to run MATLAB once the package is installed. In order to build the package the user must supply a plain text file installation key and the software. For network installations, in addition to the file installation key, a license file needs to be used for the installation. The tar archive file can be generated from an ISO downloaded from The MathWorks, generated from the official DVD, or created by using the interactive installer to download the toolboxes (installation can be made to a temporary directory and canceled once the toolboxes are downloaded). The contents of the tar archive must include: ./archives/ ./bin/ ./etc/ ./help/ ./java/ /sys ./activate.ini ./install ./installer_input.txt

pkgname=matlab
pkgver=R2017b
pkgrel=0
pkgdesc='A high-level language for numerical computation and visualization'
arch=('x86_64')
url='http://www.mathworks.com'
license=(custom)
optdepends=('gcc49: For MEX support')
source=("matlab_${pkgver}_glnxa64.zip")
md5sums=('SKIP')

prepare() {
  msg2 'Modifying the installer settings' 
  sed -i "s,^# destinationFolder=,destinationFolder=${pkgdir}/opt/$pkgname/," "${srcdir}/installer_input.txt" 
  sed -i "s,^# agreeToLicense=,agreeToLicense=yes," "${srcdir}/installer_input.txt"
} 

package() {
  msg2 'Starting MATLAB installer'
  sudo -u $USER env HOME="$srcdir" "${srcdir}/install" -t -inputFile "${srcdir}/installer_input.txt" -root "${srcdir}"
  
  msg2 'Creating links for executables' 
  install -d -m755 "${pkgdir}/usr/local/bin/"
  for _executable in mex; do 
    ln -s "/opt/$pkgname/bin/${_executable}" "${pkgdir}/usr/local/bin/${_executable}" 
  done 
  
  msg2 'Adding custom launcher'
  echo '#!/bin/sh
MATLAB_JAVA=/usr/lib/jvm/java-8-openjdk/jre /opt/'$pkgname'/bin/matlab "$@"' >> "${pkgdir}/usr/local/bin/matlab"
  chmod 0755 "${pkgdir}/usr/local/bin/matlab"

  msg2 'Configuring mex options'
  sed -i "s#CC='gcc'#CC='gcc-4.9'#g" "${pkgdir}/opt/$pkgname/bin/mexopts.sh"
  sed -i "s#CXX='g++'#CXX='g++-4.9'#g" "${pkgdir}/opt/$pkgname/bin/mexopts.sh"
}
