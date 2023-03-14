# Các công dụng khác của `?`
Trong ví dụ trước hãy lưu ý rằng phản ứng tức thời của chúng tôi đối với việc gọi `parse` là để `map` lỗi từ một lỗi thư viện vào môt lỗi được đóng gói: 
```rust
.and_then(|s| s.parse::<i32>()
    .map_err(|e| e.into())
```
Vì đây là một thao tác đơn giản và phổ biến, nên sẽ rất thuận tiện nếu nó có thể được bỏ qua. Than ôi, bởi vì `and_then` không đủ linh hoạt, nên không thể. Tuy nhiên, thay vào đó chúng ta có thể sử dụng `?`.

Trước đó `?` đã được giải thích là `unwrap` hoặc `return Err(err)`. Điều này chỉ đúng phần nào, thực tế nó có nghĩa là `unwrap` hoặc `return Err(From::from(err)`. Do `From::from` là một tiện ích để chuyển đổi giữa các kiểu khác nhau, điều này có nghĩa là nếu bạn sử dụng `?` lỗi có thể tự động chuyển đổi thành kiểu trả về .

Dưới đây, chúng tôi viết lại ví dụ trước bằng cách sử dụng `?`. Kết quả là, khi `From::from` được thực thi nó làm biến mất lỗi `map_err`. 

```rust
use std::error;
use std::fmt;

// Thay đổi bí danh thành `Box<dyn error::Error>`.
type Result<T> = std::result::Result<T, Box<dyn error::Error>>;

#[derive(Debug)]
struct EmptyVec;

impl fmt::Display for EmptyVec {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "invalid first item to double")
    }
}

impl error::Error for EmptyVec {}

// Cấu trúc giống như trước đây nhưng thay vì xâu chuỗi tất cả `Results`
// và `Option` lại với nhau, chúng ta sử dụng `?` để lấy giá trị bên trong ngay lập tức.

fn double_first(vec: Vec<&str>) -> Result<i32> {
    let first = vec.first().ok_or(EmptyVec)?;
    let parsed = first.parse::<i32>()?;
    Ok(2 * parsed)
}

fn print(result: Result<i32>) {
    match result {
        Ok(n)  => println!("The first doubled is {}", n),
        Err(e) => println!("Error: {}", e),
    }
}

fn main() {
    let numbers = vec!["42", "93", "18"];
    let empty = vec![];
    let strings = vec!["tofu", "93", "18"];

    print(double_first(numbers));
    print(double_first(empty));
    print(double_first(strings));
}
```
Cái này thực sự khá trong sáng. So với `panic` ban đầu, nó rất giống với việc thay thế các lời gọi `unwrap` bằng `?` ngoại trừ các kiểu trả về là `Result`. Kết quả là chúng được phá hủy ở cấp cao nhất.

### Xem thêm
[From::from](https://doc.rust-lang.org/std/convert/trait.From.html) và [?](https://doc.rust-lang.org/reference/expressions/operator-expr.html#the-question-mark-operator)