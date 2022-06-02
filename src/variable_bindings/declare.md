# Declare first (Khai báo trước, khởi tạo sau)

Có thể khai báo các ràng buộc biến trước rồi khởi tạo chúng sau.
Tuy nhiên, hình thức này hiếm khi được sử dụng bởi vì nó có thể dẫn đến việc sử dụng
những biến chưa được khởi tạo.

```rust,editable,ignore,mdbook-runnable
fn main() {
    // Khai báo một ràng buộc biến
    let a_binding;

    {
        let x = 2;

        // Khởi tạo ràng buộc biến đã khai báo phía trên
        a_binding = x * x;
    }

    println!("a binding: {}", a_binding);

    let another_binding;

    // Lỗi! Không được sử dụng ràng buộc chưa khởi tạo
    println!("another binding: {}", another_binding);
    // FIXME ^ Comment dòng trên lại

    another_binding = 1;

    println!("another binding: {}", another_binding);
}
```

Trình biên dịch cấm sử dụng các biến chưa được khởi tạo, vì điều này sẽ dẫn đến những
hành vi không xác định.