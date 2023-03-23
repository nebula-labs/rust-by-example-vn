# aliases for Result
Trong Rust, chúng ta có thể tạo các alias để tái sử dụng lại các kiểu Result cụ thể nhiều lần. Alias có thể được định nghĩa ở mức độ module và đặc biệt hữu ích để định nghĩa tất cả các Result liên quan với một module cụ thể một cách ngắn gọn. Trong thư viện chuẩn của Rust, ngay cả alias cho io::Result cũng được cung cấp.

Dưới đây là một ví dụ nhanh để thể hiện cú pháp:

use std::num::ParseIntError;

// Định nghĩa một alias generic cho `Result` với kiểu lỗi là `ParseIntError`.
type AliasedResult<T> = Result<T, ParseIntError>;

// Sử dụng alias trên để tham chiếu đến kiểu `Result` cụ thể của chúng ta.
fn multiply(first_number_str: &str, second_number_str: &str) -> AliasedResult<i32> {
    first_number_str.parse::<i32>().and_then(|first_number| {
        second_number_str.parse::<i32>().map(|second_number| first_number * second_number)
    })
}

// Ở đây, alias lại giúp tiết kiệm một số không gian.
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
