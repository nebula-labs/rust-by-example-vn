# enums

Một `enum` được phân tách như sau:

```rust,editable
// `allow` dùng để tắt warning vì ở đây chỉ có 
// một biến thể của enum được sử dụng.
#[allow(dead_code)]
enum Color {
    // Ba biến thể này được chỉ định bằng tên.
    Red,
    Blue,
    Green,
    // Năm biến thể này là các `u32` tuple với tên là các color model (mô hình màu) khác nhau
    RGB(u32, u32, u32),
    HSV(u32, u32, u32),
    HSL(u32, u32, u32),
    CMY(u32, u32, u32),
    CMYK(u32, u32, u32, u32),
}

fn main() {
    let color = Color::RGB(122, 17, 40);
    // TODO ^ Hãy thử các biến thể khác cho `color`

    println!("What color is it?");
    // Một `enum` có thể được phân tách bằng cách sử dụng `match`.
    match color {
        Color::Red   => println!("The color is Red!"),
        Color::Blue  => println!("The color is Blue!"),
        Color::Green => println!("The color is Green!"),
        Color::RGB(r, g, b) =>
            println!("Red: {}, green: {}, and blue: {}!", r, g, b),
        Color::HSV(h, s, v) =>
            println!("Hue: {}, saturation: {}, value: {}!", h, s, v),
        Color::HSL(h, s, l) =>
            println!("Hue: {}, saturation: {}, lightness: {}!", h, s, l),
        Color::CMY(c, m, y) =>
            println!("Cyan: {}, magenta: {}, yellow: {}!", c, m, y),
        Color::CMYK(c, m, y, k) =>
            println!("Cyan: {}, magenta: {}, yellow: {}, key (black): {}!",
                c, m, y, k),
        // `match` không cần thêm các nhánh khác vì tất cả các biến thể của enum đã được kiểm tra
    }
}
```

### Xem thêm tại đây:

[`#[allow(...)]`][allow], [color models][color_models] và [`enum`][enum]

[allow]: ../../../attribute/unused.md
[color_models]: https://en.wikipedia.org/wiki/Color_model
[enum]: ../../../custom_types/enum.md
