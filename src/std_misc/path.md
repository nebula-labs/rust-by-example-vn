# Path
Cấu trúc `Path` đại diện cho đường dẫn tới tệp tin trên hệ thống tệp (filesystem). Có hai loại `Path`: `posix::Path` dành cho các hệ thống UNIX, và `windows::Path` dành cho hệ thống Windows. Prelude sẽ tự động thiết lập các phiên bản `Path` phù hợp với nền tảng cụ thể. <br/>
> Trong Rust, "Prelude" là một tập hợp các thư viện được tự động đưa vào phạm vi của một chương trình Rust mà không cần khai báo thêm. Tập hợp này bao gồm các thư viện cốt lõi của Rust cũng như một số thư viện phổ biến khác.

Một đối tượng `Path` có thể được tạo ra từ một `OsStr`, và cung cấp nhiều phương thức để lấy thông tin từ tệp tin/thư mục mà đường dẫn trỏ tới. <br/>
Path là một đối tượng không thể thay đổi (`immutable`). Phiên bản `Path` được sở hữu bởi (`owned`) là `PathBuf`. Mối quan hệ giữa `Path` và `PathBuf` tương tự như giữa `str` và `String`: một `PathBuf` có thể được thay đổi trực tiếp (in-place), và có thể được giải tham chiếu (dereferenced) thành một `Path`. <br/>
Lưu ý rằng một `Path` không thể được biểu diễn bên dưới dạng một chuỗi UTF-8, mà sẽ được lưu trữ dưới dạng một `OsString`. Do đó, không nên chuyển đổi một `Path` thành một `&str` vì tiến trình này rất dễ xảy ra lỗi (trả về một `Option`). Tuy nhiên, một `Path` có thể được tự do chuyển đổi thành một `OsString` hoặc `&OsStr` bằng cách sử dụng `into_os_string` và `as_os_str`."

```rust, editable
use std::path::Path;

fn main() {
    // Tạo một `Path` từ một `&'static str`
    let path = Path::new(".");

    // Phương thức `display` sẽ trả về một cấu trúc `Display`able
    let _display = path.display();

    // `join` sẽ hợp nhất các phần của đường dẫn lại và thiết lập các dấu phân cách tùy thuộc vào hệ điều hành
    // sau đó sẽ trả về một `PathBuf`
    let mut new_path = path.join("a").join("b");

    // `push` sẽ mở rộng `PathBuf` với một `&Path`
    new_path.push("c");
    new_path.push("myfile.tar.gz");

    // `set_file_name` sẽ cập nhật tên của tệp tại `PathBuf`
    new_path.set_file_name("package.tgz");

    // Chuyển `PathBuf` sang một string slice (&str)
    match new_path.to_str() {
        None => panic!("new path is not a valid UTF-8 sequence"),
        Some(s) => println!("new path is {}", s),
    }
}
```
Tùy vào trường hợp cụ thể, hãy chắc chắn rằng đã kiểm tra các phương thức khác của Path (`posix::Path` hoặc `windows::Path` - thuộc vào hệ điều hành) và cấu trúc Metadata - một đối tượng cung cấp thông tin khác ngoài nội dung của file hoặc thư mục của một file hoặc thư mục. <br/>

## Xem thêm:

[OsStr](https://doc.rust-lang.org/std/ffi/struct.OsStr.html) và [Metadata](https://doc.rust-lang.org/std/fs/struct.Metadata.html).