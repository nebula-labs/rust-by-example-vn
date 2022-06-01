# Enums (Kiểu liệt kê)

Từ khóa `enum` cho phép tạo ra một kiểu dữ liệu đặc biệt mà cho phép một biến có giá trị
là một biến thể trong một tập hợp các biến thể được định sẵn. 

```rust,editable
// Tạo một ra `enum` để phân loại các sự kiện web. Chú ý cách mà hai
// thông tin về tên và kiểu được dùng để xác định một biến thể:
// `PageLoad != PageUnload` và `KeyPress(char) != Paste(String)`.
// Mỗi một biến thể trong `enum` đều là khác nhau và độc lập.
enum WebEvent {
    // Giống unit structs,
    PageLoad,
    PageUnload,
    // giống tuple structs,
    KeyPress(char),
    Paste(String),
    // giống structs truyền thống.
    Click { x: i64, y: i64 },
}

// Một hàm lấy `WebEvent` enum làm đối số và không trả về giá trị.
fn inspect(event: WebEvent) {
    match event {
        WebEvent::PageLoad => println!("page loaded"),
        WebEvent::PageUnload => println!("page unloaded"),
        // Destructure `c` từ bên trong `enum`.
        WebEvent::KeyPress(c) => println!("pressed '{}'.", c),
        WebEvent::Paste(s) => println!("pasted \"{}\".", s),
        // Destructure `Click` thành `x` và `y`.
        WebEvent::Click { x, y } => {
            println!("clicked at x={}, y={}.", x, y);
        },
    }
}

fn main() {
    let pressed = WebEvent::KeyPress('x');
    // `to_owned()` tạo một owned `String` từ một string slice 
    let pasted  = WebEvent::Paste("my text".to_owned());
    let click   = WebEvent::Click { x: 20, y: 80 };
    let load    = WebEvent::PageLoad;
    let unload  = WebEvent::PageUnload;

    inspect(pressed);
    inspect(pasted);
    inspect(click);
    inspect(load);
    inspect(unload);
}

```

## Type aliases (Đặt bí danh)

Nếu bạn sử dụng type alias, bạn có thể dùng các biến thể trong enum thông qua alias (bí danh) của nó.
Điều này có lẽ sẽ hữu ích nếu tên của enum quá dài hoặc quá chung chung, và bạn
muốn đổi tên nó.

```rust,editable
enum VeryVerboseEnumOfThingsToDoWithNumbers {
    Add,
    Subtract,
}

// Tạo một type alias
type Operations = VeryVerboseEnumOfThingsToDoWithNumbers;

fn main() {
    // Chúng ta có thể dùng các biến thể của `enum` thông qua alias của nó.
    let x = Operations::Add;
}
```

Nơi mà bạn có thể thấy việc sử dụng alias thường xuyên nhất, đó là ở các khối lệnh `impl`, chúng sử dụng `Self` alias.

```rust,editable
enum VeryVerboseEnumOfThingsToDoWithNumbers {
    Add,
    Subtract,
}

impl VeryVerboseEnumOfThingsToDoWithNumbers {
    fn run(&self, x: i32, y: i32) -> i32 {
        match self {
            Self::Add => x + y,
            Self::Subtract => x - y,
        }
    }
}
```

Để học thêm về type aliases, bạn có thể đọc
[stabilization report][aliasreport] của nó.

### Xem thêm tại đây:

[`match`][match], [`fn`][fn], [`String`][str], [`ToOwned`][to_owned_trait] và ["Type alias enum variants" RFC][type_alias_rfc]

[c_struct]: https://en.wikipedia.org/wiki/Struct_(C_programming_language)
[match]: ../flow_control/match.md
[fn]: ../fn.md
[str]: ../std/str.md
[aliasreport]: https://github.com/rust-lang/rust/pull/61682/#issuecomment-502472847
[type_alias_rfc]: https://rust-lang.github.io/rfcs/2338-type-alias-enum-variants.html
[to_owned_trait]: https://doc.rust-lang.org/std/borrow/trait.ToOwned.html

