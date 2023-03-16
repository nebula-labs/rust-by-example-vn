# Input functions

Vì closures có thể được sử dụng như là đối số, bạn có thể thắc mắc liệu
có thể áp dụng như vậy đối với các hàm được không. Và thực sự là có thể! Nếu bạn khai báo một
hàm mà nó nhận một closure làm tham số, thì bất kỳ hàm nào thỏa mãn điều kiện
của closure đó có thể được truyền vào làm tham số.

```rust,editable
// Định nghĩa một hàm mà nó nhận một đối số generic `F`
// ràng buộc nó bằng `Fn`, và gọi nó
fn call_me<F: Fn()>(f: F) {
    f();
}

// Định nghĩa một hàm ở bên ngoài thỏa mãn ràng buộc `Fn`
fn function() {
    println!("I'm a function!");
}

fn main() {
    // Định nghĩa một closure thỏa mãn ràng buộc `Fn`
    let closure = || println!("I'm a closure!");

    call_me(closure);
    call_me(function);
}
```

Một điều cần lưu ý thêm à, `Fn`, `FnMut`, và `FnOnce` `traits` chỉ định
cách một closure lấy các biến từ phạm vi bao quanh.

### Xem thêm:

[`Fn`][fn], [`FnMut`][fn_mut], and [`FnOnce`][fn_once]

[fn]: https://doc.rust-lang.org/std/ops/trait.Fn.html
[fn_mut]: https://doc.rust-lang.org/std/ops/trait.FnMut.html
[fn_once]: https://doc.rust-lang.org/std/ops/trait.FnOnce.html
