# Định nghĩa một kiểu dữ liệu lỗi

Đôi khi ta có thể đơn giản hóa đoạn mã bằng cách che giấu (masking) tất cả các kiểu dữ liệu lỗi khác nhau với một loại lỗi duy nhất. Ta có thể thể hiện ra bằng một lỗi đã được tùy chỉnh.

Rust cho phép chúng ta tự định nghĩa kiểu dữ liệu lỗi của riêng mình. Nhìn chung, một kiểu dữ liệu "tốt" phải:
* Đại diện cho nhiều lỗi với cùng một kiểu dữ liệu
* Thể hiện thông điệp lỗi cho người dùng một cách rõ ràng
* Dễ dàng so sánh với các kiểu dữ liệu lỗi khác
    - Nên: `Err(EmptyVec)`
    - Không nên: `Err("Hãy sử dụng vector có chứa ít nhất 1 phần tử".to_owned())`
* Có thể chứa thông tin về lỗi
    - Nên: `Err(BadChar(c, position))`
    - Không nên: `Err("Không thể sử dụng kí tự + ở đây".to_owned())`
* Kết hợp được với các kiểu lỗi khác


```rust,editable
use std::fmt;

type Result<T> = std::result::Result<T, DoubleError>;

// Định nghĩa lỗi. Có thể được tùy biến tùy trường hợp xử lí lỗi riêng.
// Giờ đây ta sẽ có thể lập trình các lỗi của mình, dựa theo một triển khai lỗi gốc,
// hoặc xử lí trung gian. 
#[derive(Debug, Clone)]
struct DoubleError;

// Việc tạo lập một lỗi tách biệt hoàn toàn so với cách nó được hiển thị
// Chúng ta không cần phải lo lắng về việc xáo trộn các logic phức tạp với cách hiển thị.
//
// Lưu ý rằng, ta đang không hề lưu bất cứ thông tin nào về các lỗi. Đồng nghĩa với việc, nếu không 
// chỉnh lại kiểu dữ liệu nhằm đưa ra thông tin, ta sẽ không thể chỉ ra chính xác chuỗi kí tự nào
// đã thất bại trong việc hiển thị. 
impl fmt::Display for DoubleError {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "invalid first item to double")
    }
}

fn double_first(vec: Vec<&str>) -> Result<i32> {
    vec.first()
        // Chuyển lỗi về cùng kiểu dữ liệu lỗi mới
        .ok_or(DoubleError)
        .and_then(|s| {
            s.parse::<i32>()
                // Update to the new error type here also.
                .map_err(|_| DoubleError)
                .map(|i| 2 * i)
        })
}

fn print(result: Result<i32>) {
    match result {
        Ok(n) => println!("The first doubled is {}", n),
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
