# To và from Strings

## Chuyển đổi sang String

Chuyển đổi bất kỳ kiểu nào sang `String` cũng đơn giản như việc triển khai trait [`ToString`]
cho kiểu đó. Thay vì làm như vậy trực tiếp, bạn nên triển khai trait
[`fmt::Display`][Display], thứ mà sẽ tự động cung cấp [`ToString`] và cũng cho phép in kiểu như đã đề cập trong phần [`print!`][print].

```rust,editable
use std::fmt;

struct Circle {
    radius: i32
}

impl fmt::Display for Circle {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "Circle of radius {}", self.radius)
    }
}

fn main() {
    let circle = Circle { radius: 6 };
    println!("{}", circle.to_string());
}
```

## Phân tích cú pháp (Parsing) một String

Một trong những kiểu phổ biến để chuyển đổi từ string là thành kiểu số. Cách tiếp cận cho việc chuyển đổi này là sử dụng hàm [`parse`] rồi thông qua suy luận kiểu của Rust hoặc chỉ định kiểu cần phân tích cú pháp bằng cú pháp 'turbofish'. Cả hai lựa chọn này được đề cập trong ví dụ bên dưới.

Điều này sẽ chuyển đổi string thành kiểu được chỉ định miễn là trait [`FromStr`] được triển khai cho kiểu đó. Nó được thực hiện cho nhiều kiểu trong thư viện tiêu chuẩn. Để có được chức năng này trên một kiểu do người dùng tự định nghĩa, chỉ cần triển khai trait [`FromStr`] cho kiểu đó.

```rust,editable
fn main() {
    let parsed: i32 = "5".parse().unwrap();
    let turbo_parsed = "10".parse::<i32>().unwrap();

    let sum = parsed + turbo_parsed;
    println!("Sum: {:?}", sum);
}
```

[`ToString`]: https://doc.rust-lang.org/std/string/trait.ToString.html
[Display]: https://doc.rust-lang.org/std/fmt/trait.Display.html
[print]: ../hello/print.md
[`parse`]: https://doc.rust-lang.org/std/primitive.str.html#method.parse
[`FromStr`]: https://doc.rust-lang.org/std/str/trait.FromStr.html
