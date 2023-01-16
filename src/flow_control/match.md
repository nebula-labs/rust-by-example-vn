# match

Rust cung cấp một pattern matching (đối sánh mẫu) với từ khóa `match`, có thể được sử dụng giống
như `switch` trong C. Nhánh (`arm`) đầu tiên khớp dữ liệu sẽ được đánh giá và tất 
cả các giá trị có thể có phải được bao phủ trong các nhánh (độ bao phủ bắt buộc là 100%).

```rust,editable
fn main() {
    let number = 13;
    // TODO ^ Thử một giá trị khác cho `number`

    println!("Tell me about {}", number);
    match number {
        // Match với 1 giá trị đơn 
        1 => println!("One!"),
        // Match với nhiều giá trị
        2 | 3 | 5 | 7 | 11 => println!("This is a prime"),
        // TODO ^ Thử thêm 13 vào danh sách các số lẻ trên
        // Match với một phạm vi
        13..=19 => println!("A teen"),
        // Xử lý các trường hợp còn lại
        _ => println!("Ain't special"),
        // TODO ^ Hãy thử comment dòng bên trên
    }

    let boolean = true;
    // Match cũng có thể là một biểu thức
    let binary = match boolean {
        // Các nhánh của một match phải cover tất cả các giá trị có thể có
        false => 0,
        true => 1,
        // TODO ^ Hãy thử comment một trong các nhánh trên
    };

    println!("{} -> {}", boolean, binary);
}
```
