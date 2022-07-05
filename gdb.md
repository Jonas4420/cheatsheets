# GDB

## Commands

```shell
# Start program
run
# Result execution
continue
# Quit GDB
quit

# Next assembly instruction (goes inside function)
si
# Next assembly instruction (skip functions)
ni
# Show next assembly instruction at each step
display /i $pc
```

## Breakpoints

```shell
# List breakpoints
info break

# Break at symbol
break SYMBOL
# Break at symbol + offset address
break *SYMBOL+OFFSET
# Break at address
break *ADDRESS

# Delete breakpoint N
del break N
```

## Display functions / sections

```shell
# List functions
info functions
info functions REGEX

# List sections
maintenance info sections
```

## Disassemble

```shell
# Sane assembly flavor
set disassembly-flavor intel

# Current address
disas

# Symbol
disas SYMBOL
# Address
disas ADDRESS
```

## Read value

```shell
# List registers content
info reg

# Explore memory
x /FMT ADDRESS

# Print stuff (function, variable, memory address, ...)
p /FMT STUFF
```

## Write value

```shell
# Set memory address
set *ADDRESS = <VALUE>
# Set register
set $REG = <VALUE>
```

Value can either be:
- `VALUE`: a direct value
- `$REG`: the value in a register
- `*ADDRESS`: the value in memory

## Stack frames

```shell
# List all stack frames
bt

# Info of current frame
info frame
```

## Bindings after `fork`

```shell
# Attaches to both processes after fork or vfork
set detach-on-fork off

# Parent is debugged
set follow-fork-mode parent
# Child is debugged
set follow-fork-mode child
```

## Bindings after `exec`

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
