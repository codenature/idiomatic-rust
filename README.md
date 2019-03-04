# idiomatic-rust

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
    country::is_africa("Nigeria")
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
    let country = Country {name: String::from("Nigeria")}
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

