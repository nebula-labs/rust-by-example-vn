# Hàm

Các hàm được khai báo bằng từ khóa `fn`. Những tham số của hàm phải được
chú thích kiểu dữ liệu, tương tự như các biến, và nếu hàm trả về một giá trị,
thì giá trị trả về phải được chỉ định ngay sau dấu mũi tên `->`.

Biểu thức cuối cùng trong hàm sẽ được sử dụng như là giá trị trả về.
Ngoài ra, câu lệnh `return` có thể được sử dụng để trả về một giá trị sớm hơn
từ bên trong hàm, hoặc là trong vòng lặp hoặc trong câu lệnh `if`.

Cùng thử viết lại FizzBuzz bằng hàm nhé!

```rust,editable
// Thứ tự khai báo các hàm không bị ràng buộc như trong ngôn ngữ C/C++.
fn main() {
    // Ta có thể gọi hàm ở ngay đây, rồi định nghĩa hàm ở đâu khác cũng được.
    fizzbuzz_to(100);
}

// Hàm trả về giá trị boolean
fn is_divisible_by(lhs: u32, rhs: u32) -> bool {
    // Trường hợp đặc biệt, trả về giá trị của hàm sớm hơn.
    if rhs == 0 {
        return false;
    }

    // Đây là một biểu thức, từ khóa `return` không cần phải được sử dụng ở đây
    lhs % rhs == 0
}

// Những hàm không trả về giá trị nào, sẽ trả về kiểu dữ liệu `()`
fn fizzbuzz(n: u32) -> () {
    if is_divisible_by(n, 15) {
        println!("fizzbuzz");
    } else if is_divisible_by(n, 3) {
        println!("fizz");
    } else if is_divisible_by(n, 5) {
        println!("buzz");
    } else {
        println!("{}", n);
    }
}

// Khi một hàm trả về `()`, không cần phải ghi ra kiểu dữ liệu trả về cũng được.
fn fizzbuzz_to(n: u32) {
    for n in 1..=n {
        fizzbuzz(n);
    }
}
```
