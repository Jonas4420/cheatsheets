# C

## Cross-compilation for aarch64

Needed packages:
- aarch64-linux-gnu-binutils
- aarch64-linux-gnu-gcc
- aarch64-linux-gnu-glibc
- aarch64-linux-gnu-linux-api-headers
- qemu-user

Set environment:
```shell
# C and C++ compilers
export CC=aarch64-linux-gnu-gcc
export CXX=aarch64-linux-gnu-g++

# Force static linkage of libraries
export CFLAGS=-static
export CXXFLAGS=-static
```

Build process as usual:
```shell
# Usual build process
cmake ..
make
```

## Cross-compilation for Android

Set environment:
```shell
# Set Android ABI and targeted platform
export ANDROID_ABI=arm64-v8a
export ANDROID_PLATFORM=android-28

# Set NDK paths (Linux)
export ANDROID_NDK=${HOME}/Android/sdk/ndk/${NDK_VERSION}/
# Set NDK paths (macOS)
export ANDROID_NDK=${HOME}/Library/Android/sdk/ndk/${NDK_VERSION}/

# CMake toolchain file
export CMAKE_TOOLCHAIN_FILE=${ANDROID_NDK}/build/cmake/android.toolchain.cmake
```

Build process as usual:
```shell
# Usual build process
cmake ..
make
```

## FILE vtable hijack

```c
FILE *file = ...;
void *vtable = file + offset + sizeof(FILE)
```

Remarks:
- vtable is directly after `FILE` if no offset
- take care of magic number as it contains flags (for `vfprintf`,  `0xfbad2185` is working)
- take care of lock address, as it can change with different systems

## System call tricks

`mprotect` can return -1, but still set some permissions:
```c
mprotect(0xffff0000, 0x0000ffff, 0x00000007);
```

`system` can be called even if command is not totally valid:
```c
system(0xdeadbeef + "; <COMMAND> #")
```
