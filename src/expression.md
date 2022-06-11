# Expressions (Biểu thức trong Rust)

Một chương trình Rust (hầu hết) được tạo thành từ một loạt các câu lệnh (statement):

```rust,editable
fn main() {
    // câu lệnh
    // câu lệnh
    // câu lệnh
}
```

Có một số loại câu lệnh trong Rust. Hai loại phổ biến nhất là khai báo 
ràng buộc biến và sử dụng biểu thức với dấu `;` ở cuối:

```rust,editable
fn main() {
    // ràng buộc biến
    let x = 5;

    // biểu thức;
    x;
    x + 1;
    15;
}
```

Các khối lệnh cũng là biểu thức, vì vậy chúng có thể được sử dụng làm vế phải trong
một phép gán. Biểu thức cuối cùng trong khối sẽ được gán cho vế trái chẳng hạn như một 
biến cục bộ. Tuy nhiên, nếu biểu thức cuối cùng của khối kết thúc bằng
dấu chấm phẩy, giá trị trả về sẽ là `()`.

```rust,editable
fn main() {
    let x = 5u32;

    let y = {
        let x_squared = x * x;
        let x_cube = x_squared * x;

        // Giá trị của biểu thức này được gán cho `y`
        x_cube + x_squared + x
    };

    let z = {
        // Dấu chấm phẩy loại bỏ biểu thức này và `()` được gán cho `z`
        2 * x;
    };

    println!("x is {:?}", x);
    println!("y is {:?}", y);
    println!("z is {:?}", z);
}
```
