# Toán tử `?`

Thỉnh thoảng, chúng ta chỉ muốn đơn giản là sử dụng hàm `unwrap` mà bỏ qua khả năng `panic`. Hiện tại, `unwrap` buộc chúng ta phải viết các mã lồng vào nhau sâu hơn
để lấy giá trị ra trong khi thứ mà ta muốn chỉ là lấy ra giá trị trả về.
Đây là lý do mà toán tử `?` được tạo ra.

Khi gặp phải một đối tượng `Err`, có hai cách xử lý:

1. `panic!`, chúng ta nên tránh làm thế này nếu có thể.
2. `return` vì `Err` nghĩa là chương trình không thể xử lý được tiếp tục nữa.

Toán tử `?` _gần tương đương nhất với_[^†] hàm `unwrap` vì hàm `unwrap` sẽ `return` các `Err`s
thay vì `panic` cả chương trình.
Hãy xem cách ta có thể đơn giản hoá ví dụ xử lý lỗi sử dụng combinator ở phần trước:

```rust,editable
use std::num::ParseIntError;

fn multiply(first_number_str: &str, second_number_str: &str) -> Result<i32, ParseIntError> {
    let first_number = first_number_str.parse::<i32>()?;
    let second_number = second_number_str.parse::<i32>()?;

    Ok(first_number * second_number)
}

fn print(result: Result<i32, ParseIntError>) {
    match result {
        Ok(n)  => println!("n is {}", n),
        Err(e) => println!("Error: {}", e),
    }
}

fn main() {
    print(multiply("10", "2"));
    print(multiply("t", "2"));
}
```

## Macro `try!`

Trước khi toán tử `?` được giới thiệu ở phiên bản sau này, ta dùng macro `try!` để làm được chức năng tương tự như `?`.
Hiện nay, người ta khuyến khích sử dụng toán tử `?` nhưng bạn có thể đôi khi tìm thấy macro `try!`
được sử dụng khi đọc những code cũ của các phiên bản trước.

Hàm `multiply` được ví dụ ở các ví dụ trước sẽ nhìn giống như thế này nếu dùng macro `try!`:

```rust,editable,edition2015
// Để có thể biên dịch và chạy ví dụ này thành công với Cargo, hãy thay đổi giá trị của
// trường `edition` trong phần `[package]` ở file `Cargo.toml` thành "2015"

use std::num::ParseIntError;

fn multiply(first_number_str: &str, second_number_str: &str) -> Result<i32, ParseIntError> {
    let first_number = try!(first_number_str.parse::<i32>());
    let second_number = try!(second_number_str.parse::<i32>());

    Ok(first_number * second_number)
}

fn print(result: Result<i32, ParseIntError>) {
    match result {
        Ok(n)  => println!("n is {}", n),
        Err(e) => println!("Error: {}", e),
    }
}

fn main() {
    print(multiply("10", "2"));
    print(multiply("t", "2"));
}
```

[^†]: Đọc [re-enter ?][re_enter_?] để biết thêm.

[re_enter_?]: ../multiple_error_types/reenter_question_mark.md
