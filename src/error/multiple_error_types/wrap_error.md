# Wrapping errors

Một phương pháp khác để xử lý các lỗi là bọc chúng trong một kiểu dữ liệu lỗi mà bạn tự định nghĩa.

```rust,editable
use std::error;
use std::error::Error;
use std::num::ParseIntError;
use std::fmt;

type Result<T> = std::result::Result<T, DoubleError>;

#[derive(Debug)]
enum DoubleError {
    EmptyVec,
    // Chúng ta sẽ trì hoãn việc triển khai phân tích lỗi cú pháp đối với lỗi của chúng.
    // Việc cung cấp thông tin bổ sung yêu cầu thêm dữ liệu vào loại lỗi.
    Parse(ParseIntError),
}

impl fmt::Display for DoubleError {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        match *self {
            DoubleError::EmptyVec =>
                write!(f, "please use a vector with at least one element"),
            // Lỗi được bọc(wrap) chứa thông tin bổ sung và có sẵn thông qua phương thức source(được triển khai phía dưới).
            DoubleError::Parse(..) =>
                write!(f, "the provided string could not be parsed as int"),
        }
    }
}

impl error::Error for DoubleError {
    fn source(&self) -> Option<&(dyn error::Error + 'static)> {
        match *self {
            DoubleError::EmptyVec => None,
            // Nguyên nhân là việc thực hiện triển khai loại lỗi cơ bản. Được chuyển hoàn toàn sang trait object `&error::Error`. Điều này hoạt động được vì loại bên dưới đã triển khai trong `Error` trait.
            DoubleError::Parse(ref e) => Some(e),
        }
    }
}

// Thực hiện chuyển đổi từ `ParseIntError` thành `DoubleError`.
// Điều này sẽ được gọi tự động bởi `?` nếu một `ParseIntError` cần được chuyển đổi thành một `DoubleError`.
impl From<ParseIntError> for DoubleError {
    fn from(err: ParseIntError) -> DoubleError {
        DoubleError::Parse(err)
    }
}

fn double_first(vec: Vec<&str>) -> Result<i32> {
    let first = vec.first().ok_or(DoubleError::EmptyVec)?;
    // Ở đây chúng ta ngầm sử dụng triển khai `ParseIntError` của `From` (mà chúng ta đã định nghĩa ở trên) để tạo ra một `DoubleError`.
    let parsed = first.parse::<i32>()?;

    Ok(2 * parsed)
}

fn print(result: Result<i32>) {
    match result {
        Ok(n)  => println!("The first doubled is {}", n),
        Err(e) => {
            println!("Error: {}", e);
            if let Some(source) = e.source() {
                println!("  Caused by: {}", source);
            }
        },
    }
}

fn main() {
    let numbers = vec!["42", "93", "18"];
    let empty = vec![];
    let strings = vec!["100.4", "93", "18"];

    print(double_first(numbers));
    print(double_first(empty));
    print(double_first(strings));
}
```

Điều này bổ sung thêm một chút mẫu để xử lý lỗi và có thể không cần thiết trong tất cả các ứng dụng. Có một số thư viện có thể đảm nhiệm việc xử lý mẫu lỗi cho bạn.
### See also:

[`From::from`][from] and [`Enums`][enums]

[from]: https://doc.rust-lang.org/std/convert/trait.From.html
[enums]: ../../custom_types/enum.md
