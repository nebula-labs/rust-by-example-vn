# `map` for `Result`

Khi xảy ra lỗi trong hàm `multiply` của ví dụ trước, sử dụng panic không tạo ra mã nguồn vững chắc. 
Thông thường, chúng ta muốn trả lại lỗi cho người gọi hàm để người gọi hàm quyết định cách phản hồi với lỗi đó.
Đầu tiên, chúng ta cần phải biết loại lỗi mà chúng ta đang gặp phải. Để xác định kiểu Err,
chúng ta tìm kiểu lỗi trong: [`parse()`][parse], Được thực hiện bằng cách sử dụng
[`FromStr`][from_str] trait for [`i32`][i32]. Kết quả, kiểu `Err` được xác định như sau: [`ParseIntError`][parse_int_error].

Trong ví dụ dưới đây, câu lệnh `match` trực tiếp dẫn đến mã nguồn tổng thể phức tạp hơn.

```rust,editable
use std::num::ParseIntError;

// Với kiểu trả về được viết lại, chúng ta sử dụng phân tích mẫu mà không có `unwrap()`.
fn multiply(first_number_str: &str, second_number_str: &str) -> Result<i32, ParseIntError> {
    match first_number_str.parse::<i32>() {
        Ok(first_number)  => {
            match second_number_str.parse::<i32>() {
                Ok(second_number)  => {
                    Ok(first_number * second_number)
                },
                Err(e) => Err(e),
            }
        },
        Err(e) => Err(e),
    }
}

fn print(result: Result<i32, ParseIntError>) {
    match result {
        Ok(n)  => println!("n is {}", n),
        Err(e) => println!("Error: {}", e),
    }
}

fn main() {
   // Điều này vẫn cho phép trả về một câu trả lời hợp lý.
    let twenty = multiply("10", "2");
    print(twenty);

    // Bây giờ đoạn mã sau đây cung cấp một thông báo lỗi hữu ích hơn nhiều.
    let tt = multiply("t", "2");
    print(tt);
}
```
May mắn thay, các phương thức `Option`'s `map`, `and_then` và nhiều phương thức khác cũng được cài đặt cho Result.
[`Result`][result] chứa một danh sách đầy đủ..

```rust,editable
use std::num::ParseIntError;

// Giống với `Option`, chúng ta có thể sử dụng các phương thức ghép kết quả như `map()`.
// Hàm này tương tự như hàm trên và có chức năng: 
// Nhân nếu cả hai giá trị có thể được phân tích từ chuỗi, nếu không truyền lỗi cho phía gọi hàm xử lý.
fn multiply(first_number_str: &str, second_number_str: &str) -> Result<i32, ParseIntError> {
    first_number_str.parse::<i32>().and_then(|first_number| {
        second_number_str.parse::<i32>().map(|second_number| first_number * second_number)
    })
}

fn print(result: Result<i32, ParseIntError>) {
    match result {
        Ok(n)  => println!("n is {}", n),
        Err(e) => println!("Error: {}", e),
    }
}

fn main() {
    // Điều này vẫn cho phép trả về một câu trả lời hợp lý.
    let twenty = multiply("10", "2");
    print(twenty);

    // Bây giờ đoạn mã sau đây cung cấp một thông báo lỗi hữu ích hơn nhiều.
    let tt = multiply("t", "2");
    print(tt);
}
```

[parse]: https://doc.rust-lang.org/std/primitive.str.html#method.parse
[from_str]: https://doc.rust-lang.org/std/str/trait.FromStr.html
[i32]: https://doc.rust-lang.org/std/primitive.i32.html
[parse_int_error]: https://doc.rust-lang.org/std/num/struct.ParseIntError.html
[result]: https://doc.rust-lang.org/std/result/enum.Result.html
