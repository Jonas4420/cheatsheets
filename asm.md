# Assembly (x86)

## Flags

- CF: carry flag, set on high-order bit carry, cleared otherwise
- ZF: zero flags, set if result is zero, cleared otherwise

## Jumps

```asm
" Jump if equal (ZF = 1)
je / jz
" Jump if not equal (ZF = 0)
jne / jnz

" Jump if below (CF = 1)
jb / jnae / jc
" Jump if below or equal (CF = 1, ZF = 1)
jbe / jna
" Jump if above or equal (CF = 0)
jae / jnb / jnc
" Jump if above (CF = 0, ZF = 0)
ja / jnbe
```

## Tests

The `test X, Y` instruction is equivalent to:
```c
tmp = X & Y;
SF = MSB(tmp);
ZF = (tmp == 0) ? 1 : 0;
PF = Parity(tmp[0:7]);
CF = 0;
OF = 0;
```

The side effect is that it can be used to test if a value is zero:
```asm
" X & X == X => 
" if X == 0: ZF = 1
" else     : ZF = 0
test X, X
" Semantically Equivalent (maybe less optimized)
cmp X, 0
```

Whereas the `cmp X, Y` instruction is equivalent to:
```c
tmp = X - SignExtend(Y);
// The CF, OF, SF, ZF, AF, and PF flags are set in the same manner as with sub.
ModifyFLAGS();
```

## Load Effective Address

```asm
" dst = base + (index * mult) + offset
lea dst, [base + index * mult + offset]
```

Base and index must be registers.  Multiplier can be 1, 2, 4, or 8.

The instruction does not modify `FLAGS`, which can be useful for some arithmetic operations.
