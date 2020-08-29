# Installation

## Linux

Verilator comes as binary package on many Linux distributions. So, this should work:

    # apt-get install verilator

Or, on RPM based package managers:

    # zypper install verilator


## Msys under Windows

You can install with the gcc toolchain on Windows:

    pacman -S mingw-w64-x86_64-verilator

If you want to build it from source, you can follow the method used in the PKGBUILD:

    export MSYSTEM_PREFIX='/mingw64'
    export MSYSTEM_CHOST='x86_64-w64-mingw32'
    
    cp /usr/include/FlexLexer.h src/
    
    export MSYS2_ARG_CONV_EXCL="-DDEFENV" 
    
    autoconf
    
    ./configure \
        --prefix=${MINGW_PREFIX} \
        --build=${MINGW_CHOST} \
        --host=${MINGW_CHOST}
    
    make


When you have installed Verilator succesfully you should be able to see this output:

```
$ verilator
Usage:
        verilator --help
        verilator --version
        verilator --cc [options] [source_files.v]... [opt_c_files.cpp/c/cc/a/o/so]
        verilator --sc [options] [source_files.v]... [opt_c_files.cpp/c/cc/a/o/so]
        verilator --lint-only -Wall [source_files.v]...
```

## Installing SystemC

For building systems with Verilator installing SystemC is a good idea.
You can download the package from [Accellera SysemC Github repo](https://github.com/accellera-official/systemc)

```
  cd build/
  cmake -G Ninja ..
  ninja all
  sudo ninja all
```

