# Project Related

```
.
├── Cargo.toml
├── Cargo.lock
├── src
│   ├── main.rs
│   ├── lib.rs
│   └── bin
│       └── main1.rs
│       └── main2.rs
├── tests
│   └── some_integration_tests.rs
├── benches
│   └── simple_bench.rs
└── examples
    └── simple_example.rs
```

## crate and mod

mod tree:

```
crate
 └── dining
     └── eat_at_restaurant
 └── front_of_house
     ├── hosting
     │   ├── add_to_waitlist
```

- `pub`, `self`, `super`:

    ```rust
    mod front_of_house {
        pub mod hosting {
            pub fn add_to_waitlist() {}
        }
    }
    pub fn eat_at_restaurant() {
        // absolute path
        crate::front_of_house::hosting::add_to_waitlist();
        // relative path
        front_of_house::hosting::add_to_waitlist();
    }
    
    fn serve_order() {
        self::back_of_house::cook_order();
    }
    
    mod back_of_house {
        fn fix_incorrect_order() {
            cook_order();
            super::serve_order();
        }
        pub fn cook_order() {}
    }
    ```

- use mod in other files:

    ```rust
    mod front_of_house; // load this mod in the file of the same name
    
    pub use crate::front_of_house::hosting;
    
    pub fn eat_at_restaurant() {
        hosting::add_to_waitlist();
    }
    ```

- use multiple things:

    ```rust
    use std::collections::{HashMap, BTreeMap, HashSet};
    use std::io::{self, Write};
    use std::collections::*;
    ```

- limited visibility: `pub(crate)` or `pub(crate::a)`

    ```rust
    pub mod a {
        pub const I: i32 = 3;
    
        fn semisecret(x: i32) -> i32 {
            use self::b::c::J;
            x + J
        }
    
        pub fn bar(z: i32) -> i32 {
            semisecret(I) * z
        }
        pub fn foo(y: i32) -> i32 {
            semisecret(I) + y
        }
    
        mod b {
            pub(in crate::a) mod c {
                pub(in crate::a) const J: i32 = 4;
            }
        }
    }
    ```

- 

## Comments

- coding comments

    ```rust
    //
    /*
    */
    ```

- doc comments in **lib** type crates for **functions or structs**; markdown supported!

    ````rust
    /// `add_one` 将指定值加1
    ///
    /// # Examples
    ///
    /// ```
    /// let arg = 5;
    /// let answer = my_crate::add_one(arg);
    /// assert_eq!(6, answer);
    /// ```
    pub fn add_one(x: i32) -> i32 {
        x + 1
    }
    
    
    /** `add_two` 将指定值加2
    # Examples
    ```
    let arg = 5;
    let answer = my_crate::add_two(arg);
    assert_eq!(7, answer);
    ```
    */
    pub fn add_two(x: i32) -> i32 {
        x + 2 
    }
    ````

- comments for crates or mods on the **top** of source files:

    ```rust
    //!
    /*!
    */
    ```

- use complete path to call functions in **Doc-tests** (`cargo test`):

    ```rust
    /// `add_one` 将指定值加1
    /// # Examples11
    /// ```
    /// let arg = 5;
    /// let answer = world_hello::compute::add_one(arg);
    /// assert_eq!(6, answer);
    /// ```
    pub fn add_one(x: i32) -> i32 {
        x + 1
    }
    ```

    `should_panic`:

    ```rust
    /// # Panics
    /// The function panics if the second argument is zero.
    /// ```rust,should_panic
    /// world_hello::compute::div(10, 0);
    /// ```
    pub fn div(a: i32, b: i32) -> i32 {
        if b == 0 {
            panic!("Divide-by-zero error");
        }
        a / b
    }
    ```

- hide some codes by `#`:

    ```rust
    /// ```
    /// # // 使用#开头的行会在文档中被隐藏起来，但是依然会在文档测试中运行
    /// # fn try_main() -> Result<(), String> { 
    /// let res = world_hello::compute::try_div(10, 0)?;
    /// # Ok(()) // returning from try_main
    /// # }
    /// # fn main() { 
    /// #    try_main().unwrap(); 
    /// #                       
    /// # }
    /// ```
    pub fn try_div(a: i32, b: i32) -> Result<i32, String> {
        if b == 0 {
            Err(String::from("Divide-by-zero"))
        } else {
            Ok(a / b)
        }
    }
    ```

- ```rust
    /// [`Option`] can be used to jump to the doc of Option in std lib
    /// jump to [`Self::recv()`]
    ```

- jumping between the same names:

    ```rust
    /// 跳转到结构体  [`Foo`](struct@Foo)
    struct Bar;
    
    /// 跳转到同名函数 [`Foo`](fn@Foo)
    struct Foo {}
    
    /// 跳转到同名宏 [`foo!`]
    fn Foo() {}
    
    macro_rules! foo {
      () => {}
    }
    ```

- alias for searching:

    ```rust
    #[doc(alias = "x")]
    #[doc(alias = "big")]
    pub struct BigX;
    
    #[doc(alias("y", "big"))]
    pub struct BigY;
    ```

    

- 



