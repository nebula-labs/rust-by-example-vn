# Primitives (Kiểu dữ liệu nguyên thủy)

Rust cung cấp khá nhiều `primitives`:


### Scalar Types (Các kiểu vô hướng)

* Số nguyên có dấu: `i8`, `i16`, `i32`, `i64`, `i128` và `isize` (pointer size)
* Số nguyên không dấu: `u8`, `u16`, `u32`, `u64`, `u128` và `usize` (pointer
  size)
* Số thực dấu phẩy động: `f32`, `f64`
* `char` Ký tự Unicode ví dụ như: `'a'`, `'α'` và `'∞'` (Kích thước 4 byte)
* `bool` là `true` hoặc `false`
* và unit type `()` - kiểu đơn vị, chỉ có thể có giá trị là một tuple trống : `()`

Mặc dù giá trị của một unit type là một tuple, nhưng nó không được coi là
một kiểu hỗn hợp vì nó là tuple trống, không chứa giá trị.

### Compound Types (Các kiểu hỗn hợp)

* Mảng (arrays): `[1, 2, 3]`
* Tuples: `(1, true)`

Các biến trong Rust có thể là *type annotated*, nghĩa là chúng được chú thích kiểu dữ liệu khi khởi taọ. 
Các biến là số cũng có thể được chú thích thông qua *suffix* (hậu tố) hoặc *default* (theo mặc định). 
Theo mặc định thì các số nguyên là `i32` còn số thực là `f64`. 
Lưu ý rằng Rust cũng có thể suy ra kiểu dữ lỉệu của biến theo ngữ cảnh.

```rust,editable,ignore,mdbook-runnable
fn main() {
    // Các biến trong Rust có thể là `type annotated`.
    let logical: bool = true;

    let a_float: f64 = 1.0;  // Chú thích kiểu thông thường
    let an_integer   = 5i32; // Chú thích kiểu bằng hậu tố

    // Nếu không chú thích, kiểu của biến được xác định theo mặc định.
    let default_float   = 3.0; // `f64`
    let default_integer = 7;   // `i32`
    
    // Kiểu của biến có thể được Rust suy ra theo ngữ cảnh.
    let mut inferred_type = 12; // Kiểu i64 được suy ra từ một dòng code khác.
    inferred_type = 4294967296i64;
    
    // Một biến mutable (dùng từ khóa `mut` trước tên biến) thì có thể thay đổi giá trị
    let mut mutable = 12; // Mutable `i32`
    mutable = 21;
    
    // Ở đây sẽ báo lỗi! Bởi không thể thay đổi kiểu của biến.
    mutable = true;

    // Các biến có thể được ghi đè với shadowing.
    let mutable = true;
}
```

### Xem thêm tại đây:

[`std` library][std], [`mut`][mut], [`inference`][inference], và [`shadowing`][shadowing]

[std]: https://doc.rust-lang.org/std/
[mut]: variable_bindings/mut.md
[inference]: types/inference.md
[shadowing]: variable_bindings/scope.md
