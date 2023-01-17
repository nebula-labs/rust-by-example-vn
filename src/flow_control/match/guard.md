# Guards

`match` *guard* có thể được thêm vào với mục đích làm filter cho nhánh.

```rust,editable
enum Temperature {
    Celsius(i32),
    Fahrenheit(i32),
}

fn main() {
    let temperature = Temperature::Celsius(35);
    // ^ TODO Hãy thử với giá trị khác của `temperature`

    match temperature {
        Temperature::Celsius(t) if t > 30 => println!("{}C is above 30 Celsius", t),
        // Phần `if condition` ^ bên trên là một guard
        Temperature::Celsius(t) => println!("{}C is below 30 Celsius", t),

        Temperature::Fahrenheit(t) if t > 86 => println!("{}F is above 86 Fahrenheit", t),
        Temperature::Fahrenheit(t) => println!("{}F is below 86 Fahrenheit", t),
    }
}
```

Lưu ý rằng trình biên dịch sẽ không tính đến các guard conditions khi kiểm tra xem 
tất cả các pattern có được bao phủ bởi biểu thức match hay không.

```rust,editable,ignore,mdbook-runnable
fn main() {
    let number: u8 = 4;

    match number {
        i if i == 0 => println!("Zero"),
        i if i > 0 => println!("Greater than zero"),
        // _ => unreachable!("Should never happen."),
        // TODO ^ bỏ comment dòng trên để sửa lỗi biên dịch
    }
}
```

### Xem thêm tại đây:

[Tuples](../../primitives/tuples.md)
[Enums](../../custom_types/enum.md)
