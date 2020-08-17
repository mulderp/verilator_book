# Installation

## Linux

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
    


