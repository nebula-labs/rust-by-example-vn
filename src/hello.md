# Hello World

Đây là mã nguồn của chương trình Hello World truyền thống được viết bằng ngôn ngữ Rust.

```rust,editable
// Đây là một comment, nó sẽ được bỏ qua bởi trình biên dịch
// Bạn có thể test mã code này bằng việc click vào nút "Run" ở trên -> 
// hoặc nếu bạn thích sử dụng bàn phím, hãy nhấn tổ hợp "Ctrl + Enter" 

// Mã code này là có thể sửa, hãy thử chỉnh sửa nó!
// Sau đó bạn luôn có thể trở lại mã code ban đầu bằng việc click vào nút "Reset" -> 

// Đây là main function của chương trình
fn main() {
    // Các câu lệnh ở đây được thực thi khi compiled binary (mã nhị phân đã biên dịch) được gọi

    // In văn bản ra Console
    println!("Hello World!");
}
```

`println!` là một [*macro*][macros], thứ có thể in văn bản ra
console cho bạn.

Một binary có thể được tạo ra bằng cách sử dụng trình biên dịch của Rust: `rustc`.

```bash
$ rustc hello.rs
```

`rustc` sẽ tạo ra một `hello` binary có thể thực thi.

```bash
$ ./hello
Hello World!
```

### Thực hành

Nhấn 'Run' và xem kết quả. Tiếp theo, hãy thử thêm một dòng với 
`println!` macro thứ hai để có được output như sau:

```text
Hello World!
I'm a Rustacean!
```

[macros]: macros.md