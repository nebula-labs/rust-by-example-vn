# Freezing (Đóng băng dữ liệu)

Khi dữ liệu của một ràng buộc biến bị shadow bởi một ràng buộc có tính bất biến thì nó cũng sẽ bị *freezes* (đóng băng). 
Dữ liệu bị đóng băng sẽ không có khả năng sửa đổi cho đến khi ra khỏi phạm vi của ràng bụôc bất biến trên:

```rust,editable,ignore,mdbook-runnable
fn main() {
    // Ràng buộc biến này có tính khả biến
    let mut _mutable_integer = 7i32;

    {
        // Shadowing bởi một `_mutable_integer` bất biến
        let _mutable_integer = _mutable_integer;

        // Lỗi! Vì `_mutable_integer` bị đóng băng ở scope này
        _mutable_integer = 50;
        // FIXME ^ Comment dòng trên lại

        // `_mutable_integer` đi ra khỏi scope này
    }

    // Không có lỗi! Vì `_mutable_integer` không bị đóng băng ở scope này
    _mutable_integer = 3;
}
```

