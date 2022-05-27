# Memo

- [Git](#git)
- [SSH](#ssh)
- [Find](#find)
- [Netcat](#netcat)
- [GDB](#gdb)
- [Objdump](#objdump)
- [Strace](#strace)
- [C / System](#c--system)
- [Rust](#rust)
- [Sage](#sage)
- [Assembly (x86)](#assembly-x86)
- [Steganography tools](#steganography-tools)

## Git

## Delete tag

```shell
# Delete local tag
git tag --delete TAGNAME
# Delete remote tag
git push --delete REMOTE TAGNAME
```

## SSH

#### Suspend session

```shell
# Needs to be immediatly after new line
~^Z
```

## Find

#### Search by filename

```shell
find . -name 'REGEX'
```

#### Search by file type

```shell
find . -type TYPE
```

Common types:
- `f`: regular file
- `d`: directory
- `l`: symbolic link
- ...

#### User / Group

```shell
find . -user USER
find . -group GROUP
```

`USER`/`GROUP` can be name or numeric ID

#### Size

```shell
# Exactly n units
find . -size n
# Greater than n units
find . -size +n
# Less than n units
find . -size -n
# MIN < size < MAX
find . -size +MIN -size -MAX
# Files less than 1X (but because of round up, so empty files only)
find . -size -1X
# Files less than 1024 * 1024 bytes (no round up)
find . -size -1048576c
```

Units (values are rounded up to unit, so it can be tricky)
- `c`: (bytes, *default*)
- `k`: (kilo)
- `M`: (mega)
- `G`: (giga)
- ...

#### Permissions

```shell
# Exact match
find . -perm MODE
# All bits of MODE are set
find . -perm /MODE
# Any bits of MODE are set
find . -perm -MODE
```

`MODE` can be octal (`640`, `0640`, ...) or symbolic (`u=rw`, `u=rw,g=r`, ...)

#### Execute something on matches

```shell
# Execute command on each file (\; to escape ';' in the shell)
find . -exec COMMAND \;
# Group matching files, and run command on the group
find . -exec COMMAND +
# COMMAND can use the filename with {}
find . -exec cat '{}' \;
```

#### Execute something on matches with xargs

```shell
# To run on group of matched files
find . -print0 | xargs -0 COMMAND
# To split files by group of 1
find . -print0 | xargs -0 -n 1 COMMAND
# To allow using filename with {}
find . -print0 | xargs -0 -n 1 -I {} COMMAND
```

## Netcat

#### Connect

```shell
nc HOSTNAME PORT
```

#### Listening

```shell
# Linux
nc -l -p PORT
# BSD
nc -l PORT
```

#### Listen UDP (IP address only, hostname doesn't work)

```shell
nc -l -u ADDRESS PORT
```

## GDB

#### Usual commands
- `run`
- `continue`
- `quit`
- `si`/`ni`: next asm instruction (step goes inside functions)
- `step`/`next`: next high-level instruction (step goes inside functions)

#### Sane assembly flavor

```shell
set disassembly-flavor intel
```

#### Show next assembly instruction at each step

```shell
display /i $pc
```

#### List functions

```shell
info functions
info functions REGEX
```

#### Breakpoints

```shell
# List
info break
# Break at symbol
break SYMBOL
# Break at address
break *ADDRESS
# Break at symbol + offset address
break *SYMBOL+OFFSET
# Delete breakpoint N
del break N
```

#### Disassemble

```shell
# Current address
disas
# Symbol
disas SYMBOL
# Address
disas ADDRESS
```

#### List register content

```shell
info reg
```

#### Stack frames

```shell
# List all stack frames
bt
# Info of current frame
info frame
```

#### Reading memory / values

```shell
# Explore memory
x /FMT ADDRESS
# Print stuff (function, variable, memory address, ...)
p /FMT STUFF
```

#### Write memory

```shell
set DEST = VALUE
```

`DEST` and `VALUE` can be of the following:
- `VALUE`: direct value
- `$REG`: register
- `*ADDRESS`: memory address

#### Bindings after `fork`

```shell
# Attaches to both processes after fork or vfork
set detach-on-fork off
# Parent is debugged
set follow-fork-mode parent
# Child is debugged
set follow-fork-mode child
```

#### Bindings after `exec`

```shell
# Create new inferior
set follow-exec-mode new
# Re-use same inferior
set follow-exec-mode same
# List inferiors
info inferior
# Switch to inferior N
inferior N
```

#### Sections

```shell
maintenance info sections
```

## Objdump

#### List symbols

```shell
# Symbols
objdump -t
# Dynamic symbols
objdump -T
```

#### Disassemble

```shell
# Disassemble symbol only
objdump --disassemble=SYMBOL
# Disassemble all sections
objdump -D
```

## Strace

#### Trace only some syscalls

```shell
# Comma separated list of syscalls
strace --trace=LIST PROGRAM ARGS
```

## C / System


### Disable ASLR

```shell
# Off
echo 0 | /proc/sys/kernel/randomize_va_space
# On (conservative randomization)
echo 1 | /proc/sys/kernel/randomize_va_space
# On (full randomization)
echo 2 | /proc/sys/kernel/randomize_va_space
```

For permanent changes, add a configuration file `/etc/sysctl.d/01-disable-aslr.conf`:
```shell
kernel.randomize_va_space = 0
```

#### No PIC

```shell
gcc -m32 -no-pie -fno-pic -o ...
```

#### LD_PRELOAD and setuid programs

```shell
gcc -m32 -shared -fPIC <FAKE LIB>.c -o <FAKE_LIB>.so -ldl
```

#### glibc version

```shell
getconf GNU_LIBC_VERSION
```

#### Unset randomization of pointers for setjmp for ld.so if glibc < 2.23

```shell
LD_POINTER_GUARD=0
```

#### Check for NX bit set (check if RWE present)

```shell
readelf -a FILE | grep GNU_STACK
```

#### Debug file descriptor

```shell
cat /proc/$PID/fdinfo/$FDNO
```

#### System call tricks

`system` is okay even if command is not totally valid:
```c
system(0xdeadbeef + "; $COMMAND #")
```

`mprotect` for setting the stack executable, will return -1 but still processed:
```c
mprotect(0xffff0000, 0x0000ffff, 0x00000007);
```

#### File Stream overload

```asm
" 0x94 = sizeof(FILE)
" esi = base of FILE struct
" eax = _vtable_offset
_vtable_offset = lea 0x94($esi, $eax, 1)
```
- vtable is directly after `FILE` if no offset
- take care of magic number as it contains flags (for `vfprintf`,  `0xfbad2185` is working)
- take care of lock address, as it can change with different systems

## Rust

#### Emit assembly

```shell
cargo rustc --emit asm
```

Works also for `MIR`, `llvm-ir`, ...

#### Print target info

```shell
rustc +nightly -Z unstable-options --print target-spec-json --target TARGET
```

#### Print linker info

```shell
RUSTC_LOG=rustc_codegen_ssa::back::link=info cargo build -vv
```

## Sage

#### Finite field GF(2)[X]/(P)

```python
# Create finite field
K.<X> = GF(2^128, modulus = x^128 + x^127 + x^126 + x^121 + 1)
# Compute reduction
X^-128
```

## Assembly (x86)

#### Load Effective Address

```asm
" reg2 = reg1 * offset + x
lea x(,reg1,offset), reg2
```

#### Test if 0

```asm
" X & X == 0 <=> X == 0 => ZF = 0
test X, X
" Semantically Equivalent (maybe less optimized)
cmp X, 0
```

#### Jump if ZF == 0

```asm
jne
```

## Steganography tools

- `outguess`
- `steghide`
- `jphide`/`jpseek`: Linux and Windows versions are differents
- `stegdetect`: staganalysis tool
- `stegbreak`: seems to have `SEGFAULT`...
