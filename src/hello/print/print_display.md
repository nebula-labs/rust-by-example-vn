# Display

`fmt::Debug` trông không thân thiện và gọn gàng, và nó ít có lợi khi muốn
tùy chỉnh giao diện đầu ra. Để làm được điều này, bạn cần triển khai thủ công
[`fmt::Display`][fmt], với `{}`. Thực hiện nó như sau:
```rust
// Import (bằng `use`) `fmt` module để sử dụng các thành phần nó.
use std::fmt;

// Định nghĩa cấu trúc mà ta sẽ triển khai `fmt::Display`. Nó là một tuple struct
// có tên là `Structure` chứa một số nguyên `i32`
struct Structure(i32);

// Để có thể sử dụng `{}` để in dữ liệu, trait `fmt::Display` phải được triển khai thủ công 
// cho kiểu dữ liệu đó
impl fmt::Display for Structure {
    // Trait này yêu cầu `fmt` với chữ kí hàm chính xác.
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        // Ghi phần tử đầu tiên vào output stream (luồng đầu ra) `f` được cung cấp.
        // Nó trả về `fmt::Result` để cho biết thao tác là thành công hay thất bại.
        // Lưu ý rằng `write!` có cú pháp rất giống với `println!`.
        write!(f, "{}", self.0)
    }
}
```

`fmt::Display` trông gọn gàng hơn `fmt::Debug`, nhưng điều này gây ra một 
vấn đề cho thư viện `std`. Đó là các ambiguous types (kiểu chưa được xác định rõ ràng) 
sẽ được hiển thị như thế nào? Ví dụ, nếu thư viện `std` triển khai một style hiển thị 
duy nhất cho tất cả kiểu `Vec<T>`, thì đầu ra là gì? Nó có thể là một trong hai dạng dưới đây không?

* `Vec<path>`: `/:/etc:/home/username:/bin` (phân cách bởi `:`)
* `Vec<number>`: `1,2,3` (phân cách bởi `,`)

Không, bởi vì không có style lý tưởng cho tất cả các kiểu và thư viện `std`
không thể giả định để chọn một cái cụ thể. Do đó `fmt::Display` sẽ không được triển khai cho`Vec<T>`
hoặc cho bất kỳ generic containers nào khác. Khi đó, `fmt::Debug` phải được sử dụng 
cho các trường hợp chung này.

Mặc dù vậy, đây không phải là vấn đề lớn bởi vì đối với bất kỳ kiểu *container* mới nào mà
*không phải* generic, thì `fmt::Display` vẫn có thể được triển khai.

```rust,editable
use std::fmt; // Import `fmt`

// Một cấu trúc chứa hai số. Ở đây, `Debug` sẽ được derived để 
// đối chiếu kết quả với `Display`.
#[derive(Debug)]
struct MinMax(i64, i64);

// Triển khai `Display` cho `MinMax`.
impl fmt::Display for MinMax {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        // Sử dụng `self.number` để trỏ tới vị trí của mỗi điểm dữ liệu thành phần của struct.
        write!(f, "({}, {})", self.0, self.1)
    }
}

// Định nghĩa một cấu trúc mà ở đó các trường được đặt tên dùng cho việc so sánh với MinMax.
#[derive(Debug)]
struct Point2D {
    x: f64,
    y: f64,
}

// Tương tự, triển khai `Display` cho `Point2D`.
impl fmt::Display for Point2D {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        // Tùy chỉnh để trỏ tới x, y thành phần
        write!(f, "x: {}, y: {}", self.x, self.y)
    }
}

fn main() {
    let minmax = MinMax(0, 14);

    println!("Compare structures:");
    println!("Display: {}", minmax);
    println!("Debug: {:?}", minmax);

    let big_range =   MinMax(-300, 300);
    let small_range = MinMax(-3, 3);

    println!("The big range is {big} and the small is {small}",
             small = small_range,
             big = big_range);

    let point = Point2D { x: 3.3, y: 7.2 };

    println!("Compare points:");
    println!("Display: {}", point);
    println!("Debug: {:?}", point);

    // Câu lệnh dưới này khi chạy sẽ bị lỗi. Mặc dù cả `Debug` và `Display`
    // đều được triển khai, tuy nhiên `{:b}` lại yêu cầu triển khai `fmt::Binary`.
    // println!("What does Point2D look like in binary: {:b}?", point);
}
```

So, `fmt::Display` has been implemented but `fmt::Binary` has not, and
therefore cannot be used. `std::fmt` has many such [`traits`][traits] and
each requires its own implementation. This is detailed further in
[`std::fmt`][fmt].

Dù `fmt::Display` đã được triển khai nhưng` fmt::Binary` thì chưa, và
do đó không thể sử dụng `{:b}`. `std::fmt` còn có nhiều [`traits`][traits] và
mỗi `traits` đều có yêu cầu triển khai riêng của mình. Điều này được trình bày 
chi tiết hơn trong [`std::fmt`][fmt].

### Thực hành

Sau khi kiểm tra output của ví dụ trên, hãy sử dụng cấu trúc `Point2D` làm mẫu 
để tạo thêm cấu trúc `Complex` cho ví dụ. Khi in dữ liệu theo cùng một cách, 
đầu ra phải là:

```txt
Display: 3.3 + 7.2i
Debug: Complex { real: 3.3, imag: 7.2 }
```

### Xem thêm tại đây:

[`derive`][derive], [`std::fmt`][fmt], [`macros`][macros], [`struct`][structs],
[`trait`][traits], và [`use`][use]

[derive]: ../../trait/derive.md
[fmt]: https://doc.rust-lang.org/std/fmt/
[macros]: ../../macros.md
[structs]: ../../custom_types/structs.md
[traits]: https://doc.rust-lang.org/std/fmt/#formatting-traits
[use]: ../../mod/use.md

