# Literals

Numeric literals có thể được chú thích kiểu bằng việc thêm vào suffix. Như ví dụ dưới đây,
ta có thể chỉ định rằng literal `42` sẽ có kiểu là `i32`, bằng cách viết `42i32`.

Các numeric literals mà không có suffix thì kiểu của nó sẽ được xác định theo cách mà nó 
được sử dụng. Nếu không có ràng buộc nào xuất hiện, trình biên dịch sẽ sử dụng `i32` cho các 
số nguyên, và `f64` cho các số thực dấu phẩy động.

```rust,editable
fn main() {
    // Literals có suffix, kiểu của chúng được xác định lúc khởi tạo biến
    let x = 1u8;
    let y = 2u32;
    let z = 3f32;

    // Literals không có suffix, kiểu phụ thuộc vào cách chúng được sử dụng
    let i = 1;
    let f = 1.0;

    // `size_of_val` trả về kích thước của biến theo bytes
    println!("size of `x` in bytes: {}", std::mem::size_of_val(&x));
    println!("size of `y` in bytes: {}", std::mem::size_of_val(&y));
    println!("size of `z` in bytes: {}", std::mem::size_of_val(&z));
    println!("size of `i` in bytes: {}", std::mem::size_of_val(&i));
    println!("size of `f` in bytes: {}", std::mem::size_of_val(&f));
}
```

Có một số khái niệm được sử dụng trong đoạn mã code trên chưa được giải thích,
dưới đây là giải thích ngắn gọn cho chúng:

* `std::mem::size_of_val` là một hàm, nhưng được gọi dưới dạng *full path*. Code
  được chia thành các đơn vị logic gọi là *modules*. Trong trường hợp này, hàm
  `size_of_val` được định nghĩa trong `mem` module, và `mem` module
  được định nghĩa trong `std` *crate*. Để biết thêm chi tiết, hãy xem thêm về
  [modules][mod] và [crates][crate].

[mod]: ../mod.md
[crate]: ../crates.md

