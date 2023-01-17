# pointers/ref

Đối với con trỏ, cần phân biệt giữa destructuring và dereferencing 
vì chúng là những khái niệm khác nhau được sử dụng trong Rust theo 
cách khác với các ngôn ngữ như C/C++.

 * Dereferencing (Truy cập dữ liệu có trong một vị trí trong bộ nhớ 
 được trỏ tới một con trỏ) sử dụng `*`
 * Destructuring (Tách dữ liệu) sử dụng `&`, `ref`, và `ref mut`

```rust,editable
fn main() {
    // Gán một tham chiếu với kiểu `i32`. `&` có nghĩa là ở đây có một 
    // tham chiếu.
    let reference = &4;

    match reference {
        // Nếu `reference` match với `&val`, nó là kết quả của 
        // một so sánh giữa:
        // `&i32` và `&val`
        // ^ We see that if the matching `&`s are dropped, then the `i32`
        // should be assigned to `val`.
        &val => println!("Got a value via destructuring: {:?}", val),
    }

    // Để tránh dùng `&`, bạn có thể dereference bằng `*` trước khi thực hiện matching.
    match *reference {
        val => println!("Got a value via dereferencing: {:?}", val),
    }

    // Điều gì sẽ xảy ra nếu bạn không bắt đầu với một tham chiếu? `reference` là một tham chiếu
    // vì phía bên phải đã là một tham chiếu: `&4`. Còn biến dưới đây không phải là
    // tham chiếu vì vế phải không phải là một tham chiếu.
    let _not_a_reference = 3;

    // Rust cung cấp từ khoá `ref` cho mục đích này. Nó sửa đổi
    // việc gán để tạo ra một tham chiếu cho phần tử; dưới đây là một
    // tham chiếu.
    let ref _is_a_reference = 3;

    // Theo đó, bằng cách định nghĩa 2 giá trị không có tham chiếu, tham chiếu
    // có thể lấy thông qua `ref` và `ref mut`.
    let value = 5;
    let mut mut_value = 6;

    // Sử dụng `ref` để tạo một tham chiếu.
    match value {
        ref r => println!("Got a reference to a value: {:?}", r),
    }

    // Sử dụng `ref mut` cũng tương tự.
    match mut_value {
        ref mut m => {
            // Ở đây có một tham chiếu. Cần dereference nó trước khi có thể
            // thêm bất cứ thứ gì vào nó.
            *m += 10;
            println!("We added 10. `mut_value`: {:?}", m);
        },
    }
}
```

### Xem thêm tại đây:

[The ref pattern](../../../scope/borrow/ref.md)
