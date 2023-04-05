# Box, stack và heap

Mặc định, tất cả các giá trị trong Rust đều được cấp phát trên stack. Tuy nhiên, giá trị có thể được _đóng gói_ (boxed)
(nghĩa là được cấp phát trên heap) bằng cách tạo ra một `Box<T>`. Box là một con trỏ thông minh (smart pointer) trỏ
tới một giá trị được cấp phát trên heap có kiểu dữ liệu `T`. Khi một box ra khỏi scope, destructor của nó sẽ được gọi, đối tượng
được chứa bên trong sẽ bị xoá, tài nguyên trên heap sẽ được giải phóng.

Giá trị được boxed có thể được giải tham chiếu (dereferenced) bằng toán tử `*`;
điều này giúp ta loại bớt một tầng gián đoạn (indirection)

```rust,editable
use std::mem;

#[allow(dead_code)]
#[derive(Debug, Clone, Copy)]
struct Point {
    x: f64,
    y: f64,
}

// Một hình chữ nhật có thể được xác định bởi vị trí của các đỉnh trên cùng bên trái và
// dưới cùng bên phải của nó trong không gian
#[allow(dead_code)]
struct Rectangle {
    top_left: Point,
    bottom_right: Point,
}

fn origin() -> Point {
    Point { x: 0.0, y: 0.0 }
}

fn boxed_origin() -> Box<Point> {
    // Cấp phát vùng nhớ cho điểm này trên heap, và trả về một con trỏ tới nó.
    Box::new(Point { x: 0.0, y: 0.0 })
}

fn main() {
    // (Tất cả các chú thích kiểu dữ liệu đều là dư thừa trong đoạn code này)
    // Các biến được cấp phát trên stack
    let point: Point = origin();
    let rectangle: Rectangle = Rectangle {
        top_left: origin(),
        bottom_right: Point { x: 3.0, y: -4.0 }
    };

    // Hình chữ nhật được cấp phát trên heap
    let boxed_rectangle: Box<Rectangle> = Box::new(Rectangle {
        top_left: origin(),
        bottom_right: Point { x: 3.0, y: -4.0 },
    });

    // Kết quả đầu ra của hàm cũng có thể được boxed
    let boxed_point: Box<Point> = Box::new(origin());

    // Có hai tầng gián đoạn (indirection) dưới đây
    let box_in_a_box: Box<Box<Point>> = Box::new(boxed_origin());

    println!("Point occupies {} bytes on the stack",
             mem::size_of_val(&point));
    println!("Rectangle occupies {} bytes on the stack",
             mem::size_of_val(&rectangle));

    // Kích thước của box = kích thước con trỏ
    println!("Boxed point occupies {} bytes on the stack",
             mem::size_of_val(&boxed_point));
    println!("Boxed rectangle occupies {} bytes on the stack",
             mem::size_of_val(&boxed_rectangle));
    println!("Boxed box occupies {} bytes on the stack",
             mem::size_of_val(&box_in_a_box));

    // Sao chép dữ liệu chứa trong `boxed_point` đến `unboxed_point`
    let unboxed_point: Point = *boxed_point;
    println!("Unboxed point occupies {} bytes on the stack",
             mem::size_of_val(&unboxed_point));
}
```
