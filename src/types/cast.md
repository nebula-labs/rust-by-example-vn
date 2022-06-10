# Casting (Ép kiểu)

Rust không cung cấp ép kiểu ngầm định (implicit) giữa các kiểu dữ liệu nguyên thủy.
Tuy nhiên, ép kiểu tường minh (explicit) có thể được thực hiện bằng cách sử dụng từ khóa `as`.

Các quy tắc chuyển đổi giữa các kiểu dữ liệu thường tuân theo các quy ước trong C,
trừ các trường hợp có hành vi không xác định. Hành vi của tất cả các phép ép kiểu
giữa các kiểu dữ liệu trong Rust phải được xác định rõ ràng.

```rust,editable,ignore,mdbook-runnable
// Loại bỏ tất cả các cảnh báo từ các phép ép kiểu bị tràn overflow.
#![allow(overflowing_literals)]

fn main() {
    let decimal = 65.4321_f32;

    // Lỗi! Vì không có ép kiểu ngầm định
    let integer: u8 = decimal;
    // FIXME ^ Comment dòng này lại

    // Ép kiểu tường minh
    let integer = decimal as u8;
    let character = integer as char;

    // Lỗi! Có những giới hạn trong các quy tắc chuyển đổi.
    // Một số thực float không thể được chuyển đổi trực tiếp thành một ký tự char.
    let character = decimal as char;
    // FIXME ^ Comment dòng này lại

    println!("Casting: {} -> {} -> {}", decimal, integer, character);

    // khi truyền bất kỳ giá trị nào sang kiểu số không âm, T,
    // T::MAX + 1 sẽ được dùng để cộng hoặc trừ cho đến khi giá trị
    // phù hợp với kiểu mới

    // 1000 trở thành một số u16
    println!("1000 as a u16 is: {}", 1000 as u16);

    // 1000 - 256 - 256 - 256 = 232
    // Phép ép kiểu này hoạt động như sau, 8 bit thấp (bit ở cực phải - LSB) của 1000 được giữ lại,
    // còn các bit cao (bit ở cực trái - MSB) sẽ bị cắt bớt.
    println!("1000 as a u8 is : {}", 1000 as u8);
    // -1 + 256 = 255
    println!("  -1 as a u8 is : {}", (-1i8) as u8);

    // Với số nguyên dương thì phép ép kiểu trên cũng giống như phép chia lấy dư
    println!("1000 mod 256 is : {}", 1000 % 256);

    // Khi ép kiểu sang kiểu số nguyên có dấu, kết quả (theo bit) cũng giống 
    // như ép kiểu sang kiểu số nguyên không dấu tương ứng. Nếu bit cao nhất của 
    // kết quả là 1, thì giá trị là âm. Ngược lại, nếu bằng 0, thì giá trị là dương.

    // Tất nhiên là nếu số đó nằm trong khoảng giá trị của kiểu dữ liệu số cần ép,
    // thì không có gì thay đổi.
    println!(" 128 as a i16 is: {}", 128 as i16);
    
    // 128 as u8 -> -128
    println!(" 128 as a i8 is : {}", 128 as i8);

    // 1000 as u8 -> 232
    println!("1000 as a u8 is : {}", 1000 as u8);
    // 232 as i8 -> -24
    println!(" 232 as a i8 is : {}", 232 as i8);

    // Kể từ phiên bản Rust 1.45, từ khóa `as` thực hiện một *saturating cast*
    // khi ép kiểu từ float sang int. Tức là nếu giá trị cuả số thực vượt quá
    // giới hạn trên hoặc nhỏ hơn giới hạn dưới, giá trị trả về
    // sẽ bằng với giới hạn biên của kiểu dữ liệu.
    
    // 300.0 -> 255
    println!("300.0 is {}", 300.0_f32 as u8);
    // -100.0 as u8 -> 0
    println!("-100.0 as u8 is {}", -100.0_f32 as u8);
    // nan as u8 -> 0
    println!("nan as u8 is {}", f32::NAN as u8);
    
    // This behavior incurs a small runtime cost and can be avoided 
    // with unsafe methods, however the results might overflow and 
    // return **unsound values**. Use these methods wisely:
    // *Saturating cast* phát sinh một khoản chi phí thời gian chạy nhỏ nhưng có thể 
    // tránh được với các unsafe method (phương thức không an toàn), tuy nhiên, kết quả 
    // có thể tràn và trả về giá trị chưa tìm thấy **unsound values**. Nên nhớ hãy sử 
    // dụng các phương pháp này một cách khôn ngoan:
    unsafe {
        // 300.0 -> 44
        println!("300.0 is {}", 300.0_f32.to_int_unchecked::<u8>());
        // -100.0 as u8 -> 156
        println!("-100.0 as u8 is {}", (-100.0_f32).to_int_unchecked::<u8>());
        // nan as u8 -> 0
        println!("nan as u8 is {}", f32::NAN.to_int_unchecked::<u8>());
    }
}
```

