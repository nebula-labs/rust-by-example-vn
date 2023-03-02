
# Crates

Thuộc tính `crate_type` có thể được sử dụng để thông báo cho trình biên dịch biết một crate là một tệp kiểu nhị phân hay là một thư viện( và thậm chí là một loại thư viện nào), và thuộc tính `crate_name` có thể được sử dụng để đặt tên cho crate. 

Tuy nhiên, điều quan trọng cần lưu ý là cả 2 thuộc tính `crate_type` và `crate_name` đều không có tác dụng gì khi sử dụng Cargo - trình quản lý gói Rust. Kể từ khi Cargo được sử dụng cho phần lớn các project Rust, điều này có nghĩa việc sử dụng `crate_type` và `crate_name` trong thế giới thực là tương đối hạn chế.

```rust,mdbook-runnable,editable
// Crate này là một thư viện
#![crate_type = "lib"]
// Crate này có tên là "rary"
#![crate_name = "rary"]

pub fn public_function() {
    println!("called rary's `public_function()`");
}

fn private_function() {
    println!("called rary's `private_function()`");
}

pub fn indirect_access() {
    print!("called rary's `indirect_access()`, that\n> ");

    private_function();
} 
```
Khi thuộc tính `crate_type` này được sử dụng, chúng ta không còn cần truyền cờ `--crate-type` cho trình biên dịch `rustc` nữa.

```shell
$ rustc lib.rs
$ ls lib*
library.rlib
```