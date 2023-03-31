# Chú thích lifetime rõ ràng

Trình kiểm tra mượn sử dụng chú thích lifetime rõ ràng để xác định thời gian tham chiếu có hiệu lực. Trong những trường hợp mà việc xác định lifetime không được bỏ qua[^1], Rust yêu cầu chú thích rõ ràng để xác định thời gian tồn tại của tham chiếu. Cú pháp để chú thích lifetime rõ ràng sử dụng ký tự nháy đơn như sau:

```rust,ignore
foo<'a>
// `foo` có tham số lifetime `'a`
```

Tương tự như [closures][anonymity], việc sử dụng lifetime yêu cầu sử dụng generics. Ngoài ra, cú pháp này cho biết lifetime của `foo` không thể vượt quá `'a`. Chú thích rõ ràng của một kiểu có dạng `&'a T` với `'a` đã được giới thiệu trước đó.

Trong những trường hợp với nhiều lifetime, cú pháp tương tự như sau:

```rust,ignore
foo<'a, 'b>
// `foo` có tham số lifetime là `'a` và `'b`
```

Trong trường hợp này, lifetime của `foo` không thể vượt quá `'a` _hoặc_ `'b`.

Xem ví dụ sau để thấy cách sử dụng chú thích lifetime rõ ràng:

```rust,editable,ignore,mdbook-runnable
// `print_refs` nhận 2 tham chiếu đến biến kiểu `i32`
// có các lifetime khác nhau là `'a` và `'b`. Hai lifetime này phải có lifetime
// ít nhất là bằng với lifetime của hàm `print_refs`.
fn print_refs<'a, 'b>(x: &'a i32, y: &'b i32) {
    println!("x is {} and y is {}", x, y);
}

// Một hàm không có đối số, nhưng có tham số lifetime `'a`.
fn failed_borrow<'a>() {
    let _x = 12;

    // ERROR: `_x` không có lifetime đủ dài
    let y: &'a i32 = &_x;
    // Cố sử dụng lifetime `'a` như là một chú thích rõ ràng
    // bên trong hàm sẽ bị lỗi vì lifetime của `&_x` ngắn hơn
    // so với `y`. Một lifetime ngắn không thể được ép thành một lifetime dài hơn.
}

fn main() {
    // Tạo biến để có thể được mượn bên dưới.
    let (four, nine) = (4, 9);

    // Mượn (`&`) cả hai biến và truyền chúng vào hàm `print_refs`.
    print_refs(&four, &nine);
    // Bất cứ tham số đầu vào nào đuợc mượn phải có lifetime dài hơn thứ mượn nó (ở đây là `print_refs`)
    // Nói cách khác, lifetime của `four` và `nine`
    // phải dài hơn lifetime của `print_refs`.

    failed_borrow();
    // Hàm `failed_borrow` không có tham chiếu nào để buộc `'a` phải
    // có lifetime dài hơn lifetime của hàm, nhưng `'a` vẫn có lifetime dài hơn.
    // Bởi vì lifetime không bị giới hạn, nó mặc định là `'static`.
}
```

[^1]: [elision] chú thích lifetime không rõ ràng và sự khác biệt.

### Xem thêm:

[generics][generics] và [closures][closures]

[anonymity]: ../../fn/closures/anonymity.md
[closures]: ../../fn/closures.md
[elision]: elision.md
[generics]: ../../generics.md
