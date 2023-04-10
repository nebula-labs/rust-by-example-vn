# Repeat

Macros có thể sử dụng `+` trong danh sách đối số để biểu thị rằng một đối số có thể lặp lại ít nhất một lần,
hoặc `*`, để biểu thị rằng một đối số có thể lặp lại từ không cho đến nhiều lần.

Trong ví dụ sau đây, các kí hiệu xung quanh bộ so khớp (matcher) `$(...),+` sẽ phù hợp với
một hoặc nhiều biểu thức, được cách nhau bởi dấu phẩy.
Lưu ý rằng dấu chấm phẩy (;) là tùy chọn cho trường hợp cuối cùng.


```rust,editable
// `find_min!` sẽ tìm ra giá trị nhỏ nhất trong dãy đối số.
macro_rules! find_min {
    // Base case:
    ($x:expr) => ($x);
    // `$x` followed by at least one `$y,`
    ($x:expr, $($y:expr),+) => (
        // Call `find_min!` on the tail `$y`
        std::cmp::min($x, find_min!($($y),+))
    )
}
fn main() {
    println!("{}", find_min!(1));
    println!("{}", find_min!(1 + 2, 2));
    println!("{}", find_min!(5, 2 * 3, 4));
}
```