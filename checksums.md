# One's Complement `n`-bit Addition

One's complement `n`-bit addition is a way to sum two `n`-bit numbers and get another `n`-bit
number. It is probably best explained in code (we'll use `n==16` for the code):

```rust
fn ones_complement_sum(integers: &[u16]) -> u16 {
    // Chop off the high bits (if there are any) and add back in as low bits
    let fold = |x| (x & 0xFFFF) + (x >> 16);
    let mut result: u32 = 0;
    for &x in integers {
        result += x as u32;
        result = fold(result);
    }
    result as u16
}
```
