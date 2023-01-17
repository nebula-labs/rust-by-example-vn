# Binding

Việc truy cập gián tiếp vào một biến khiến nó không thể phân nhánh và sử dụng cho đến khi được gán lại (re-binding). `match` sử dụng ký tự `@` để gán các giá trị với các name:

```rust,editable
// Hàm `age` có giá trị trả về là một số `u32`.
fn age() -> u32 {
    15
}

fn main() {
    println!("Tell me what type of person you are");

    match age() {
        0             => println!("I haven't celebrated my first birthday yet"),
        // Có thể `match` trực tiếp n với 1 ..= 12 nhưng như vậy thì tuổi 
        // của đứa trẻ sẽ là bao nhiêu? Thay vào đó, gán n với dãy số 1 ..= 12 bằng @.
        n @ 1  ..= 12 => println!("I'm a child of age {:?}", n),
        n @ 13 ..= 19 => println!("I'm a teen of age {:?}", n),
        // Không có binding, đơn giản là trả về kết quả
        n             => println!("I'm an old person of age {:?}", n),
    }
}
```

Bạn cũng có thể sử dụng binding để "destructure" các biến thể của `enum`, chẳng hạn như `Option`:

```rust,editable
fn some_number() -> Option<u32> {
    Some(42)
}

fn main() {
    match some_number() {
        // Đây là `Some` variant, match nếu như giá trị của nó, mà gán với `n`,
        // bằng 42.
        Some(n @ 42) => println!("The Answer: {}!", n),
        // Match một số khác bất kỳ.
        Some(n)      => println!("Not interesting... {}", n),
        // Match với bất cứ thứ gì khác (`None` variant).
        _            => (),
    }
}
```

### Xem thêm tại đây:
[`functions`][functions], [`enums`][enums] và [`Option`][option]

[functions]: ../../fn.md
[enums]: ../../custom_types/enum.md
[option]: ../../std/option.md
