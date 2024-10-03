# Getting iterators out of sentinels

If you have a cpp range or view, you can use `std::ranges::views::common` or
`std::ranges::common_view` to get a view which will let you call `.begin()` and `.end()` to get
begin and end iterators (rather than a begin iterator and an end "sentinel") that you can use as
normal.

# `std::views::filter` and `std::views::transform`

In the code below, `evens` will be a view consisting of "2" and "4":

```cpp
using std::views::filter;
using std::views::transform;
const auto v = std::vector{1, 2, 3, 4};
const auto evens = v | filter([](const auto i) { return i % 2 == 0; })
    | transform([](const auto e) { return std::to_string(e); });
```
