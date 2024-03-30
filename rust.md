<div style="text-align: center">
<h1>Rust</h1>
</div>

# Primitives
### Integers
Rust's standard library contains 8, 16, 32, 64, and 128-bit integers. All integers are either signed or unsigned. Signed integers can be negative, whereas unsigned integers can only be positive.
```rust
// Unsigned integers
u8
u16
u32
u64
u128

// Signed integers
i8
i16
i32
i64
i128
```

There is also an architecture-dependent integer type called `usize`. `usize` is 64 bits on 64-bit systems and 32 bits on 32-bit systems.

Additionally, Rust allows you to specify integer literals in a variety of notations. Here's a table with some examples taken from [the official Rust book](https://doc.rust-lang.org/book/ch03-02-data-types.html). Commas cannot be used when specifying integer literals, but underscores can be used instead to make them easier to read.

| Number literals | Example |
| --------------- | ------- |
| Decimal         | 98_222  |
| Hex             | 0xff    |
| Octal           | 0o77    |
| Binary          | 0b1111_0000 |
| Byte (u8 only)  | b'A'    |

### Floating-point numbers
Rust's standard library includes 32-bit and 64-bit floating-point numbers.

Example:
```rust
let float: f32 = 13.37;
```

### Booleans
As with most programming languages, Rust has a boolean type which can be either `true` or `false`. It is specified using the `bool` type.

Example:
```rust
let boolean: bool = false;
```

### Characters
Like many other programming languages, Rust contains a `char` type. It is four bytes wide and can hold any Unicode characters.

Example:
```rust
let c: char = '№';
```

### Tuples
Python lovers, rejoice! Rust has tuples. In Rust, you can bundle together various data types into a single unit using tuples.

Example:
```rust
let woah_tuples_are_neat: (u32, i8, f32, char) = (5318416, -100, 3.14159265359, '☻');
```

### Arrays
Unsurprisingly, Rust also has arrays. They are fixed-length and homogeneous (can only hold one data type), which makes them very efficient for certain use cases.

Example:
```rust
let array: [char; 11] = ['h', 'e', 'l', 'l', 'o', ' ', 'w', 'o', 'r', 'l', 'd'];
```
