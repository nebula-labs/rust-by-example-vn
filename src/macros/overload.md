# Overload
Macros có thể được nạp chồng để chấp nhận những kiểu kết hợp khác nhau của các đối số.
Khi đó, `macro_rules!` hoạt động tương tự như là một đoạn mã khớp mẫu (a `match` block):

```rust,editable
// `test!` sẽ so sánh `$left` and `$right`
// theo những cách khác nhau tùy thuộc vào cách bạn gọi nó:
macro_rules! test {
    // Các đối số không nhất thiết phải được tách ra bằng dấu phẩy
    // Có thể sử dụng bất cứ kiểu nào cũng được
    ($left:expr; and $right:expr) => {
        println!("{:?} and {:?} is {:?}",
                 stringify!($left),
                 stringify!($right),
                 $left && $right)
    };
    // ^ mỗi nhánh phải được kết thúc bằng dấu chấm phẩy.
    ($left:expr; or $right:expr) => {
        println!("{:?} or {:?} is {:?}",
                 stringify!($left),
                 stringify!($right),
                 $left || $right)
    };
}

fn main() {
    test!(1i32 + 1 == 2i32; and 2i32 * 2 == 4i32);
    test!(true; or false);
}
```