# Literals và operators

Số nguyên `1`, số thực `1.2`, ký tự `'a'`, xâu `"abc"`, boolean `true`
and kiểu đơn vị `()` đều có thể được diễn đạt bằng literals.
Literal là gì? Nó là một giá trị thể hiện chính nó, một giá trị cố định được chèn trực tiếp
vào code mà chương trình không thể thay đổi trong quá trình thực thi.

Các số nguyên có thể được biểu diễn theo các cách khác nhau dưới dạng số thập lục phân (hexadecimal literals), 
bát phân (octal literals) hay nhị phân (binary literals), bằng cách sử dụng các tiền tố tương ứng: `0x`, `0o` or `0b`.

Dấu gạch dưới có thể được chèn vào trong các numeric literals (hiểu đơn giản là các số) 
để giúp việc biểu diễn số được rõ ràng, dễ đọc hơn, ví dụ: 
`1_000` chính là `1000`, và `0.000_001` chính là `0.000001` nhưng dễ đọc hơn.

Chúng ta cũng cần cho trình biên dịch biết kiểu literals mà ta sử dụng. Như trong đoạn code bên dưới,
ta sẽ sử dụng hậu tố `u32` để chỉ ra rằng literal ở đây là một nguyên không dấu 32-bit
và hậu tố `i32` để chỉ ra rằng đây là một số nguyên có dấu 32 bit.

Các operators (toán tử, phép toán) là có sẵn trong Rust và [mức độ ưu tiên của chúng trong Rust][rust op-prec]
là tương tự như trong [các ngôn ngữ lập trình được truyền cảm hứng từ C khác][op-prec].

```rust,editable
fn main() {
    // 100 là một numeric literals
    let i: u32 = 100; 

    // Phép cộng hai số nguyên
    println!("1 + 2 = {}", 1u32 + 2);

    // Phép trừ hai số nguyên
    println!("1 - 2 = {}", 1i32 - 2);
    // TODO ^ Thử thay đổi `1i32` sang `1u32` và quan sát lỗi
    // để xem tại sao kiểu của biến lại quan trọng.

    // Short-circuiting boolean logic (Các phép toán logic: hội, tuyển, phủ định)
    println!("true AND false is {}", true && false);
    println!("true OR false is {}", true || false);
    println!("NOT true is {}", !true);

    // Các toán tử Bitwise (Thao tác bit)
    println!("0011 AND 0101 is {:04b}", 0b0011u32 & 0b0101);
    println!("0011 OR 0101 is {:04b}", 0b0011u32 | 0b0101);
    println!("0011 XOR 0101 is {:04b}", 0b0011u32 ^ 0b0101);
    println!("1 << 5 is {}", 1u32 << 5);
    println!("0x80 >> 2 is 0x{:x}", 0x80u32 >> 2);

    // Sử dụng dấu gạch dưới để việc biểu diễn số được sáng sủa và dễ đọc hơn!
    println!("One million is written as {}", 1_000_000u32);
}
```

[rust op-prec]: https://doc.rust-lang.org/reference/expressions.html#expression-precedence
[op-prec]: https://en.wikipedia.org/wiki/Operator_precedence#Programming_languages

