# The ref pattern

Khi thực hiện matching hoặc destructuring thông qua `let`, từ khóa `ref` có thể được sử dụng lấy tham chiếu của các trường trong một biến kiểu struct/tuple. Ví dụ dưới đây là một vài tình huống có thể hữu ích:

```rust,editable
#[derive(Clone, Copy)]
struct Point { x: i32, y: i32 }

fn main() {
    let c = 'Q';

    // Từ khóa `ref` ở vế trái dòng lệnh có ý nghĩa tương tự với
    // với dấu `&` ở vế phải.
    let ref ref_c1 = c;
    let ref_c2 = &c;

    println!("ref_c1 equals ref_c2: {}", *ref_c1 == *ref_c2);

    let point = Point { x: 0, y: 0 };

    // `ref` cũng có thể được dùng trong trường hợp cần trích xuất giá trị từ biến kiểu struct 
    let _copy_of_x = {
        // `ref_to_x` là một tham chiếu đến trường `x` bên trong biến `point`.
        let Point { x: ref ref_to_x, y: _ } = point;

        // Trả về bản sao của trường `x` trong biến `point`.
        *ref_to_x
    };

    // Một bản sao có thể thay đổi giá trị của biến `point`
    let mut mutable_point = point;

    {
        // Từ khóa `ref` có thể đi cùng từ khóa `mut` để tạo ra một tham chiếu có thể thay đổi giá trị.
        let Point { x: _, y: ref mut mut_ref_to_y } = mutable_point;

        // Thay đổi trường `y` của `mutable_point` thông qua tham chiếu có thể thay đổi giá trị (vừa được tạo ở trên).
        *mut_ref_to_y = 1;
    }

    println!("point is ({}, {})", point.x, point.y);
    println!("mutable_point is ({}, {})", mutable_point.x, mutable_point.y);

    // Một biến tuple có thể thay đổi giá trị có giá trị là một Pointer (điểm)
    let mut mutable_tuple = (Box::new(5u32), 3u32);
    
    {
        // Truy xuất vào biến `mutable_tuple` để thay đổi giá trị của `last`.
        let (_, ref mut last) = mutable_tuple;
        *last = 2u32;
    }
    
    println!("tuple is {:?}", mutable_tuple);
}
```