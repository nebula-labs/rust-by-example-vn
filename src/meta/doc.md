# Documentation

Sử dụng `cargo doc` để build tài iệu trong thư mục `target/doc`.

Sử dụng `cargo test` để chạy tất cả các phép kiểm thử (bao gồm cả các kiểm thử dành cho tài liệu), và `cargo test --doc` để chạy chỉ các kiểm thử của tài liệu.

Các câu lệnh này sẽ gọi `rustdoc` (và `rustc`) theo yêu cầu một cách phù hợp.

## Các ghi chú tài liệu

Các ghi chú tài liệu rất hữu ích cho những dự án lớn yêu cầu tài liệu mô tả. Khi chạy lệnh `rustdoc`, tài liệu của dự án sẽ được xây dựng dựa vào các ghi chú này. Ghi chú được bắt đầu bằng một dấu `///`, và hỗ trợ định dạng [Markdown].

````rust,editable,ignore
#![crate_name = "doc"]

/// Cấu trúc đại diện cho một con người
pub struct Person {
    /// Một người cần phải có tên, mặc cho Juliet có ghét nó
    name: String,
}

impl Person {
    /// Trả về một người cùng với tên của họ
    ///
    /// # Các đối số
    ///
    /// * `name` - Một chuỗi ký tự chứa tên của người
    ///
    /// # Ví dụ
    ///
    /// ```
    /// // Bạn có thể đặt mã rust giữa các dấu rào (```) trong các ghi chú
    /// // Nếu bạn truyền tham số --test đến `rustdoc`, nó sẽ kiểm tra chính tài liệu cho bạn!
    /// use doc::Person;
    /// let person = Person::new("name");
    /// ```
    pub fn new(name: &str) -> Person {
        Person {
            name: name.to_string(),
        }
    }

    /// Gửi một lời chào thân thiện nào!
    ///
    /// Nói "Hello, [name](Person::name)" với `Person` mà gọi nó.
    pub fn hello(& self) {
        println!("Hello, {}!", self.name);
    }
}

fn main() {
    let john = Person::new("John");

    john.hello();
}
````

Để chạy các kiểm thử, phải build mã thành một thư viện trước, sau đó hãy chỉ cho `rustdoct` biết nơi để tìm thư viện liên kết nó với mỗi một chường trình doctest:

```shell
$ rustc doc.rs --crate-type lib
$ rustdoc --test --extern doc="libdoc.rlib" doc.rs
```

## Các thuộc tính tài liệu

Dưới đây là vài ví dụ về các thuộc tính `#[doc]` thường được sử dụng phổ biến nhất với `rustdoc`.

### `inline`

Được dùng cho các tài liệu nội tuyến, thay vì liên kết ra ngoài một trang riêng biệt.

```rust,ignore
#[doc(inline)]
pub use bar::Bar;

/// tài liệu cho mod bar
mod bar {
    /// tài liệu cho cấu trúc Bar
    pub struct Bar;
}
```

### `no_inline`

Dùng chặn các liên kết ra trang riêng hoặc nơi khác.

```rust,ignore
// Ví dụ từ libcore/prelude
#[doc(no_inline)]
pub use crate::mem::drop;
```

### `hidden`

Dùng thuộc tính này để chỉ cho `rustdoc` bỏ qua phần này trong tài liệu:

```rust,editable,ignore
// Ví dụ từ thư viện futures-rs
#[doc(hidden)]
pub use self::async_await::*;
```

Đối với tài liệu, `rustdoc` được cộng đồng sử dụng một cách rộng rãi. Nó được dùng để tạo ra [tài liệu của thư viện std](https://doc.rust-lang.org/std/).

### Xem thêm:

- [The Rust Book: Making Useful Documentation Comments][book]
- [The rustdoc Book][rustdoc-book]
- [The Reference: Doc comments][ref-comments]
- [RFC 1574: API Documentation Conventions][api-conv]
- [RFC 1946: Relative links to other items from doc comments (intra-rustdoc links)][intra-links]
- [Is there any documentation style guide for comments? (reddit)][reddit]

[markdown]: https://en.wikipedia.org/wiki/Markdown
[book]: https://doc.rust-lang.org/book/ch14-02-publishing-to-crates-io.html#making-useful-documentation-comments
[ref-comments]: https://doc.rust-lang.org/stable/reference/comments.html#doc-comments
[rustdoc-book]: https://doc.rust-lang.org/rustdoc/index.html
[api-conv]: https://rust-lang.github.io/rfcs/1574-more-api-documentation-conventions.html#appendix-a-full-conventions-text
[intra-links]: https://rust-lang.github.io/rfcs/1946-intra-rustdoc-links.html
[reddit]: https://www.reddit.com/r/rust/comments/ahb50s/is_there_any_documentation_style_guide_for/
