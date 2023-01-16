# structs

Tương tự với các kiểu đã nói, một `struct` cũng có thể được phân tách bằng `match`:

```rust,editable
fn main() {
    struct Foo {
        x: (u32, u32),
        y: u32,
    }

    // Hãy thử thay đổi giá trị trong struct này và xem điều gì xảy ra
    let foo = Foo { x: (1, 2), y: 3 };

    match foo {
        Foo { x: (1, b), y } => println!("First of x is 1, b = {},  y = {} ", b, y),

        // Bạn có thể phân tách một cấu trúc và đổi tên biến,
        // thứ tự là không quan trọng
        Foo { y: 2, x: i } => println!("y is 2, i = {:?}", i),

        // và bạn cũng có thể bỏ qua một vài biến:
        Foo { y, .. } => println!("y = {}, we don't care about x", y),
        // điều này sẽ phát sinh lỗi: pattern không đề cập đến trường `x`
        //Foo { y } => println!("y = {}", y),
    }
}
```

### Xem thêm tại đây:

[Structs](../../../custom_types/structs.md)
