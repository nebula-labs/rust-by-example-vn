# while let

Tương tự như `if let`, `while let` cũng có thể khiến một chuỗi `match` rườm rà trở nên đẹp hơn. Hãy xem xét trình tăng dần `i` bên dưới:

```rust
// Biến `optional` có kiểu `Option<i32>`
let mut optional = Some(0);

// Lặp đi lặp lại test này.
loop {
    match optional {
        // Nếu `optional` được destructure, khối lệnh sau sẽ được dùng.
        Some(i) => {
            if i > 9 {
                println!("Greater than 9, quit!");
                optional = None;
            } else {
                println!("`i` is `{:?}`. Try again.", i);
                optional = Some(i + 1);
            }
            // ^ Yêu cầu 3 lần thụt đầu dòng!
        },
        // Thoát vòng lặp nếu việc tách dữ liệu thất bại:
        _ => { break; }
        // ^ Dòng cuối này có vẻ thừa thãi nhưng lại bắt buộc phải có. Cần có một cách tốt hơn!
    }
}
```

Sử dụng `while let` sẽ giúp chuỗi lặp `match` trên gọn gàng hơn:

```rust,editable
fn main() {
    // Biến `optional` có kiểu `Option<i32>`
    let mut optional = Some(0);

    // Cấu trúc `white let` được dùng như sau: "while `let` sẽ destructure `optional` thành
    // `Some(i)`, đánh giá trong các block (`{}`).
    while let Some(i) = optional {
        if i > 9 {
            println!("Greater than 9, quit!");
            optional = None;
        } else {
            println!("`i` is `{:?}`. Try again.", i);
            optional = Some(i + 1);
        }
        // ^ Ít thụt đầu dòng hơn `if let` và cũng 
        // không đòi hỏi xử lý trường hợp failure.
    }
    // ^ `if let` có tuỳ chọn bổ sung thêm `else`/`else if`. 
    // Nhưng `while let` không có và không cần điều này.
}
```

### Xem thêm tại đây:

[`enum`][enum], [`Option`][option], và [white let RFC][while_let_rfc]

[enum]: ../custom_types/enum.md
[option]: ../std/option.md
[while_let_rfc]: https://github.com/rust-lang/rfcs/pull/214
