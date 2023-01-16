# tuples

Tuple có thể được tách ra các phần tử bằng một khối `match` như sau:

```rust,editable
fn main() {
    let triple = (0, -2, 3);
    // TODO ^ Thử với giá trị khác của triple

    println!("Tell me about {:?}", triple);
    // Match có thể được sử dụng để phân tách một tuple
    match triple {
        // Tách phần tử thứ 2 và thứ 3
        (0, y, z) => println!("First is `0`, `y` is {:?}, and `z` is {:?}", y, z),
        (1, ..)  => println!("First is `1` and the rest doesn't matter"),
        (.., 2)  => println!("last is `2` and the rest doesn't matter"),
        (3, .., 4)  => println!("First is `3`, last is `4`, and the rest doesn't matter"),
        // `..` có thể được sử dụng để bỏ qua phần còn lại của tuple
        _      => println!("It doesn't matter what they are"),
        // `_` có nghĩa là không gán trị cho một biến
    }
}
```

### Xem thêm tại đây:

[Tuples](../../../primitives/tuples.md)
