# `cfg`

Có thể kiểm tra điều kiện cấu hình thông qua 2 toán tử khác nhau:

* thuộc tính `cfg`: `#[cfg(...)]` ở vị trí thuộc tính
* macro `cfg!`: `cfg!(...)` trong biểu thức boolean

Trong khi cái đầu tiên sử dụng kiểm tra điều kiện khi biên dịch, điều kiện sau 
đánh giá là `true` hay `false` trong thời gian chạy. Cả 2 đều dùng những tham số giống nhau.

`cfg!`, không giống `#[cfg]`, không xóa bất kỳ mã nào và chỉ đánh giá `true` hoặc `false`. Ví dụ, tất cả các block trong một câu điều kiện if/else cần phải hợp lệ khi sử dụng marco `cfg!`, bất kể cái gì `cfg!` đang đánh giá.

```rust,editable
// Hàm này chỉ được biên dịch nếu OS là Linux
#[cfg(target_os = "linux")]
fn are_you_on_linux() {
    println!("You are running linux!");
}

// Và hàm này chỉ được biên dịch nếu OS *không* là linux
#[cfg(not(target_os = "linux"))]
fn are_you_on_linux() {
    println!("You are *not* running linux!");
}

fn main() {
    are_you_on_linux();

    println!("Are you sure?");
    if cfg!(target_os = "linux") {
        println!("Yes. It's definitely linux!");
    } else {
        println!("Yes. It's definitely *not* linux!");
    }
}
```

### See also:

[the reference][ref], [`cfg!`][cfg], and [macros][macros].

[cfg]: https://doc.rust-lang.org/std/macro.cfg!.html
[macros]: ../macros.md
[ref]: https://doc.rust-lang.org/reference/attributes.html#conditional-compilation