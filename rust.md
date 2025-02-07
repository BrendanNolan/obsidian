# Nested Module Structure

## Using File Instead of mod.rs (Modern Approach)

As of Rust 2018, you can organise nested modules without using mod.rs. Instead, you use a file named
after the parent module. Here's how it works:

File Structure:

```css
src/
├── main.rs
├── my_module.rs
├── my_module/
│   └── sub_module.rs
```

`main.rs`:

```rust
mod my_module;

fn main() {
    my_module::sub_module::greet();
}
```

`my_module.rs`:

```rust
pub mod sub_module;
```

`my_module/sub_module.rs`:

```rust
pub fn greet() {
    println!("Hello from sub_module!");
}
```

### Explanation:

In `main.rs`, the `mod my_module;` statement tells Rust to look for a file named `my_module.rs` in
the `src` directory. Submodule Declaration:

In `my_module.rs`, the `pub mod sub_module;` statement tells Rust to look for a file named
`sub_module.rs` in the `my_module/` directory.

### Accessing Functions:

The greet function in `sub_module.rs` is marked as pub to make it accessible from outside the
module.
