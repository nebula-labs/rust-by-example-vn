# Kĩ thuật early returns

Trong các ví dụ trước, bạn đã được giới thiệu về cách xử lý lỗi một cách tường minh nhờ vào việc sử dụng các combinator.
Ngoài cách trên, bạn có thể phân tích và xử lý lỗi theo từng trường hợp bằng cách sử dụng câu lệnh `match` và kĩ thuật _early returns_

Nhờ đó, bạn có thể dừng thực thi hàm đang chạy và trả về lỗi nếu có. Đối với một số người,
kiểu code này có thể dễ hiểu và dễ viết hơn. Dưới đây là một phiên bản khác của ví dụ trước đó được viết lại sử dụng kĩ thuật early returns:

```rust,editable
use std::num::ParseIntError;

fn multiply(first_number_str: &str, second_number_str: &str) -> Result<i32, ParseIntError> {
    let first_number = match first_number_str.parse::<i32>() {
        Ok(first_number)  => first_number,
        Err(e) => return Err(e),
    };

    let second_number = match second_number_str.parse::<i32>() {
        Ok(second_number)  => second_number,
        Err(e) => return Err(e),
    };

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

Tới đây, bạn đã được học về cách xử lý lỗi một cách tường minh bằng cách sử dụng combinators
và kĩ thuật early returns. Mặc dù chúng ta thường sẽ tránh việc sử dụng panic khi gặp lỗi,
nhưng đôi khi phải xử lý tường minh tất cả các lỗi có thể xảy ra dẫn đến việc viết code
rất lằng nhằng và phiền toái.

Ở phần tiếp theo, bạn sẽ được giới thiệu toán tử `?` để áp dụng vào những trường hợp
mà chúng ta chỉ cần `unwrap` là có thể giải quyết được vấn đề mà không dẫn đến `panic`.

[combinators]: https://doc.rust-lang.org/reference/glossary.html#combinator
