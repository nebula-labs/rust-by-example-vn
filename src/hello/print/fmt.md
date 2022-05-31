# Formatting (Định dạng dữ liệu)

Chúng ta thấy ở đây, việc định dạng dữ liệu được chỉ định thông qua một *format string* (xâu định dạng):

* `format!("{}", foo)` -> `"3735928559"`
* `format!("0x{:X}", foo)` ->
  [`"0xDEADBEEF"`][deadbeef]
* `format!("0o{:o}", foo)` -> `"0o33653337357"`

Cùng một biến (`foo`) nhưng có thể được định dạng theo nhiều cách khác nhau phụ thuộc vào
*argument type* (kiểu đối số) nào được sử dụng: `X` hoặc `o` hoặc *unspecified* (không xác định).

Chức năng định dạng này được triển khai thông qua các `traits`, mỗi trait
tương ứng với một loại đối số. Trait định dạng phổ biến nhất là `Display`, nó 
xử lý các trường hợp mà ở đó kiểu đối số là không xác định: ví dụ:`{}`.

```rust,editable
use std::fmt::{self, Formatter, Display};

struct City {
    name: &'static str,
    // Vĩ độ
    lat: f32,
    // Kinh độ
    lon: f32,
}

impl Display for City {
    // `f` là một buffer (vùng đệm), và phương thức này phải ghi xâu định dạng vào nó
    fn fmt(&self, f: &mut Formatter) -> fmt::Result {
        let lat_c = if self.lat >= 0.0 { 'N' } else { 'S' };
        let lon_c = if self.lon >= 0.0 { 'E' } else { 'W' };

        // `write!` cũng như `format!`, nhưng nó sẽ ghi xâu định dạng vào trong một buffer `f`.
        write!(f, "{}: {:.3}°{} {:.3}°{}",
               self.name, self.lat.abs(), lat_c, self.lon.abs(), lon_c)
    }
}

#[derive(Debug)]
struct Color {
    red: u8,
    green: u8,
    blue: u8,
}

fn main() {
    for city in [
        City { name: "Dublin", lat: 53.347778, lon: -6.259722 },
        City { name: "Oslo", lat: 59.95, lon: 10.75 },
        City { name: "Vancouver", lat: 49.25, lon: -123.1 },
    ].iter() {
        println!("{}", *city);
    }
    for color in [
        Color { red: 128, green: 255, blue: 90 },
        Color { red: 0, green: 3, blue: 254 },
        Color { red: 0, green: 0, blue: 0 },
    ].iter() {
        // Ở đây, hãy triển khai fmt::Display cho `color` để có thể
        // sử dụng `{}`
        println!("{:?}", *color);
    }
}
```

Bạn có thể xem [danh sách đầy đủ các formatting traits][fmt_traits] và các kiểu đối số của chúng
tại [`std::fmt`][fmt] documentation.

### Thực hành
Triển khai `fmt::Display` trait cho cấu trúc `Color` phía trên
để output được in ra có kết quả như thế này:

```text
RGB (128, 255, 90) 0x80FF5A
RGB (0, 3, 254) 0x0003FE
RGB (0, 0, 0) 0x000000
```

Hai gợi ý nhỏ nếu bạn gặp khó khăn trong bài tập trên:
 * Bạn [có lẽ sẽ cần liệt kê các màu nhiều hơn một lần, cho nên hãy đặt tên cho chúng][named_parameters],
 * Bạn có thể [đệm một số lượng số 0 để độ dài xâu hiển thị bằng 2][fmt_width] với `:0>2`.

### Xem thêm tại đây:

[`std::fmt`][fmt]

[named_parameters]: https://doc.rust-lang.org/std/fmt/#named-parameters
[deadbeef]: https://en.wikipedia.org/wiki/Deadbeef#Magic_debug_values
[fmt]: https://doc.rust-lang.org/std/fmt/
[fmt_traits]: https://doc.rust-lang.org/std/fmt/#formatting-traits
[fmt_width]: https://doc.rust-lang.org/std/fmt/#width
