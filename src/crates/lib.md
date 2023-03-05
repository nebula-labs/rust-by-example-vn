# Tạo một thư viện

Hãy tạo một thư viện, và sau đó xem cách liên kết nó với một crate khác.

```rust,ignore
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

```shell
$ rustc --crate-type=lib rary.rs
$ ls lib*
library.rlib
```

Các thư viện có tiền tố là `lib`, và mặc định chúng có cùng tên với tập tin crate, nhưng cái tên mặc định này có thể được ghi đè bằng cách truyền tùy chọn `--crate-name` đến trình biên dịch `rustc` hoặc bằng cách sử dụng [ thuộc tính `crate_name`][crate-name].

[crate-name]: ../attribute/crate.md
