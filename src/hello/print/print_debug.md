# Debug

Tất cả các kiểu dữ liệu muốn sử dụng các `trait` của định dạng `std::fmt` đều yêu cầu phải có
một implementation để có thể in được dữ liệu. Việc tự động triển khai chỉ được cung cấp
cho các kiểu có sẵn trong thư viện `std`. Tất cả những kiểu khác đều *phải* triển khai 
thủ công bằng cách nào đó.

 `Trait` `fmt::Debug` làm cho việc này trở nên rất đơn giản. *Tất cả* các kiểu đều có thể
`derive` (dẫn xuất) tự động tạo một `fmt::Debug` implementation. Điều này không áp dụng cho `fmt::Display`,
thứ mà bắt buộc phải được triển khai một cách thủ công.

```rust
// Kiểu struct này không thể in được dữ liệu với `fmt::Display` 
// hoặc `fmt::Debug`.
struct UnPrintable(i32);

// Thuộc tính `derive` sẽ tự động tạo một implementation được yêu cầu
// để `struct` này có thể in được dữ liệu với `fmt::Debug`.
#[derive(Debug)]
struct DebugPrintable(i32);
```

Tất cả các kiểu của thư viện `std` cũng tự động có khả năng in dữ liệu với `{:?}`:

```rust,editable
// Derive một `fmt::Debug` implementation cho kiểu `Structure`. `Structure`
// là một cấu trúc chỉ chứa một số nguyên `i32`.
#[derive(Debug)]
struct Structure(i32);

// Đặt `Structure` vào trong cấu trúc `Deep`. Ở đây cũng tạo cho nó khả năng
// in dữ liệu.
#[derive(Debug)]
struct Deep(Structure);

fn main() {
    // In dữ liệu với `{:?}` là tương tự như với `{}`.
    println!("{:?} months in a year.", 12);
    println!("{1:?} {0:?} is the {actor:?} name.",
             "Slater",
             "Christian",
             actor="actor's");

    // `Structure` có thể in dữ liệu!
    println!("Now {:?} will print!", Structure(3));

    // Vấn đề với `derive` đó là không thể kiểm soát cách kết quả được
    // in ra thế nào. Sẽ thế nào nếu tôi chỉ muốn hiển thị ra một số `7`?
    println!("Now {:?} will print!", Deep(Structure(7)));
}
```

`fmt::Debug` làm cho mọi thứ có thể in được nhưng phải hy sinh tính linh hoạt trong việc 
tùy chỉnh hiển thị kết quả. Rust có cung cấp tính năng "pretty printing" với `{:#?}` 
để kết quả in ra được thân thiện hơn.

```rust,editable
#[derive(Debug)]
struct Person<'a> {
    name: &'a str,
    age: u8
}

fn main() {
    let name = "Peter";
    let age = 27;
    let peter = Person { name, age };

    // Pretty print
    println!("{:#?}", peter);
}
```

Để kiểm soát việc hiển thị kết quả, cần triển khai thủ công `fmt::Display`.

### Xem thêm tại đây:

[`attributes`][attributes], [`derive`][derive], [`std::fmt`][fmt],
và [`struct`][structs]

[attributes]: https://doc.rust-lang.org/reference/attributes.html
[derive]: ../../trait/derive.md
[fmt]: https://doc.rust-lang.org/std/fmt/
[structs]: ../../custom_types/structs.md

