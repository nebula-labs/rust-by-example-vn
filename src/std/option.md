# Option

Đôi lúc, chúng ta muốn xử lý lỗi của một phần trong chương trình thay vì gọi `panic!`; điều này có thể được thực hiện bằng cách sử dụng `Option`.

`Option<T>` có thể mang hai biến thể:

* `None`, biểu hiện cho việc thất bại khi thực thi hoặc không có giá trị và
* `Some(value)`, một kiểu tuple bọc ngoài giá trị `value` với kiểu dữ liệu là `T`.

```rust,editable,ignore,mdbook-runnable
// Một phép chia số nguyên không gây `panic!`
fn checked_division(dividend: i32, divisor: i32) -> Option<i32> {
    if divisor == 0 {
        // Việc thất bại khi thực thi được biểu diễn bằng biến thể `None`
        None
    } else {
        // Kết quả được bọc bên trong biến thể `Some`    
        Some(dividend / divisor)
    }
}

// Hàm này có thể không thực hiện thành công phép chia
fn try_division(dividend: i32, divisor: i32) {
    // Giá trị của `Option` có thể được phân tích cú pháp theo match, tương tự như kiểu enum
    match checked_division(dividend, divisor) {
        None => println!("{} / {} failed!", dividend, divisor),
        Some(quotient) => {
            println!("{} / {} = {}", dividend, divisor, quotient)
        },
    }
}

fn main() {
    try_division(4, 2);
    try_division(1, 0);

    // Gán `None` cho một biến cần xác định kiểu
    let none: Option<i32> = None;
    let _equivalent_none = None::<i32>;

    let optional_float = Some(0f32);

    // Unwrap biến thể `Some` sẽ trích xuất ra giá trị được bọc bên trong
    println!("{:?} unwraps to {:?}", optional_float, optional_float.unwrap());

    // Unwrap biến thể `None` sẽ gây ra `panic!`
    println!("{:?} unwraps to {:?}", none, none.unwrap());
}
```