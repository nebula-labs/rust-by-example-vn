# loop

Rust cung cấp từ khóa `loop` để biểu thị một vòng lặp vô hạn.

Câu lệnh `break` có thể được sử dụng để thoát khỏi một vòng lặp bất cứ lúc nào, trong khi 
câu lệnh `continue` có thể được sử dụng để bỏ qua phần còn lại của lần lặp và bắt đầu lần lặp tiếp theo.

```rust,editable
fn main() {
    let mut count = 0u32;

    println!("Let's count until infinity!");

    // Infinite loop
    loop {
        count += 1;

        if count == 3 {
            println!("three");

            // Skip the rest of this iteration
            continue;
        }

        println!("{}", count);

        if count == 5 {
            println!("OK, that's enough");

            // Exit this loop
            break;
        }
    }
}
```