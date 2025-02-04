# Dependencies

## GNU/Linux
* GNU Autoconf
* GNU Autoconf Archive
* GNU Automake
* GNU Libtool
* C compiler
* C library development libraries and header files
* pkg-config
* tpm2-tss >= 2.3
* libqrencode
* pandoc (optional, for man pages)
* doxygen (optional, for man pages)
* plymouth header files (optional, for initramfs integration)

For the integration test suite:
* liboath
* [swtpm](https://github.com/stefanberger/swtpm) or [tpm_server](https://sourceforge.net/projects/ibmswtpm2/)
* realpath
* ss

## Ubuntu
```
sudo apt -y install \
  build-essential \
  autoconf \
  autoconf-archive \
  automake \
  m4 \
  libtool \
  gcc \
  pkg-config \
  libqrencode-dev \
  pandoc \
  doxygen \
  liboath-dev \
  iproute2 \
  plymouth \
  libplymouth-dev
git clone --depth=1 https://github.com/tpm2-software/tpm2-totp
cd tpm2-tss
./bootstrap
./configure
make -j$(nproc)
sudo make install
```

# Building from source
```
./bootstrap
./configure
make -j$(nproc)
make -j$(nproc) check
sudo make install
```

# Configuration options
You may pass the following options to `./configure`

## Debug messages
This option will enable a lot of debug printing during the invocation of the
library:
```
./configure --enable-debug
```

## Developer linking
In order to link against a developer version of tpm2-tss (not installed):
```
./configure \
  PKG_CONFIG_PATH=${TPM2TSS}/lib:$PKG_CONFIG_PATH \
  CFLAGS=-I${TPM2TSS}/include \
  LDFLAGS=-L${TPM2TSS}/src/tss2-{tcti,mu,sys,esys}/.libs
```

# initramfs-tools and mkinitcpio integration
To make sure that the hooks get installed to the correct directory, either use
```
./configure --sysconfdir=/etc
```
or set the correct directory directly with the `--with-initramfstoolsdir`/
`--with-mkinitcpiodir` configuration option.

# Post installation

## ldconfig
You may need to run ldconfig after `make install` to update runtime bindings:
```
sudo ldconfig
```
