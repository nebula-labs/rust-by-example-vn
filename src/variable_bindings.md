# Variable Bindings (Ràng buộc giữa kiểu và giá trị với biến trong Rust)

Rust cung cấp sự an toàn cho kiểu (type safety) bằng các kiểu tĩnh. Các ràng buộc biến có thể được
chú thích kiểu khi khai báo. Tuy nhiên, trong hầu hết các trường hợp, trình biên dịch sẽ có thể
để suy ra kiểu dữ liệu của biến thông qua ngữ cảnh, giảm đáng kể gánh nặng cho việc chú thích.

Các giá trị (như literals) có thể được gán vào cho các biến, thông qua `let` binding.

```rust,editable
fn main() {
    let an_integer = 1u32;
    let a_boolean = true;
    let unit = ();

    // copy `an_integer` vào `copied_integer`
    let copied_integer = an_integer;

    println!("An integer: {:?}", copied_integer);
    println!("A boolean: {:?}", a_boolean);
    println!("Meet the unit value: {:?}", unit);

    // Trình biên dịch sẽ cảnh báo về việc có tạo ràng buộc biến mà không sử dụng;
    // những cảnh báo này sẽ đươc tắt nến bạn thêm trước tên biến một dấu gạch dưới.
    let _unused_variable = 3u32;

    let noisy_unused_variable = 2u32;
    // FIXME ^ Thêm dấu gạch dưới phía trước để loại bỏ cảnh báo
    // Có điều là những cảnh báo này sẽ không hiển thị trên trình duyệt
    // Bạn có thể thấy chúng khi cope đoạn lệnh này vào trong code editor của bạn.
}
```
