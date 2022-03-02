# Pointer

## Box

Common Usage:

- Allocate any data to the heap (even `i32`)

- Fix size

    ```rust
    enum List {
        Cons(i32, Box<List>),
        Nil,
    }
    ```

- Trait object

    ```rust
    let elems: Vec<Box<dyn Draw>> = vec![Box::new(Button { id: 1 })];
    ```

Use `*` to deference:

```rust
let arr = vec![Box::new(1), Box::new(2)];
// container[index] is actually syntactic sugar for
// *container.index_mut(index) or *container.index_mut(index)
let (first, second) = (&arr[0], &arr[1]);
// first: &Box<i32>
// *first: Box<i32>
// **first: i32
let sum = **first + **second;
```

`Box::leak` Initialize a global value at runtime:

- note that only `'static` annotation is just for compilers; it will still be freed if it is out of scope
- But `Box::leak` can convert a runtime value into real `'static`

```rust
fn main() {
   let s = gen_static_str();
   println!("{}", s);
}

fn gen_static_str() -> &'static str{
    let mut s = String::new();
    s.push_str("hello, world");

    Box::leak(s.into_boxed_str())
}
```

## Deref

`*box` is actually: `*(box.deref())`

```rust
use std::ops::Deref;

struct MyBox<T>(T);

impl<T> Deref for MyBox<T> {
    type Target = T;

    fn deref(&self) -> &Self::Target {
        &self.0
    }
}

impl<T> DerefMut for MyBox<T> {
    fn deref_mut(&mut self) -> &mut Self::Target {
        &mut self.0
    }
}
```

## Drop

`pub fn drop<T>(_x: T)` in `std::mem::drop` takes away the ownership

cannot implement both `Copy` and `Drop` for the same type

## Rc and Arc

Both of them are **immutable** reference to data.

They need `RefCell` to modify data.

### Reference Counting

- `Rc<T>` is a **immutable** reference to a value
- it will be dropped if nobody owns it
- Enable multiple owners for a single data

```rust
use std::rc::Rc;
fn main() {
    // a smart pointer to the string
    let a = Rc::new(String::from("hello, world"));
    println!("count after creating a = {}", Rc::strong_count(&a));
    // "clone" is not a "deepcopy" here; only the pointer is copied!
    let b = Rc::clone(&a);
    println!("count after creating b = {}", Rc::strong_count(&a));
    {
        let c =  Rc::clone(&a);
        println!("count after creating c = {}", Rc::strong_count(&c));
    }
    println!("count after c goes out of scope = {}", Rc::strong_count(&a));
}
```

### Atomic RC

- thread safety version of RC



### Cell and RefCell

Cell: **Enables *inner* mutability**

- `Cell<T>` for `T: Copy`

- get, set or get ref by `&`

    ```rust
    let c = Cell::new("asdf");
    // changing c will not change one because get creates a copy
    let one = c.get();
    c.set("qwer");
    let two = c.get();
    println!("one: {}, two: {}", one, two);
    // one: asdf, two: qwer
    println!("addr. of one: {:p}, addr. of two: {:p}", &one, &two);
    // addr. of one: 0x7ff7b81fa630, addr. of two: 0x7ff7b81fa640
    
    let d = &c; // d and f are references to the Cell c
    let f = d; // f is a copy of d
    f.set("zxcv"); // will change c and d
    println!("c: {}, d: {}, f: {}", c.get(), d.get(), f.get());
    // c: zxcv, d: zxcv, f: zxcv
    println!("addr. of c: {:p}, addr. of d: {:p}, addr. of f: {:p}", &c, &d, &f);
    // addr. of c: 0x7ff7b00d8508, addr. of d: 0x7ff7b00d85e8, addr. of f: 0x7ff7b00d85f0
    ```

RefCell:

- Postpone the rule of borrowing from compilation stage to runtime stage (may panic)

    ```rust
    let s = RefCell::new(String::from("hello, world"));
    let s1 = s.borrow();
    let s2 = s.borrow_mut(); // panic here: already borrowed: BorrowMutError
    println!("{},{}", s1, s2);
    ```

- Combine with `Rc`: `Rc` enables multiple owners, and `RefCell` enables mutability

    ```rust
    let p = Rc::new(RefCell::new(String::from("hello")));
    println!("{:?}", p);
    let p1 = Rc::clone(&p);
    let p2 = Rc::clone(&p);
    p1.borrow_mut().push_str(" world");
    
    println!("c: {:?}, c1: {:?}, c2: {:?}", Rc::strong_count(&p), Rc::strong_count(&p1), Rc::strong_count(&p2));
    // c: 3, c1: 3, c2: 3
    println!("p: {:?}, p1: {:?}, p2: {:?}", p, p1, p2);
    // p: RefCell { value: "hello world" }, p1: RefCell { value: "hello world" }, p2: RefCell { value: "hello world" }
    ```

    

- 

