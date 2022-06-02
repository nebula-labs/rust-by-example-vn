# Scope (Phạm vi) và Shadowing

Các ràng buộc biến đều có một phạm vi dùng để giới hạn sự tồn tại của nó trong một *block* (khối lệnh). Một
block là một tập hợp các câu lệnh được bao bởi dấu ngoặc nhọn `{}`.
```rust,editable,ignore,mdbook-runnable
fn main() {
    // Ràng buộc này tồn tại trong hàm main
    let long_lived_binding = 1;

    // Đây là một block code, có scope nhỏ hơn so với hàm main
    {
        // Ràng buộc này chỉ tồn tại trong block này
        let short_lived_binding = 2;

        println!("inner short: {}", short_lived_binding);
    }
    // Kết thúc của block

    // Lỗi! `short_lived_binding` không tồn tại trong phạm vi này
    println!("outer short: {}", short_lived_binding);
    // FIXME ^ Comment dòng lệnh trên

    println!("outer long: {}", long_lived_binding);
}
```
[Variable shadowing][variable-shadow] (ẩn biến) cũng được cho phép trong Rust. Trong lập trình nói chung, 
variable shadowing xảy ra khi một biến được khai báo trong một phạm vi nhất định (block, method 
hoặc inner class) có cùng tên với một biến được khai báo trong phạm vi bên ngoài hoặc cùng phạm vi nhưng ở trước. 
```rust,editable,ignore,mdbook-runnable
fn main() {
    // Đây là một ràng buộc biến
    let shadowed_binding = 1;

    {
        println!("before being shadowed: {}", shadowed_binding);

        // Ràng buộc này *shadows* shadowed_binding bên ngoài
        let shadowed_binding = "abc";

        println!("shadowed in inner block: {}", shadowed_binding);
    }
    println!("outside inner block: {}", shadowed_binding);

    // Ràng buộc này *shadows* ràng buộc trước (let shadowed_binding = 1;)
    let shadowed_binding = 2;
    println!("shadowed in outer block: {}", shadowed_binding);
}
```
[variable-shadow]: https://en.wikipedia.org/wiki/Variable_shadowing

