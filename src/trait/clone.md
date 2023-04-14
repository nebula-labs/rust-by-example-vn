# Clone

Khi xử lý tài nguyên, hành vi mặc định trong  chuyển chúng trong quá trình gán hoặc gọi hàm. Tuy nhiên, đôi khi chúng ta cũng cần phải tạo một bản sao của tài nguyên.

Trait [`Clone`][clone] giúp chúng ta thực hiện được điều này. Thông thường, chúng ta có thể sử dụng phương thức `.clone()` được xác định bởi trait `Clone`.

```rust,editable
// Một cấu trúc Unit không có tài nguyên
#[derive(Debug, Clone, Copy)]
struct Unit;

// Cấu trúc tuple với các tài nguyên triển khai trait `Clone`
#[derive(Clone, Debug)]
struct Pair(Box<i32>, Box<i32>);

fn main() {
    // Khởi tạo `Unit`
    let unit = Unit;
    // Sao chép `Đơn vị`, không có tài nguyên nào để di chuyển
    let copied_unit = unit;

    // Cả hai `Unit` đều có thể được sử dụng độc lập
    println!("original: {:?}", unit);
    println!("copy: {:?}", copied_unit);

    // Khởi tạo `Pair`
    let pair = Pair(Box::new(1), Box::new(2));
    println!("original: {:?}", pair);

    // Chuyển `pair` thành `moved_pair`, di chuyển tài nguyên
    let moved_pair = pair;
    println!("moved: {:?}", moved_pair);

    // Lỗi! `pair` đã mất dữ liệu
    //println!("original: {:?}", pair);
    // TODO ^ Hãy thử chạy dòng lệnh trên

    // Sao chép `moved_pair` vào `cloned_pair` (bao gồm tài nguyên)
    let cloned_pair = moved_pair.clone();
    // Xóa cặp ban đầu bằng cách sử dụng std::mem::drop
    drop(moved_pair);

    // Lỗi! `moved_pair` đã bị loại bỏ
    //println!("copy: {:?}", moved_pair);
    // TODO ^ Hãy thử chạy dòng lệnh trên

    // Kết quả từ .clone() vẫn có thể được sử dụng!
    println!("clone: {:?}", cloned_pair);
}
```

[clone]: https://doc.rust-lang.org/std/clone/trait.Clone.html
