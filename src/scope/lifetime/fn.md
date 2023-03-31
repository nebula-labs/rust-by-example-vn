# Functions

Bỏ qua [elision], mô tả hàm có chú thích lifetime có một vài ràng buộc:

- bất cứ tham chiếu nào _phải_ có một chú thích lifetime.
- bất cứ tham chiếu nào được trả về _phải_ có cùng một lifetime với một tham số đầu vào hoặc là `static`.

Ngoài ra, lưu ý rằng trả về tham chiếu mà không có tham số đầu vào là không được phép nếu nó sẽ dẫn đến việc trả về tham chiếu đến dữ liệu không hợp lệ. Ví dụ sau đây cho thấy một số hình thức hợp lệ đối với các hàm có lifetime:

```rust,editable
// Một tham chiếu đầu vào với lifetime `'a` phải có lifetime
// ít nhất là cho đến khi hàm kết thúc.
fn print_one<'a>(x: &'a i32) {
    println!("`print_one`: x is {}", x);
}

// Các tham chiếu có thể sửa đổi (mutable) cũng có thể có lifetime.
fn add_one<'a>(x: &'a mut i32) {
    *x += 1;
}

// Nhiều thành phần với các lifetime khác nhau. Trong trường hợp này,
// nó sẽ không sao nếu cả hai có cùng một lifetime `'a`, nhưng
// trong các trường hợp phức tạp hơn, các lifetime khác nhau có thể được yêu cầu.
fn print_multi<'a, 'b>(x: &'a i32, y: &'b i32) {
    println!("`print_multi`: x is {}, y is {}", x, y);
}


// Trả về các tham chiếu được truyền vào là hợp lệ.
// Tuy nhiên, nó phải được trả về với đúng lifetime được truyền vào.
fn pass_x<'a, 'b>(x: &'a i32, _: &'b i32) -> &'a i32 { x }

//fn invalid_output<'a>() -> &'a String { &String::from("foo") }
// Hàm trên không hợp lệ: `'a` phải có lifetime lâu hơn hàm.
// Ở đây, `&String::from("foo")` sẽ tạo ra một `String`, theo sau bởi một
// tham chiếu. Sau đó dữ liệu sẽ bị hủy đi khi thoát khỏi phạm vi, để lại
// một tham chiếu đến dữ liệu không hợp lệ để được trả về.

fn main() {
    let x = 7;
    let y = 9;

    print_one(&x);
    print_multi(&x, &y);

    let z = pass_x(&x, &y);
    print_one(z);

    let mut t = 3;
    add_one(&mut t);
    print_one(&t);
}
```

### Xem thêm:

[functions][fn]

[elision]: elision.md
[fn]: fn.md
