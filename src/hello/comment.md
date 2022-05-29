# Comments

Bất kỳ chương trình nào cũng yêu cầu có comments (nhận xét, giải thích) và Rust hỗ trợ
một số loại comments khác nhau:

* *Regular comments*, sẽ được bỏ qua bởi trình biên dịch:
   * `// Line comments là loại comment kéo dài đến cuối dòng.`
   * `/* Block comments là loại comment của một khối lệnh.*/`
* *Doc comments*, sẽ được phân tích cú pháp thành HTML library
  [documentation][docs]:
   * `/// Tạo library docs cho mục bên dưới nó.`
   * `//! Tạo library docs cho mục bao quanh nó.`

```rust,editable
fn main() {
    // Đây là một ví dụ cho line comment
    // Có hai dấu gạch chéo / ở đầu dòng
    // Và tất cả những gì viết phía sau chúng trong dòng sẽ không được trình biên dịch đọc

    // println!("Hello, world!");

    // Câu lệnh trên sẽ không được thực hiện vì nó đã được tính là một line comment. Nếu muốn chạy nó, thử xóa hai dấu gạch chéo và chạy lại.

    /* 
     * Đây là một loại comment khác, block comment. Nói chung,
     * line comment là kiểu comment được khuyên dùng. Tuy nhiên
     * block comment cũng cực kỳ hữu ích khi muốn tạm thời vô hiệu hóa 
     * một đoạn mã dài giúp bạn không mất công gõ nhiều dấu // ở đầu dòng. 
     * /* Block comment có thể /* lồng vào nhau, */ như ví dụ này */
     * /*/*/* Bạn hãy tự thử xem! */*/*/
     */

    // Bạn có thể thao tác với các biểu thức dễ dàng hơn bằng block comment
    // so với line comment. Thử xóa các dấu của block comment dưới đây
    // để thay đổi kết quả:
    let x = 5 + /* 90 + */ 5;
    println!("Is `x` 10 or 100? x = {}", x);
}

```

### Xem thêm tại đây:

[Library documentation][docs]

[docs]: ../meta/doc.md
