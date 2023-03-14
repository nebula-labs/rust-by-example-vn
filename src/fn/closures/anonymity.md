# Type anonymity

Closure (đóng gói) nhắm gọn các biến từ phạm vi bao quanh một cách ngắn gọn. Điều này có gây ra bất kỳ hệ quả gì không? Chắc chắn là có. Dễ thấy rằng sử dụng một closure như một tham số của hàm đòi hỏi sử dụng [generics], điều này cần thiết bởi cách chúng được định nghĩa:

```rust
// `F` phải là một generic.
fn apply<F>(f: F) where
    F: FnOnce() {
    f();
}
```

Khi một closure được định nghĩa, compiler tự động tạo ra một kiểu cấu trúc không xác định mới để lưu giữ các biến bên trong closure, trong khi đó, triển khai chức năng thông qua một trong các `traits`: `Fn`, `FnMut`, hoặc `FnOnce` cho kiểu không xác định đó. Kiểu dữ liệu này được gán cho biến và được lưu trữ khi gọi đến.

Vì kiểu dữ liệu mới này là kiểu dữ liệu không xác định, bất kì cách sử dụng nào trong một function sẽ yêu cầu kiểu generics. Tuy nhiên, một tham số kiểu không giới hạn(unbounded) `<T>` vẫn sẽ là mơ hồ và không được phép. Vì vậy, ràng buộc bởi một trong các `traits`: `Fn`, `FnMut`, hoặc
`FnOnce` (kiểu nó được triển khai) là đủ để xác định kiểu của nó.

```rust,editable
// `F` phải triển khai `Fn` cho một closure mà không có inputs
// và không trả lại gì cả - chính xác những gì được yêu cầu cho `print`
fn apply<F>(f: F) where
    F: Fn() {
    f();
}

fn main() {
    let x = 7;

    // Capture `x` vào một kiểu không xách định và triển khai `Fn` cho nó
    // Lưu trữ nó trong `print`.
    let print = || println!("{}", x);

    apply(print);
}
```

### See also:

[A thorough analysis][thorough_analysis], [`Fn`][fn], [`FnMut`][fn_mut],
and [`FnOnce`][fn_once]

[generics]: ../../generics.md
[fn]: https://doc.rust-lang.org/std/ops/trait.Fn.html
[fn_mut]: https://doc.rust-lang.org/std/ops/trait.FnMut.html
[fn_once]: https://doc.rust-lang.org/std/ops/trait.FnOnce.html
[thorough_analysis]: https://huonw.github.io/blog/2015/05/finding-closure-in-rust/