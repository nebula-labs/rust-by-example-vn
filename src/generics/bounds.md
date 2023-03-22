# Bounds

Khi làm việc với generics, các tham số thường phải sử dụng các traits như *bounds*
để chỉ định chức năng mà một kiểu triển khai. Ví dụ sau sử dụng trait `Display`
để in và do đó nó yêu cầu `T` được ràng buộc bởi `Display`; nói cách khác, `T` *phải* triển khai `Display`.

```rust,ignore
// Định nghĩa hàm `printer` nhận kiểu `T` generic,
// kiểu `T` phải triển khai trait `Display`.
fn printer<T: Display>(t: T) {
    println!("{}", t);
}
```

Bounding hạn chế generic thành các kiểu tuân thủ với bounds. Nghĩa là:

```rust,ignore
struct S<T: Display>(T);

// Lỗi! `Vec<T>` không triển khai `Display`.
let s = S(vec![1]);
```

Một công dụng khác của bounding là các generic được phép sử dụng [methods]
của các traits được chỉ định trong bounds. Ví dụ:

```rust,editable
// Một trait triển khai marker in: `{:?}`.
use std::fmt::Debug;

trait HasArea {
    fn area(&self) -> f64;
}

impl HasArea for Rectangle {
    fn area(&self) -> f64 { self.length * self.height }
}

#[derive(Debug)]
struct Rectangle { length: f64, height: f64 }
#[allow(dead_code)]
struct Triangle  { length: f64, height: f64 }

// Generic `T` phải triển khai `Debug`.
// Bất kể là loại nào, nó vẫn sẽ hoạt động đúng cách.
fn print_debug<T: Debug>(t: &T) {
    println!("{:?}", t);
}

// `T` phải triển khai `HasArea`. Bất kỳ kiểu nào đáp ứng
// bound này đều có thể sử dụng hàm `area` của `HasArea`.
fn area<T: HasArea>(t: &T) -> f64 { t.area() }

fn main() {
    let rectangle = Rectangle { length: 3.0, height: 4.0 };
    let _triangle = Triangle  { length: 3.0, height: 4.0 };

    print_debug(&rectangle);
    println!("Area: {}", area(&rectangle));

    //print_debug(&_triangle);
    //println!("Area: {}", area(&_triangle));
    // ^ TODO: Hãy thử bỏ dấu chú thích cho phần này.
    // | Lỗi: Không triển khai `Debug` hoặc `HasArea`.
}
```

Ngoài ra, [`where`][where] cũng có thể được sử dụng để áp dụng bounds
trong một số trường hợp để biểu đạt rõ ràng hơn.

### Xem thêm:

[`std::fmt`][fmt], [`struct`s][structs], và [`trait`s][traits]

[fmt]: ../hello/print.md
[methods]: ../fn/methods.md
[structs]: ../custom_types/structs.md
[traits]: ../trait.md
[where]: ../generics/where.md