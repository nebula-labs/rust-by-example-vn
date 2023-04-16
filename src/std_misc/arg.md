# Program arguments

## Thư viện tiêu chuẩn

Các đối số dòng lệnh có thể truy cập bằng cách sử dụng `std::env::args`,
nó trả về một bộ lặp (iterator) mà mỗi lần lặp lại nó sẽ cung cấp một `String` cho mỗi đối số:

```rust,editable
use std::env;

fn main() {
    let args: Vec<String> = env::args().collect();

    // Đối số đầu tiên là đường dẫn được sử dụng để gọi chương trình.
    println!("Đường dẫn của tôi là {}.", args[0]);

    // Các đối số còn lại là các tham số được truyền từ dòng lệnh.
    // Gọi chương trình như sau:
    //   $ ./args arg1 arg2
    println!("Tôi nhận được {:?} đối số: {:?}.", args.len() - 1, &args[1..]);
}
```

```shell
$ ./args 1 2 3
Đường dẫn của tôi là ./args.
Tôi nhận được 3 đối số: ["1", "2", "3"].
```

## Crates

Một cách khác là sử dụng các crate để cung cấp các chức năng phụ trợ hơn khi tạo ứng dụng dòng lệnh.
[Rust Cookbook] cho thấy những cách làm tốt nhất về cách sử dụng một trong những crate
đối số dòng lệnh phổ biến, `clap`.

[Rust Cookbook]: https://rust-lang-nursery.github.io/rust-cookbook/cli/arguments.html