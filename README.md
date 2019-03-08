# idiomatic-rust

## Unused variables
Prepend unused variables with underscore. This will also silence compiler warning.
```rust
fn main() {
    let _greeting = "Hello, World!";
}
```

```rust
fn greet(_name: &str) {
    println!("Hello, World!");
}

fn main() {
    greet("Good Afternoon");
}
```

## Function or method arguments
### Prefer `&str` to `&String` as function or method  arguments


## `use`
### `use` Paths for functions
Specify the parent module of the function with `use`. Then, call the function by prefixing with the parent module.

```rust
mod places {
    pub mod country {
        pub fn is_africa(name: &str) -> bool {
            true
        }
    }
}

use crate::places::country;

fn main() {
    country::is_africa("Nigeria");
}
```

### `use` Paths for structs, enums and other items
Specify the full path to structs, enums and other items with `use`.

```rust
mod places {
    pub mod country {
        pub struct Country {
            pub name: String
        }
    }
}

use crate::places::country::Country;

fn main() {
    let _country = Country {name: String::from("Nigeria")};
}
```

### `use` Paths for multiple structs, enums or other items with similar names
1 Specify the parent module of the item with `use`.

```rust
use std::fmt;
use std::io;

fn fun1() -> fmt::Result {
}

fn fun2() -> io::Result {
}
```

2 Or, rename the type with `as`
```rust
use std::fmt;
use std::io as IoResult;

fn fun1() -> fmt::Result {
}

fn fun2() -> IoResult {
}
```

### Re-export an item brought into scope with `use` by prefixing the `use` statement with `pub`
```rust
mod places {
    pub mod country {
        pub fn is_africa(name: &str) -> bool {
            true
        }
    }
}

mod people {
    pub use crate::places::country;

    pub fn is_an_african(country_name: &str) -> bool {
        country::is_africa(country_name)
    }
}

fn main() {
    let country = "Nigeria";
    people::is_an_african(country);
    people::country::is_africa(country);
}
```

### Bring multiple items with shared path into scope with nested `use`
```rust
use std::cmp::Ordering;
use std::io;
```

could be re-written as

```rust
use std::{cmp::Ordering, io};
```

### Minimize the use of `use` with glob operator because it's harder to keep track of names that are brought into scope and where those names are defined. If you can, avoid them.
```rust
use std::io::*;
```
The statement above brings all public items defined in `std::io` into local scope.


## Error handling
### Minimize nested matching during error handling by taking advantage of the various `Result<T, E>` methods and closures
Some of these methods are `map_err`, `unwrap_or_else`

### Between `expect` and `unwrap` methods of `Result<T, E>`
Both `expect` and `unwrap` return the value in the `Ok(T)` variant or call `panic!` for the `Err(E)`. However, choose `expect` over `unwrap` because it allows you to specify a panic error message which could potentially improve the documentation/usability of your code and make debugging easier. 

### Propagate errors with `?` operator placed after a `Result` value. The `?` can only be used within functions that return `Result<T, E>`.
