# Raw identifiers

Như các ngôn ngữ lập trình khác, Rust cũng có khái niệm "keywords" - từ khóa. Các từ khóa này có ý nghĩa riêng trong Rust, do đó chúng ta không thể sử dụng các từ khóa này để đặt tên cho biến, hàm,...

Raw identifiers cho phép bạn sử dụng các từ khóa này. Raw identifiers trong Rust là một chuỗi ký tự bắt đầu bằng từ khóa "r#". Điều này đặc biệt hữu ích khi Rust giới thiệu các từ khóa mới, và một thư viện sử dụng phiên bản Rust cũ hơn có một biến hoặc hàm có cùng tên với từ khóa được giới thiệu trong phiên bản Rust mới hơn.

Ví dụ, một crate được biên dịch với phiên bản 2015 và export một hàm được đặt tên là `try`. Trong phiên bản 2018 của Rust giới thiệu `try` là một từ khóa mới, thì chúng ta sẽ không thể sử dụng `try` để đặt tên cho hàm nếu không có raw identifiers.

```rust,ignore
extern crate foo;

fn main() {
    foo::try();
}
```

Đây là lỗi sẽ xảy ra:

```text
error: expected identifier, found keyword `try`
 --> src/main.rs:4:4
  |
4 | foo::try();
  |      ^^^ expected identifier, found keyword
```

Chúng ta sẽ phải sử dụng raw identifier:

```rust,ignore
extern crate foo;

fn main() {
    foo::r#try();
}
```