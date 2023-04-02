# Mutability

Tính biến đổi của dữ liệu có thể thay đổi khi quyền sở hữu được chuyển giao.

```rust,editable
fn main() {
    let immutable_box = Box::new(5u32);

    println!("immutable_box chứa {}", immutable_box);

    // Lỗi biến đổi (không thể thay đổi giá trị biến immutable)
    //*immutable_box = 4;

    // *Move* hộp, sẽ thay đổi quyền sở hữu (và khả năng biến đổi) của nó
    let mut mutable_box = immutable_box;

    println!("mutable_box chứa {}", mutable_box);

    // Sửa đổi nội dung của hộp (hợp lệ vì biến là mutable)
    *mutable_box = 4;

    println!("mutable_box bây giờ chứa {}", mutable_box);
}
```