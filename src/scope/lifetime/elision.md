# Elision

Trường hợp các biến tồn tại suốt vòng đời (của một hàm) là rất phổ biến và vì thế trình kiểm tra mượn (borrow checker) cho phép bạn bỏ qua việc đánh dấu lifetime để tiết kiệm thời gian nhập và để dễ đọc hơn. Điều này được gọi là Elision. Cơ chế Elision tồn tại trong Rust bởi vì các trường hợp như thế này quá phổ biến.

Đoạn code dưới đây sễ đưa ra một vài ví dụ về cơ chế Elision. Để hiểu rõ hơn về cơ chế này, bạn có thể đọc tại [đây][lifetime ellision]: 
```rust,editable
// `elided_input` và `annotated_input` có mô tả (function signatures) giống nhau
// vì lifetime của `elided_input` được trình biên dịch xác định:
fn elided_input(x: &i32) {
    println!("`elided_input`: {}", x);
}

fn annotated_input<'a>(x: &'a i32) {
    println!("`annotated_input`: {}", x);
}

// Tương tự, `elided_pass` và `annotated_pass` cũng có những mô tả giống nhau
// vì lifetime được ngầm định thêm vào `elided_pass`:
fn elided_pass(x: &i32) -> &i32 { x }

fn annotated_pass<'a>(x: &'a i32) -> &'a i32 { x }

fn main() {
    let x = 3;

    elided_input(&x);
    annotated_input(&x);

    println!("`elided_pass`: {}", elided_pass(&x));
    println!("`annotated_pass`: {}", annotated_pass(&x));
}
```



### Xem thêm tại đây:

[elision][elision]


[lifetime ellision]: https://doc.rust-lang.org/book/ch10-03-lifetime-syntax.html#lifetime-elision
[elision]: https://doc.rust-lang.org/book/ch10-03-lifetime-syntax.html#lifetime-elision