# Phân cấp tập tin

Các module có thể được ánh xạ vào một cấu trúc tập tin/thư mục. Hãy phân tích ví dụ [visibility][visibility] trong các tập tin:

```shell
$ tree .
.
├── my
│   ├── inaccessible.rs
│   └── nested.rs
├── my.rs
└── split.rs
```

Trong tập tin `split.rs`:

```rust,ignore
// Khai báo này sẽ tìm tập tin tên là `my.rs` và sẽ
// chèn nội dung của nó vào trong một module tên `my` trong phạm vi này (scope).
mod my;

fn function() {
    println!("called `function()`");
}

fn main() {
    my::function();

    function();

    my::indirect_access();

    my::nested::function();
}

```

Trong tập tin `my.rs`:

```rust,ignore
// Tương tự `mod inaccessible` và `mod nested` sẽ tìm các tập tin `nested.rs`
// và `inaccessible.rs` và chèn chúng vào đây dưới các module tương ứng.
mod inaccessible;
pub mod nested;

pub fn function() {
    println!("called `my::function()`");
}

fn private_function() {
    println!("called `my::private_function()`");
}

pub fn indirect_access() {
    print!("called `my::indirect_access()`, that\n> ");

    private_function();
}
```

Trong tập tin `my/nested.rs`:

```rust,ignore
pub fn function() {
    println!("called `my::nested::function()`");
}

#[allow(dead_code)]
fn private_function() {
    println!("called `my::nested::private_function()`");
}
```

Trong tập tin `my/inaccessible.rs`:

```rust,ignore
#[allow(dead_code)]
pub fn public_function() {
    println!("called `my::inaccessible::public_function()`");
}
```

Kiểm tra xem mọi thứ vẫn hoạt động như trước:

```shell
$ rustc split.rs && ./split
called `my::function()`
called `function()`
called `my::indirect_access()`, that
> called `my::private_function()`
called `my::nested::function()`
```

[visibility]: visibility.md
