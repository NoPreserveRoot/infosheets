<h1 align="center">Rust</h1>

# Resources
- [The Rust Book](https://doc.rust-lang.org/stable/book/)
    - Written by Steve Klabnik and Carol Nichols with contributions from the community
- [/u/somebodddy's explanation of OOP in Rust](https://web.archive.org/web/20240403022822/https://old.reddit.com/r/rust/comments/d7w6n7/is_it_idiomatic_to_write_setters_and_getters/f15ib88/)
    - Explains why setters and getters are rare (and kind of a bad idea) in idiomatic Rust
- [The Rustonmonicon](https://doc.rust-lang.org/nomicon/intro.html)
    - Dives into the dark arts of writing unsafe Rust. Borrow checker? Memory safety? Who needs thats? In all seriousness, this book will teach you a _lot_ about Rust and why you might want to use certain unsafe features (such as type coercion).

# Definitions
Just some terms that people reading this may not be familiar with.
- Panic — an unrecoverable error. Basically, something went wrong when we didn't anticipate it and the program needs to go bye bye.

# Table of Contents
1. [Variables](#variables)
1. [Primitives](#primitives)
1. [References](#references)
1. [Operators](#operators)
1. [Vectors](#vectors)
1. [Flow Control](#flow-control)
1. [Loops](#loops)
1. [Options](#options)
1. [Results](#results)
1. [Functions](#functions)
1. [Macros](#macros)
1. [Structs](#structs)
1. [Enums](#enums)
1. [Traits](#traits)
1. [Impl blocks](#impl-blocks)
1. [Comments](#comments)
1. [Crates and Modules](#crates-and-modules)
1. [Example](#example)

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

### Slice
Slices allow you to reference a contiguous sequence of elements in a collect (array, vector, String, et cetera). Their size is not known at compile time, so it's possible for them to cause a panic when indexed out of bounds.

Example:
```rust
let val = String::from("Rust is cool!");

let slice = &val[0..4]; // let slice: &str = "Rust";
```

This also shows the "string slice" type, which is written as `&str`. This is also the implicit type of string literals

Example:

```rust
let val = "Hello, world!"; // Type is &str
```

# References
References are sort of like pointers; they point to an address in memory. However, references are checked by the borrow checker at compile time, so they are always guaranteed to point to valid data. So long, null pointer exceptions! Cloning data is an expensive operation ($O(n)$), so using references can help you save memory and execution time.

```rust
fn access_ref(string_ref: &String) {
    println!("Woah, I was able to find this so much faster thanks to references! {}", string_ref);
}

fn main() {
    let data: String = String::from("Cloning is O(n). Ew.〈⋟﹏⋞〉川");
    // Accessing data via a reference is MUCH faster and allows us to keep
    // it in scope. Passing data itself would move its value into access_ref,
    // making it unavailable after access_ref.
    access_ref(&data);
}
```

# Operators
Rust has quite a few operators, but the ones you're most likely to need are:

| Operator | Use |
| -------- | --- |
| !        | Bitwise or logical complement |
| &        | Borrow or bitwise AND |
| +        | Arithmetic addition   |
| -        | Arithmetic subtraction or negation |
| *        | Arithmetic multiplication |
| /        | Arithmetic division       |
| %        | Modulo |
| ..       | Range |
| &&       | Logical AND |
| \|\|     | Logical OR |

A complete list can be found [here](https://doc.rust-lang.org/book/appendix-02-operators.html).

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

# Loops
### `for` loops
Rust has `for` loops like many other languages.

Example of a "for each" loop:
```rust
let arr: [u8; 5] = [0, 1, 2, 3, 4];

for i in arr {
    println!("{}", i);
}
```

Example of a "typical" for loop (ex. `for (int i = 0; i < 20; i++)` in Java):
```rust
for i in 0..20 {
    println!("{}", i);
}
```

### `while` loops
`while` loops are pretty straightforward.

Example:
```rust
let mut i: u8 = 20;
while i > 0 {
    println!("{}", i);
    i -= 1;
}
```

### `loop` loops
A better way to write this might be "loop expression," but I think "loop loops" sounds way funnier. These are basically infinite loops. They run until broken.

Example:

```rust
loop {
    print!(":D "); // Print happy faces until interrupted
}
```

### `while let` loops
Runs until a tested pattern evaluates to false. These can be very useful when working with stacks, for example.

Example:
```rust
let mut stack = vec![1, 2, 3, 4, 5, 6, 7, 8, 9];

while let Some(val) = stack.pop() {
    println!("{}", val);
}

// stack is empty after the loop completes
```

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

# Results
Results are similar to Options, but they can contain an "Ok" value or an "Err" value. These can be used for error handling without panicking. Results can also be used in `if let` statements.

Example:

```rust
let result = fn_that_may_error();

match result {
    Ok(val) => println!("{}", val),
    Err(err) => println!("{}", err.to_string());
}
```
# Functions
Coming from a language like Java, C, or C++, declaring functions in Rust seems kinda funky. To create a function called `print_n_times`, we would write the following:
```rust
fn print_n_time(count: u32, val: String) {
    for _ in 0..count {
        println!("{val}");
    }
}
```

To create a function that accepts someone's name and returns a string that says "Hello, {your_name}":

```rust
fn hello_name(name: String) -> String {
    format!("Hello, {}!", name)
}
```

**Notice that I didn't write `return` in there.** The last expression in a function can be written without a semicolon to return its value. The `return` keyword, can be used to do an early return (ex. in an `if` statement) but it can also be used at the end of a function. Whether or not you use the `return` keyword at the end is essentially a stylistic choice.

# Macros
You may have noticed that certain functions end with `!`. This is because they aren't functions, they're macros. Macros are essentially "code that writes code for you." A few common macros that you'll likely need to use are:

| Macro | Use |
| ----- | --- |
| println! | Print a line of text using a format and specified arguments |
| print!   | The same thing as println! but without a newline |
| vec!     | Create a vector from specified values |

# Structs
In Rust, a `struct` is used to hold data. They are fairly straightfoward and by themselves, contain no functionality. Without an implementation, a `struct` can instantiated by specifying concrete values for the `struct`'s fields.

The concept of getters and setters is handled a bit differently in Rust than in languages like Java and C++. For more information, see [this explanation](https://web.archive.org/web/20240403022822/https://old.reddit.com/r/rust/comments/d7w6n7/is_it_idiomatic_to_write_setters_and_getters/f15ib88/).

Example:

```rust
struct Dog {
    name: String,
    owner: String,
    age: u8
}

let fido = Dog { name: String::from("Fido"), owner: String::from("Abraham Lincoln"), age: 8 };
```

# Traits
Traits in Rust are a bit like interfaces in Java. They are used to specify shared behavior with types.

Example:

```rust
pub trait Animal {
    fn make_sound(&self) -> String;
}

struct Dog {
    pub name: String
}

struct Cat {
    pub name: String
}

impl Animal for Dog {
    fn make_sound(&self) -> String {
        String::from("Woof!")
    }
}

impl Animal for Cat {
    fn make_sound(&self) -> String {
        String::from("Meow!")
    }
}

fn main() {
    let cat = Cat { name: String::from("Mr. Bigglesworth") };
    let dog = Dog { name: String::from("Fido") };

    println!("{} and {} say {} and {}",
            cat.name,
            dog.name,
            cat.make_sound(),
            dog.make_sound()
            );
}
```

### `derive` keyword
When creating a `struct`, you can `derive` certain traits that automatically add their behavior to that `struct`.

Example:

```rust
#[derive(Debug)]
struct Dog {
    name: String,
    is_female: bool
}
```

By deriving the `Debug` trait for the `Dog` `struct`, it is possible to print the Dog without writing a `to_string()` function. However, when using a `struct` that derives `Debug` in a formatter (`format!`, `println!`, et cetera), you must specify it in the format a bit differently.

```rust
#[derive(Debug)]
struct Dog {
    name: String,
    is_female: bool
}

fn main() {
    let dog = Dog { name: "Princess".to_string(), is_female: true };
    println!("{:?}", dog); // Standard Debug print
    println!("{:#?}", dog); // Pretty Debug print
}
```

# Impl blocks
In Rust, behavior for a struct is defined in its `impl` block.

Example:

```rust
struct Dog {
    name: String,
    is_female: bool
}

impl Dog {
    pub fn new(name: String, is_female: bool) -> Self {
        Dog {
            name,
            is_female
        }
    }
}
```

Note that if a parameter in a constructor function has a different name than the `struct`'s field, it has to be specified explicitly:

```rust
struct Dog {
    name: String,
    is_female: bool
}

impl Dog {
    pub fn new(dog_name: String, female: bool) -> Self {
        Dog {
            name: dog_name,
            is_female: female
        }
    }
}
```

Any member functions must have either `&self` or `&mut self` as a parameter. This doesn't need to be specified when calling the function since the compiler does it for you automatically.

```rust
struct Dog {
    name: String,
    is_female: bool
}

impl Dog {
    pub fn new(name: String, is_female: bool) -> Self {
        Dog {
            name,
            is_female
        }
    }

    pub fn say_hi(&self) {
        println!("{} says hi!", self.name);
    }

    pub fn make_a_knight(&mut self) {
        let title = if self.is_female { "Dame" } else { "Sir" };
        self.name = format!("{} {}", title, self.name);
    }
}

fn main() {
    // Variable must be declared as mutable to use functions
    // that mutate its values
    let mut dog = Dog::new("Princess".to_string(), true);
    dog.say_hi();
    dog.make_a_knight();
    dog.say_hi();
}
```

# Lifetimes
Rust doesn't have a garbage collector. Memory is freed when it is no longer referenced in the program, and references are checked at compile time. Because of this, we sometimes need to define lifetimes that tell the compiler "hey, this data needs to live at least as long as this other piece of data." Lifetime parameters are created using `<'{identifier}>` and the lifetime created is referenced using `&'{identifier}`. Lifetimes are definitely one of the most complex aspects of the language, but once you learn how to use them, they feel natural and make writing secure, fast, efficient code SO much easier.

More information on lifetimes can be found in the book [here](https://doc.rust-lang.org/stable/book/ch10-03-lifetime-syntax.html#validating-references-with-lifetimes).

```rust
fn longer_string<'a>(s1: &'a str, s2: &'a str) -> &'a str {
    if s1.len() > s2.len() { s1 } else { s2 }
}

fn main() {
    let string1 = String::from("hello");
    let string2 = String::from("world");

    let longer = longer_string(&string1, &string2);
    println!("Longer string is: {}", longer);
}
```

# Comments
Comments in Rust are pretty much the same as Java, C, C++, et cetera.

Single line comment:
```rust
// woah a single line comment
```

Multiline comment:
```rust
/*
woah
look
a
multiline
comment
*/
```

# Crates and Modules
In Rust, modules and crates (similar to packages in Java) can be imported into a file for use. The syntax for this can be found in the example below. Note that `::` is used to traverse paths for "imports" rather than `.` or `/`.

Example:
```rust
// Multiple packages can be imported explicitly in one use
// statement using curly braces
use std::{io, fs};
// Using the Regex object from the regex crate
// NOTE: regex must be added to your Cargo.toml
// before it can be used.
use regex::Regex;
// Use everything in Actix (generally not the best idea)
// NOTE: Actix is a web framework that allows you to easily
// create HTTP servers (great for REST APIs :D)
use actix_web::*;
```

# Example

```rust
fn average(nums: &[i32]) -> f64 {
    // Create a mutable variable called sum so we can
    // add to it. I chose i64 as the type since it's
    // a good idea to choose a type with twice the size
    // of whatever you're working with in cases like this.
    let mut sum: i64 = 0;
    // For loop iterating over nums
    for i in nums {
        // i has a type of &i32 and references can't be type cast,
        // so we use the * operator on i to dereference it. Casting
        // i from i32 to i64 helps to avoid integer overflows when
        // using unchecked operations.
        sum += *i as i64;
    }
    // Return the average as a 64 bit float
    sum as f64 / nums.len() as f64
}

// Check if a character is in the Latin alphabet
// using the .contains() method on a &str literal
fn is_alpha(c: char) -> bool {
    "abcdefghijklmnopqrstuvwxyzABCDEFHIJKLMNOPQRSTUVWXYZ".contains(c)
}

// Check if two vectors are equal
fn i32_vecs_eq(list1: Vec<i32>, list2: Vec<i32>) -> bool {
    // If their lengths are different, they're obviously
    // not equal.
    if list1.len() != list2.len() {
        return false
    }

    // Iterate over the indices of the lists
    for i in 0..list1.len() {
        // If two elements are unequal, return false early
        if list1[i] != list2[i] {
            return false
        }
    }

    // Otherwise, return true
    true
}

fn char_to_multiplier(c: char) -> u16 {
    // Set multiplier to a value returned by
    // the match block.
    let multiplier: u16 = match c {
        'a' => 65535,
        'b' => 4095,
        'c' => 255,
        'd' => 1,
        // _ would be the "default" case in Java
        _ => 0
    };

    // Return mutliplier
    multiplier
}

fn main() {
    // average() accepts a slice of i32s since an array's size
    // must be known at compile time, while the size of a slice
    // isn't necessary. A vec could could also be used here.
    println!("{}", average(&[1, 2, 4, 5, 6, 6, 2, 5, 7, 2,
        13, 5641, 5413641, 1]));

    println!("{}", is_alpha('b'));

    let v1: Vec<i32> = vec![1, 5, 4, 3, 6, 7];
    let v2: Vec<i32> = vec![5, 6, 4, 32, 7, 9];
    println!("{}", i32_vecs_eq(v1, v2));
}
```
