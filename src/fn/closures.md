# Closures

Closures là các hàm có thể capture môi trường bao quanh. Ví dụ, closure capture biến `x`:

```Rust
|val| val + x
```

Cú pháp và khả năng của closures làm cho chúng rất tiện lợi cho việc sử dụng ngay lập tức.
Gọi một closure hoàn toàn giống như gọi một hàm.
Tuy nhiên, cả kiểu dữ liệu đầu vào và đầu ra có thể được suy luận và tên biến đầu vào phải được chỉ định.

Một số đặc điểm khác của closures bao gồm:
* Sử dụng `||` thay vì `()` để bao quanh biến đầu vào.
* Tùy chọn việc đặt dấu mở ngoặc nhọn (`{}`) cho một biểu thức đơn (bắt buộc nếu không phải là biểu thức đơn).
* Khả năng capture được các biến môi trường bên ngoài (outer environment variables).

```rust,editable
fn main() {
    let outer_var = 42;
    
    // Một hàm thông thường không thể tham chiếu đến các biến trong môi trường bao quanh nó.
    //fn function(i: i32) -> i32 { i + outer_var }
    // TODO:  xóa comment trên và xem lỗi biên dịch. Trình biên dịch
    // gợi ý chúng ta nên định nghĩa một closure thay vì.

    // Closures là vô danh, ở đây chúng ta đang liên kết chúng với các tham chiếu.
    // Chú thích giống với chú thích của hàm, nhưng là tùy chọn
    // giống như các dấu ngoặc nhọn `{}` bao quanh thân hàm. Những hàm vô danh này
    // được gán cho các biến được đặt tên phù hợp.
    let closure_annotated = |i: i32| -> i32 { i + outer_var };
    let closure_inferred  = |i     |          i + outer_var  ;

    // Gọi các closures.
    println!("closure_annotated: {}", closure_annotated(1));
    println!("closure_inferred: {}", closure_inferred(1));
    // Một khi kiểu dữ liệu của closure đã được suy ra, thì nó không thể được suy ra lại với một kiểu dữ liệu khác.
    //println!("cannot reuse closure_inferred with another type: {}", closure_inferred(42i64));
    // TODO: bỏ ghi chú dòng trên và xem lỗi trình biên dịch

    // Một closure không có đối số, trả về một `i32`.
    // Kiểu dữ liệu trả về được suy luận tự động.
    let one = || 1;
    println!("closure returning one: {}", one());

}
```
