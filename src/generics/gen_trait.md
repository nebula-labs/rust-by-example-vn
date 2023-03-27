# Traits
Tất nhiên `trait` cũng có thể  là generic. Ở đây chúng ta định nghĩa một phương thức triển khai lại `Drop trait` như một phương thức generic để  `Drop` chính nó và input.

```rust
// Các kiểu không thể coppy.
struct Empty;
struct Null;

// Một trait generic `T`.
trait DoubleDrop<T> {

    // Định nghĩa một method trên type hiện tại, method nhận một giá trị khác
    // cũng có kiểu `T` và không làm gì với nó.
    fn double_drop(self, _: T);
}

// Implement `DoubleDrop<T>` cho mọi generic parameter `T` và
// caller `U`.
impl<T, U> DoubleDrop<T> for U {
    // Method này sẽ take ownership của cả 2 tham số, 
    // sau đó giải phóng bộ nhớ cho cả 2.
    fn double_drop(self, _: T) {}
}
fn main() {
    let empty = Empty;
    let null  = Null;

    // Giải phóng `empty` and `null`.
    empty.double_drop(null);

    //empty;
    //null;
    // ^ TODO: Try uncommenting these lines.
}
```
