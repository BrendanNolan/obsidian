# Two's Complement

## Negation Modulo a Power of 2

For unsigned `x` and a power of two `P = 2^k`, the negative of `x` modulo `P` is:

```cpp
(-x) & MASK  // where MASK = P - 1
```

This works because unsigned negation wraps modulo 2^N (giving the two's complement), and `& MASK`
extracts the low k bits, which is equivalent to mod P. The result is the unique value in `[0, P)`
such that `(x + result) % P == 0` i.e. `(x + result) & MASK == 0`.

Equivalently: `(~x + 1) & MASK`, which is just the two's complement spelled out.

This relies on C++ guaranteeing that unsigned arithmetic wraps modulo 2^N (where N is the bit
width). `-x` on an unsigned type produces `2^N - x`, and `& MASK` then reduces that to `2^k - x`.
Without this guarantee (e.g. with trap-on-overflow or arbitrary precision), `-x` wouldn't produce a
useful bit pattern.
