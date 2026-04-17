# Compiling OAHCoin on Windows (Native MSYS2)

The OAHCoin codebase has been modernized to compile on recent toolchains (OpenSSL 3.0, Boost 1.70+, GCC C++14). This guide will help you build the Windows daemon (`oldagehomecoind.exe`) and the graphical wallet (`oldagehomecoin-qt.exe`) using the MSYS2 environment.

## 1. Install MSYS2
Determine your system architecture (64-bit is standard) and download the MSYS2 installer from [msys2.org](https://www.msys2.org/). Follow the installation instructions until you have the MSYS2 terminal open.

## 2. Install Dependencies
Open the **MSYS2 MinGW 64-bit** terminal (or 32-bit if you require it) from your Start Menu.
Execute the following commands to update your package database and install necessary dependencies:

```bash
pacman -Syu
# After it finishes, it may ask you to close the terminal. Reopen it and run:
pacman -Su
```

Now, install the toolchain and required cryptocurrency libraries:

```bash
pacman -S mingw-w64-x86_64-toolchain 
pacman -S mingw-w64-x86_64-boost
pacman -S mingw-w64-x86_64-openssl
pacman -S mingw-w64-x86_64-db
pacman -S mingw-w64-x86_64-miniupnpc
pacman -S mingw-w64-x86_64-qt5
pacman -S mingw-w64-x86_64-qrencode
pacman -S make
```

## 3. Compile the Command-Line Daemon (`oldagehomecoind`)
Navigate to the `src` directory in the MSYS2 terminal:
```bash
cd /d/OAH/OAHCoin/src
```

Use the newly created `makefile.msys2` to build:
```bash
make -f makefile.msys2
```
This will produce `oldagehomecoind.exe` in the `src` folder.

## 4. Compile the Graphical Wallet (`oldagehomecoin-qt`)
Navigate to the root directory `d:\OAH\OAHCoin`:
```bash
cd /d/OAH/OAHCoin
```

Generate the Qt makefiles and build:
```bash
qmake "USE_UPNP=1" "USE_QRCODE=1" oldagehomecoin-qt.pro
make -f Makefile.Release
```
This will compile the UI wallet and produce `release/oldagehomecoin-qt.exe`.

### Troubleshooting
- If you see `auto_ptr` errors from Boost or Qt, ensure you are using `-std=c++14`, which we have added to the makefiles.
- The `BIGNUM` and `ECDSA` OpenSSL structs have been safely polyfilled. If you see warnings regarding `EC_KEY_new`, they are expected due to modern OpenSSL 3.0 deprecating some ECC primitives, but the software will still function correctly since we explicitly suppress the `-Wdeprecated-declarations` warnings.
