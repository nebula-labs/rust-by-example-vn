# Structures (Cấu trúc)

Có ba kiểu cấu trúc mà có thể tạo được qua từ khóa `struct`:

* Tuple structs, về cơ bản là các tuple được đặt tên.
* Structs truyền thống [như trong ngôn ngữ C][c_struct].
* Unit structs, không có các trường dữ liệu, hữu ích cho mục đích làm generics.

```rust,editable
// Đây là một attribute dùng để ẩn cảnh báo về những đoạn code không sử dụng.
#![allow(dead_code)]

#[derive(Debug)]
struct Person {
    name: String,
    age: u8,
}

// Một unit struct
struct Unit;

// Một tuple struct
struct Pair(i32, f32);

// Một struct truyền thống với hai trường
struct Point {
    x: f32,
    y: f32,
}

// Các struct có thể được tái sử dụng để làm trường dữ liệu cho struct khác.
struct Rectangle {
    // Một hình chữ nhật có thể được xác định bằng vị trí top left (điểm trên cùng bên trái) 
    // và bottom right (điểm dưới cùng bên phải) của nó trong không gian.
    top_left: Point,
    bottom_right: Point,
}

fn main() {
    // Tạo một `Person` struct
    let name = String::from("Peter");
    let age = 27;
    let peter = Person { name, age };

    // In struct bằng chế độ debug
    println!("{:?}", peter);

    // Khởi tạo một `Point`
    let point: Point = Point { x: 10.3, y: 0.4 };

    // Truy cập các trường của điểm trên
    println!("point coordinates: ({}, {})", point.x, point.y);

    // Tạo một điểm mới bằng cú pháp cập nhật struct để lấy dữ liệu các trường còn lại từ một struct khác.
    let bottom_right = Point { x: 5.2, ..point };

    // `bottom_right.y` sẽ giống như `point.y` bởi vì thông qua cú pháp cập nhật struct, chúng ta đã
    // lấy trường này từ point.
    println!("second point: ({}, {})", bottom_right.x, bottom_right.y);

    // Destructure point bằng `let` binding
    let Point { x: left_edge, y: top_edge } = point;

    let _rectangle = Rectangle {
        // Việc tạo một struct cũng là một biểu thức
        top_left: Point { x: left_edge, y: top_edge },
        bottom_right: bottom_right,
    };

    // Tạo một unit struct
    let _unit = Unit;

    // Tạo một tuple struct
    let pair = Pair(1, 0.1);

    // Truy cập các trường của một tuple struct
    println!("pair contains {:?} and {:?}", pair.0, pair.1);

    // Destructure một tuple struct
    let Pair(integer, decimal) = pair;

    println!("pair contains {:?} and {:?}", integer, decimal);
}
```

### Thực hành

1. Viết hàm `rect_area` để tính diện tích của một `Rectangle` (thử sử dụng 
   nested destructuring).
2. Viết một hàm `square` lấy một `Point` và một số thực `f32` làm đối số, trả về
   một `Rectangle` có top left là điểm đầu vào, còn chiều rộng và chiều dài bằng nhau
   và bằng số thực đầu vào.

### See also

[`attributes`][attributes], và [destructuring][destructuring]

[attributes]: ../attribute.md
[c_struct]: https://en.wikipedia.org/wiki/Struct_(C_programming_language)
[destructuring]: ../flow_control/match/destructuring.md

