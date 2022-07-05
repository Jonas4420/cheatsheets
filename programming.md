# Programming

- [Rust](#rust)
- [Sage](#sage)

# Rust

## Emit assembly

```shell
cargo rustc --emit asm
```

Works also for `MIR`, `llvm-ir`, ...

## Print target info

```shell
rustc +nightly -Z unstable-options --print target-spec-json --target <TARGET>
```

## Print linker info

```shell
RUSTC_LOG=rustc_codegen_ssa::back::link=info cargo build -vv
```

# Sage

## Finite field GF(2)[X]/(P)

```python
# Create finite field
K.<X> = GF(2^128, modulus = x^128 + x^127 + x^126 + x^121 + 1)

# Compute reduction
X^-128
```
