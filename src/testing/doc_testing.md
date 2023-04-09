# Kiểm thử tài liệu (document testing)

Cách cơ bản nhất để tài liệu hoá một dự án Rust là thông qua việc chú thích ngay trên mã nguồn.
Các chú thích tài liệu được viết theo chuẩn [CommonMark Markdown specification][commonmark] và có hỗ trợ các khối code
bên trong những chú thích đó. Rust sẽ quan tâm đến tính đúng đắn, do đó những khối code này sẽ được biên dịch và sử dụng như
các bài kiểm thử tài liệu (documentation tests)

````rust,ignore
/// Dòng đầu tiên là tóm tắt ngắn mô tả hàm.
///
/// Những dòng tiếp theo trình bày tài liệu chi tiết. Các khối code thường bắt đầu với
/// ba dấu nháy ngược và sẽ tồn tài mặc định một cách không tường minh hàm `fn main()` bên trong
/// và `extern crate <cratename>`. Giả sử là chúng ta đang kiểm thử crate `doccomments`:
///
/// ```
/// let result = doccomments::add(2, 3);
/// assert_eq!(result, 5);
/// ```
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

/// Thường các chú thích tài liệu sẽ bao gồm các phần "Examples", "Panics" và "Failures"
///
/// Hàm tiếp theo sẽ thực hiện phép chia hai số
///
/// # Examples
///
/// ```
/// let result = doccomments::div(10, 2);
/// assert_eq!(result, 5);
/// ```
///
/// # Panics
///
/// Hàm sẽ panic nếu đối số thứ hai là 0
///
/// ```rust,should_panic
/// // panics on division by zero
/// doccomments::div(10, 0);
/// ```
pub fn div(a: i32, b: i32) -> i32 {
    if b == 0 {
        panic!("Divide-by-zero error");
    }

    a / b
}
````

Các khối code bên trong docs sẽ được kiểm thử một cách tự động
khi ta chạy lệnh `cargo test` thông thường:

```shell
$ cargo test
running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

   Doc-tests doccomments

running 3 tests
test src/lib.rs - add (line 7) ... ok
test src/lib.rs - div (line 21) ... ok
test src/lib.rs - div (line 31) ... ok

test result: ok. 3 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

## Động lực đằng sau các bài documentation tests

Mục đích chủ yếu của documentation tests là phục vụ như là các ví dụ để thực hiện
chức năng, đó là một trong những nguyên tắc quan trọng nhất
[guidelines][question-instead-of-unwrap]. Điều này cho phép sử dụng các ví dụ đưa ra từ trong docs
như là một đoạn code hoàn chỉnh. Tuy nhiên việc sử dụng `?` khiến cho việc biên dịch bị lỗi vì `main`
trả về `unit`. Khả năng ẩn đi một số dòng mã nguồn trong tài liệu
sẽ là cứu tinh trong trường hợp này: ta có thể viết `fn try_main() -> Result<(), ErrorType>`, ẩn nó đi
và `unwrap` nó bên trong hàm `main` đã bị ẩn. Nghe có vẻ phức tạp nhỉ? Sau đây là một ví dụ:

````rust,ignore
/// Sử dụng hàm `try_main` đã được ẩn trong các bài doc tests
///
/// ```
/// # // các dòng bị ẩn sẽ bắt đầu với kí hiệu `#`, tuy nhiên chúng vẫn được biên dịch!
/// # fn try_main() -> Result<(), String> { // dòng này sẽ bao lại quanh phần thân sẽ được hiển thị bên trong docs
/// let res = doccomments::try_div(10, 2)?;
/// # Ok(()) // trả về từ hàm try_main
/// # }
/// # fn main() { // Bắt đầu một hàm main mà nó sẽ thực thi unwrap()
/// #    try_main().unwrap(); // gọi hàm try_main và thực thi việc unwrap
/// #                         // nhờ đó test sẽ panic khi gặp lỗi
/// # }
/// ```
pub fn try_div(a: i32, b: i32) -> Result<i32, String> {
    if b == 0 {
        Err(String::from("Divide-by-zero"))
    } else {
        Ok(a / b)
    }
}
````

## Đọc thêm

- [RFC505][rfc505] về các phong cách ghi tài liệu
- [API Guidelines][doc-nursery] về các hướng dẫn ghi chú tài liệu

[doc-nursery]: https://rust-lang-nursery.github.io/api-guidelines/documentation.html
[commonmark]: https://commonmark.org/
[rfc505]: https://github.com/rust-lang/rfcs/blob/master/text/0505-api-comment-conventions.md
[question-instead-of-unwrap]: https://rust-lang-nursery.github.io/api-guidelines/documentation.html#examples-use--not-try-not-unwrap-c-question-mark
