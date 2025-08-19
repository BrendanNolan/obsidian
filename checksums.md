# One's Complement 16-bit Addition

One's complement `16`-bit addition is a way to sum two `16`-bit numbers and get another `16`-bit
number. To sum two `16`-bit integers with one's complement addition, just sum the numbers normally
and denote the sum by `x`:

- If `x<2^16`, then `x` is your answer
- If `x>=2^16`, then notice that `2^16<=x<2^17` and so `x`'s binary representation has a `1` as its
  most significant (`17`th) bit. To get the one's complement sum, simply replace this `1` with a `0`
  (drop the high bit) and add `1` to the result. You may again have a number that is `>=2^16`, in
  which case you repeat until you do have a number that is `<2^16` . This process is called
  `"end-around carry"` .
