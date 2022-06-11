# Inference (Suy luận kiểu)

Công cụ suy luận kiểu của Rust khá thông minh. Nó không chỉ nhìn vào
kiểu của giá trị trong biểu thức khởi tạo. Nó còn xem xét cách các biến 
được sử dụng sau đó để suy ra kiểu của chúng. Đây là một ví dụ nâng cao 
về suy luận kiểu trong Rust:

```rust,editable
fn main() {
    // Bởi vì ở đây có chú thích kiểu, trình biên dịch biết rằng `elem` có kiểu u8.
    let elem = 5u8;

    // Tạo một vector trống (một mảng có thể thêm phần tử).
    let mut vec = Vec::new();
    // Tại thời điểm này, trình biên dịch không biết chính xác kiểu của `vec`, nó
    // chỉ biết rằng đây la một vector gì đó (`Vec<_>`).

    // Chèn thêm `elem` vào vector.
    vec.push(elem);
    // Và ở đây trình biên dịch đã biết `vec` là một vector của các số `u8` (`Vec<u8>`)
    // TODO ^ Thử comment dòng `vec.push(elem)` lại

    println!("{:?}", vec);
}
```

Như vậy là ở đây không cần chú thích kiểu cho các biến, trình biên dịch rất vui và 
lập trình viên cũng vậy!
