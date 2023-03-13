# Type anonymity

Closures succinctly capture variables from enclosing scopes. Điều này có bất kì hậu quả gì không? Chắc chắn là có. Observe bằng cách sử dụng một closure như là một function parameter yêu cầu là [generics], điều này là cần thiết vì các chúng được định nghĩa:

```rust
// `F` phải là một generic.
fn apply<F>(f: F) where
    F: FnOnce() {
    f();
}
```

Khi một closure được định nghĩa, compiler mặc nhiên tạo ra một kiểu cấu trúc không xác định mới để lưu giữ các biến bên trong, trong lúc triển khai các chức năng qua một kiểu `traits` là: `Fn`, `FnMut`, hoặc
`FnOnce` cho một kiểu không xác định này. Kiểu dữ liệu này được gán cho biến được lưu trữ khi gọi đến.

Vì kiểu dữ liệu mới này là kiểu dữ liệu không xác định, bất kì cách sử dụng nào trong một function sẽ yêu cầu kiểu generics. Tuy nhiên, một tham số kiểu không giới hạn(unbounded) `<T>` vẫn sẽ là mơ hồ và không được phép. Như vậy, giới hạn bởi một trong các kiểu `traits` là: `Fn`, `FnMut`, hoặc
`FnOnce` (kiểu nó được triển khai) là đủ để xác định loại của nó.

```rust,editable
// `F` phải triển khai `Fn` cho một closure mà không có inputs
// và không trả lại cái gì - chính xác những gì được yêu cầu là `print`
fn apply<F>(f: F) where
    F: Fn() {
    f();
}

fn main() {
    let x = 7;

    // Capture `x` vào một kiểu ẩn danh và triển khai `Fn` cho nó
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