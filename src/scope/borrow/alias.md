# Aliasing

Trong Rust, dữ liệu có thể mượn theo kiểu không thể thay đổi bao nhiêu lần tùy thích, nhưng với kiểu mượn dữ liệu có thể thay đổi, dữ liệu gốc không thể mượn kiểu dữ liệu không thể thay đổi. Mặc khác, chỉ cho phép mượn dữ liệu không thể thay đổi tại 1 thời điểm. Dữ liệu gốc chỉ có thể được mượn lại sau khi tham chiếu có thể thay đổi được đã được sử dụng lần cuối trong mã.

```rust,editable
struct Point { x: i32, y: i32, z: i32 }

fn main() {
    let mut point = Point { x: 0, y: 0, z: 0 };

    let borrowed_point = &point;
    let another_borrow = &point;

    // Dữ liệu có thể được truy cập thông qua các tham chiếu và chủ sở hữu gốc
    println!("Point has coordinates: ({}, {}, {})",
                borrowed_point.x, another_borrow.y, point.z);

    // Lỗi! Không thể mượn `point` theo kiểu có thể thay đổi bởi vì hiện tại nó đã được mượn theo kiểu không thể thay đổi 
    // let mutable_borrow = &mut point;
    // TODO ^ Thử uncommenting dòng phía trên

    // Các giá trị được mượn được sử dụng lại ở đây
    println!("Point has coordinates: ({}, {}, {})",
                borrowed_point.x, another_borrow.y, point.z);

    // Các tham chiếu không thể thay đổi không được sử dụng trong phần còn lại của mã nên có thể mượn lại với một tham chiếu có thể thay đổi.
    let mutable_borrow = &mut point;

    // Thay đổi dữ liệu thông qua tham chiếu có thể thay đổi
    mutable_borrow.x = 5;
    mutable_borrow.y = 2;
    mutable_borrow.z = 1;

    // Lỗi! Không thể mượn `point` theo kiểu không thể thay đổi bởi vì 
    //  hiện tại nó đã được mượn theo kiểu có thể thay đổi 
    // let y = &point.y;
    // TODO ^ Thử uncommenting dòng phía trên

    // Lỗi! Không thể in ra màn hình bởi vì `println!` yêu cầu một tham chiếu không thể thay đổi tới `point`
    // println!("Point Z coordinate is {}", point.z);
    // TODO ^ Thử uncommenting dòng phía trên

    // Ok! Tham chiếu có thể thay đổi có thể được truyền như là không thể thay đổi cho `println!`
    println!("Point has coordinates: ({}, {}, {})",
                mutable_borrow.x, mutable_borrow.y, mutable_borrow.z);

    // Tham có thể thay đổi không được sử dụng trong phần còn lại của mã 
    // nên có thể mượn lại được
    let new_borrowed_point = &point;
    println!("Point now has coordinates: ({}, {}, {})",
             new_borrowed_point.x, new_borrowed_point.y, new_borrowed_point.z);
}
```