# Diverging functions

Các hàm "Diverging" (không kết thúc) không bao giờ trả về giá trị. Chúng được đánh dấu bằng `!`, đây là một kiểu dữ liệu rỗng.

```rust
fn foo() -> ! {
    panic!("This call never returns.");
}
```

Khác với tất cả các kiểu dữ liệu khác, kiểu dữ liệu này không thể được khởi tạo, 
vì tập hợp tất cả các giá trị có thể của kiểu dữ liệu này là rỗng. 
Lưu ý rằng, điều này khác với kiểu `()` (kiểu unit), mà có đúng một giá trị có thể.

Ví dụ, hàm này trả về như bình thường, mặc dù không có thông tin nào trong giá trị trả về.

```rust
fn some_fn() {
    ()
}

fn main() {
    let _a: () = some_fn();
    println!("This function returns and you can see this line.");
}
```

Khác với hàm này, hàm này sẽ không bao giờ trả lại quyền điều khiển cho người gọi.

```rust,ignore
#![feature(never_type)]

fn main() {
    let x: ! = panic!("This call never returns.");
    println!("You will never see this line!");
}
```

Mặc dù điều này có vẻ như một khái niệm trừu tượng, thực tế nó rất hữu ích và thường tiện dụng.
Ưu điểm chính của kiểu dữ liệu này là nó có thể được chuyển đổi sang bất kỳ kiểu dữ liệu nào khác 
và do đó được sử dụng ở những nơi cần đúng kiểu dữ liệu, ví dụ như trong các khối `match`.
Điều này cho phép chúng ta viết mã như thế này:


```rust
fn main() {
    fn sum_odd_numbers(up_to: u32) -> u32 {
        let mut acc = 0;
        for i in 0..up_to {
            // Lưu ý rằng kiểu trả về của biểu thức match này phải là u32
            // vì kiểu của biến "addition" là kiểu dữ liệu u32.
            let addition: u32 = match i%2 == 1 {
                // Biến "i" có kiểu dữ liệu là u32, điều này hoàn toàn hợp lệ.
                true => i,
                // Mặt khác, biểu thức "continue" không trả về giá trị kiểu u32
                // nhưng vẫn không sao cả, vì nó không bao giờ trả về và do đó
                // không vi phạm yêu cầu kiểu dữ liệu của biểu thức match.
                false => continue,
            };
            acc += addition;
        }
        acc
    }
    println!("Sum of odd numbers up to 9 (excluding): {}", sum_odd_numbers(9));
}
```

Đây cũng là kiểu trả về của các hàm lặp mãi mãi (e.g. `loop {}`) 
 như các máy chủ mạng hoặc các hàm kết thúc quá trình thực thi (e.g. `exit()`).
