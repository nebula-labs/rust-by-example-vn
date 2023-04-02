# aliases for `Result`
Khi chúng ta muốn tái sử dụng một kiểu `Result` cụ thể nhiều lần thì làm như thế nào?
Hãy nhớ rằng Rust cho phép chúng ta tạo [aliases][typealias].
Một cách tiện lợi, chúng ta có thể định nghĩa một bí danh cho kiểu Result cụ thể đó.

Ở mức độ module, việc tạo bí danh có thể rất hữu ích. Các lỗi được tìm thấy trong một module cụ thể thường có cùng kiểu `Err`,
do đó một bí danh duy nhất có thể ngắn gọn định nghĩa tất cả các `Result` liên quan. 
Việc này rất hữu ích đến mức thư viện `std` cung cấp sẵn một bí danh cho việc này: [`io::Result`][io_result]!

Đây là một ví dụ nhanh để minh họa cú pháp:

```rust,editable
use std::num::ParseIntError;

// Định nghĩa một bí danh chung cho `Result` với kiểu lỗi `ParseIntError`.
type AliasedResult<T> = Result<T, ParseIntError>;

// Sử dụng bí danh trên để tham chiếu đến kiểu Result cụ thể của chúng ta.
fn multiply(first_number_str: &str, second_number_str: &str) -> AliasedResult<i32> {
    first_number_str.parse::<i32>().and_then(|first_number| {
        second_number_str.parse::<i32>().map(|second_number| first_number * second_number)
    })
}

// Ở đây, bí danh lại cho phép chúng ta tiết kiệm không gian.
fn print(result: AliasedResult<i32>) {
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

### See also:

[`io::Result`][io_result]

[typealias]: ../../types/alias.md
[io_result]: https://doc.rust-lang.org/std/io/type.Result.html
