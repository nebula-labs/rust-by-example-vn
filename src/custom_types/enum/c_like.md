# C-like

`enum` trong Rust cũng có thể ở dạng C-like enums (enum trong ngôn ngữ C).

```rust,editable
// Đây là một attribute dùng để ẩn cảnh báo về những đoạn code không sử dụng.
#![allow(dead_code)]

// enum với giá trị phần tử là ngầm định (gán theo vị trí, bắt đầu từ 0)
enum Number {
    Zero, // 0
    One, // 1
    Two, // 2
}

// enum với giá trị phần tử được gán cụ thể
enum Color {
    Red = 0xff0000,
    Green = 0x00ff00,
    Blue = 0x0000ff,
}

fn main() {
    // `enums` có thể ép kiểu sang số nguyên.
    println!("zero is {}", Number::Zero as i32);
    println!("one is {}", Number::One as i32);

    println!("roses are #{:06x}", Color::Red as i32);
    println!("violets are #{:06x}", Color::Blue as i32);
}
```

### Xem thêm tại đây:

[casting][cast]

[cast]: ../../types/cast.md

