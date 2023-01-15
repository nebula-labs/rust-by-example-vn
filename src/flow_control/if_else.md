# if/else

Cấu trúc phân nhánh `if`-`else` trong Rust tương tự như các ngôn ngữ khác. Nhưng không giống như nhiều ngôn ngữ khác, điều kiện boolean không cần được bao quanh bởi dấu ngoặc đơn và mỗi điều kiện được theo sau bởi một khối lệnh. Các điều kiện `if`-`else` là các biểu thức và tất cả các nhánh phải trả về cùng một kiểu.

```rust,editable
fn main() {
    let n = 5;

    if n < 0 {
        print!("{} is negative", n);
    } else if n > 0 {
        print!("{} is positive", n);
    } else {
        print!("{} is zero", n);
    }

    let big_n =
        if n < 10 && n > -10 {
            println!(", and is a small number, increase ten-fold");

            // Biểu thức này trả về một số nguyên `i32`.
            10 * n
        } else {
            println!(", and is a big number, halve the number");

            // Biểu thức này cũng phải trả về một số nguyên `i32`.
            n / 2
            // TODO ^ Hãy thử thêm dấu chấm phẩy ở đây và xem lỗi gì xảy ra.
        };
    //   ^ Đừng quên đặt dấu chấm phẩy ở cuối! Tất cả các `let` binding đều cần nó.

    println!("{} -> {}", n, big_n);
}
```
