<div style="text-align: center">
<h1>Rust</h1>
</div>

# Variables
In Rust, variables are defined using the `let` keyword.

Variable definitions follow this format ([] denotes optional, <> denotes required):
```rust
let [mut] <name>[: type] = <value>;
```
`[: type]` is in brackets because a variable's type can be inferred from its value in many cases, in which case it is implicitly specified by the compiler. However, you can also explicitly specify a type using `: <type>`.
Example:
```rust
let explicit: u32 = 3223463;
```

Rust variables are immutable by default, so you must define them as mutable if you ever want to change their value.

Example:
```rust
// Define an immutable variable called "immutable" with a value of 9 and a type of i32
let immutable = 9;

// Define a mutable variable called "woah_mutable" with a value of 27 and a type of i32
let mut woah_mutable = 27;
```

In the previous example

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

# Vectors
Rust doesn't have an ArrayList type like in Java. Nor does it have a stack or queue. Instead, there's the (wonderful) vector, which is essentially those three things bundled into one absolute unit of a data type. Vectors are homogeneous, but they have a dynamic length and plenty of functions that make them great for a myriad of use cases.

Example:
```rust
// Create a vector using Vec::new()
let vec1: Vec<u8> = Vec::new();

// Create a vector with values using the vec! macro
let vec2: Vec<u8> = vec![0,1,2,3,4,5,6,7,8,9];

// Add a new value onto the end of vec1
vec1.push(27);

// Remove the last value in the vec1
let popped = vec1.pop();

// Remove the fourth value in vec1 (remember, indices start at 0)
let removed = vec1.remove(3);

// Insert a new value as the second element in vec1
vec1.insert(1, 28);

// Get the length of vec1
let length = vec1.len();

// Check if vec1 is empty
let empty = vec1.is_empty(); // false

// Access first element in vec1
let first_elem = vec1.first();

// Access last element in vec1
let last_elem = vec1.last();
```

# Flow control
### `if` statements
Unsurprisingly, Rust has `if` statements. Their syntax is fairly straightforward; extremely similar to that of Java, but without any annoying parentheses.

Example:
```rust
let x = 5;
if x > 20 {
    println!("x is greater than 20");
} else {
    println!("x is less than or equal to 20");
}
```

### `match` statements
Rust also has `match` statements, which are similar to `switch` statements in other languages. Instead of having to write `case` for each case, you just need to specify the case and use the "right arrow" operator to define what should be done. You can also match multiple values using the `|` operator and a range of values using the `..` operator. The `_ =>` case is basically the same as the `default` case in some other languages, and it's required for all `match` statements since they must be exhaustive.

Example:
```rust
let x = 5;
let result = match x {
    5 => "woohoo! x is 5!",
    9 | 10 => "x is 9 or 10! multiple matches are cool!",
    11..20 => "x is in the range of [11, 20]",
    21.. => "x is greater than or equal to 20",
    _ => {
        panic!("That's really weird! This shouldn't happen! AhhhhhHHHH!!!!");
    }
};
```
As shown above, `match` statements can be used to return a value *or* execute statements and functions. I chose to use `panic!()` here, which causes the program to terminate and print a stack trace, but you can using `match` statements to essentially avoid building a massive tree of `if else` statements.

# Options
One of Rust's biggest selling points is its memory safety. This means it *has* to avoid null pointers. But how?

The main way this is accomplished is by using the `Option<T>` enum, where `T` is a type. An Option can either contain the `Some(T)` enum member or the `None` enum member. Calling the `.unwrap()` function on an Option will return the value `T` inside if it is `Some` or panic if it is `None`.

Example:
```rust
let vector: Vec<u8> = vec![];
let another: Vec<u8> = vec![1,5,1,2];

let value = vector.pop(); // None
let value2 = another.pop(); // Some(2)

let unwrap = value2.unwrap(); // 2
let uhoh = value.unwrap(); // panic
```

### `let match`
There are a few better ways to handle `Options`. One of which is using pattern matching.

Example:
```rust
let vector: Vec<u8> = vec![];
let value = vector.pop(); // None

let val: u8 = match value {
    Some(x) => x,
    None => 0
};
```

### `if let`
However, Rust has another way of handling Options that's just *beautiful* once you get the hang of it. This is the `if let` statement.

Example:
```rust
let vector: Vec<u8> = vec![];
let value = vector.pop(); // None

if let Some(v) = value {
    println!("{}", v);
} else {
    println!("v is None, that's problematic!");
}
```
