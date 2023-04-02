# Ép kiểu (coercion)

Một lifetime dài hơn có thể được ép kiểu thành một lifetime ngắn hơn
để nó có thể được dùng trong phạm vi mà nó thường không thể được sử dụng.
Điều này được thực hiện thông qua quá trình ép kiểu tự suy luận bởi trình biên dịch Rust,
và cũng thông qua việc khai báo sự khác biệt về lifetime:

```rust,editable
// Ở đâu, Rust sẽ tự suy ra một lifetime ngắn nhất có thể.
// Hai tham chiếu này sau đó sẽ bị ép kiểu thành lifetime đó.
fn multiply<'a>(first: &'a i32, second: &'a i32) -> i32 {
    first * second
}

// `<'a: 'b, 'b>` được đọc như sau: lifetime `'a` tối thiểu dài ngang lifetime `'b`.
// Ở đây, chúng ta nhận vào một `&'a i32` và trả về a `&'b i32` như là một kết quả của sự ép kiểu.
fn choose_first<'a: 'b, 'b>(first: &'a i32, _: &'b i32) -> &'b i32 {
    first
}

fn main() {
    let first = 2; // Lifetime dài hơn

    {
        let second = 3; // Lifetime ngắn hơn

        println!("The product is {}", multiply(&first, &second));
        println!("{} is the first", choose_first(&first, &second));
    };
}
```
