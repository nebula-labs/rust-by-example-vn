# Playground

[Rust Playground](https://play.rust-lang.org/) là cách để trải nghiệm viết mã Rust thông qua giao diện trình duyệt web.

## Sử dụng với `mdbook`

Với [`mdbook`][mdbook], bạn có thể viết các đoạn mã ví dụ mà có thể chạy và chỉnh sửa được.

```rust,editable
fn main() {
    println!("Hello World!");
}
```

Điều này cho phép người đọc không những có thể chạy được đoạn mã mẫu của bạn, mà còn có thể sửa và tinh chỉnh nó. Chìa khóa ở đây là thêm từ `editable` vào khối rào mã của bạn phân cách bởi dấu phẩy.

````markdown
```rust,editable
//...đặt mã của bạn ở đây
```
````

Ngoài ra, bạn có thể thêm từ `ignore` nếu bạn muốn `mdbook` bỏ qua mã của bạn khi nó thực hiện build hoặc kiểm thử.

````markdown
```rust,editable,ignore
//...place your code here
```
````

## Sử dụng với tài liệu

Bạn có thể nhận ra rằng trong một số [tài liệu chính thức của Rust][official-rust-docs] xuất hiện một nút "chạy" ("Run"), có tác dụng mở mã mẫu trong một cửa sổ mới trên Rust Playground. Tính năng này được bật nếu bạn sử dụng thuộc tính `#[doc]` gọi là [`html_playground_url`][html-playground-url].

### Xem thêm:

- [The Rust Playground][rust-playground]
- [rust-playground][rust-playground]
- [The rustdoc Book][rustdoc-book]

[rust-playground]: https://play.rust-lang.org/
[rust-playground]: https://github.com/integer32llc/rust-playground/
[mdbook]: https://github.com/rust-lang/mdBook
[official-rust-docs]: https://doc.rust-lang.org/core/
[rustdoc-book]: https://doc.rust-lang.org/rustdoc/what-is-rustdoc.html
[html-playground-url]: https://doc.rust-lang.org/rustdoc/the-doc-attribute.html#html_playground_url
