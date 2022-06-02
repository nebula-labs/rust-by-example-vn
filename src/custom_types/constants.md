# constants (Hằng số)

Rust có hai loại hằng số khác nhau có thể được khai báo trong bất kỳ phạm vi nào, 
kể cả global. Cả hai đều yêu cầu phải chú thích kiểu rõ ràng:

* `const`: Một giá trị không thể thay đổi (trường hợp thường gặp).
* `static`: Một static variable (biến tĩnh) là một biến có thời gian tồn tại là [`'static`][static] (`'static` lifetime).
  Static lifetime của nó được Rust tự suy ra và không cần phải được chỉ định.
  Một static variable chỉ có thể thay đổi giá trị được nếu đi kèm với từ khóa `mut` 
  (trở thành một static mutable variable) nhưng việc truy cập hoặc sửa đổi một static mutable variable 
  là [`unsafe`][unsafe] (không an toàn), muốn thực hiện được thì phải đưa vào trong một khối lệnh `unsafe`.

```rust,editable,ignore,mdbook-runnable
// Các hằng số có thể khai báo ở global, nằm ngoài mọi phạm vi.
static LANGUAGE: &str = "Rust";
static mut LANGUAGE_2 : &str = "Rust boy";
const THRESHOLD: i32 = 10;

fn is_big(n: i32) -> bool {
    // Truy cập tới hằng số trong phạm vi hàm is_big
    n > THRESHOLD
}

fn main() {
    let n = 16;

    // Truy cập tới hằng số trong hàm main
    println!("This is {}", LANGUAGE);
    println!("The threshold is {}", THRESHOLD);
    println!("{} is {}", n, if is_big(n) { "big" } else { "small" });
    // Lỗi! Không thể sửa một hằng số.
    THRESHOLD = 5;
    LANGUAGE = "C";
    // FIXME ^ Hãy comment dòng các trên lại.

    // Lỗi! Truy cập hoặc sửa đổi một static mutable variable là không an toàn
    println!("This static mutable variable is {}", LANGUAGE_2);
    LANGUAGE_2 = "C++";
    // FIXME ^ Hãy đưa hai dòng trên vào trong một `unsafe` block.
}
```

### Xem thêm tại đây:

[The `const`/`static` RFC](
https://github.com/rust-lang/rfcs/blob/master/text/0246-const-vs-static.md),
[`'static` lifetime][static]

[static]: ../scope/lifetime/static_lifetime.md
[unsafe]: ../unsafe.md
