# Arrays và Slices

Array (Mảng) là một tập hợp các đối tượng có cùng kiểu dữ liệu `T`, được lưu trữ liền nhau 
trong bộ nhớ. Mảng được tạo bằng cách sử dụng dấu ngoặc vuông `[]` và độ dài của chúng, 
được xác định tại thời điểm biên dịch, mảng có type signature là `[T; length]`.

Slices (Lát cắt dữ liệu) cũng tương tự như mảng, nhưng độ dài của chúng không được xác định 
tại thời điểm biên dịch. Thay vào đó, một slice là một two-word object, từ đầu tiên là một con trỏ trỏ đến dữ liệu,
và từ thứ hai là chiều dài của slice. Kích thước của từ tương tự như usize, được xác định bởi kiến trúc của bộ 
xử lý, ví dụ 64 bit trên x86-64. Slices được sử dụng để [borrow][borrow_ownership] (mượn quyền sử dụng) một phần của mảng và có type signature là `&[T]`.

```rust,editable,ignore,mdbook-runnable
use std::mem;

// Hàm này borrow một slice
fn analyze_slice(slice: &[i32]) {
    println!("first element of the slice: {}", slice[0]);
    println!("the slice has {} elements", slice.len());
}

fn main() {
    // Mảng có kích thước cố định (ở đây có chú thích type signature của mảng, 
    // nhưng việc chú thích này là không bắt buộc)
    let xs: [i32; 5] = [1, 2, 3, 4, 5];

    // Tất cả các phần tử trong mảng có thể được khởi tạo với cùng một giá trị.
    let ys: [i32; 500] = [0; 500];

    // Chỉ mục các phần tử của mảng bắt đầu từ 0
    println!("first element of the array: {}", xs[0]);
    println!("second element of the array: {}", xs[1]);

    // `len` trả về số lượng phần tử có trong mảng
    println!("number of elements in array: {}", xs.len());

    // Mảng được cấp phát vùng nhớ trên bộ nhớ stack
    println!("array occupies {} bytes", mem::size_of_val(&xs));

    // Mảng có thể được tự động borrow dưới dạng slices
    println!("borrow the whole array as a slice");
    analyze_slice(&xs);

    // Slices có thể trỏ tới một phần của một mảng
    // dưới dạng [starting_index..ending_index]
    // starting_index là vị trí đầu tiên của slice
    // ending_index là vị trí cuối cùng của slice cộng thêm 1
    println!("borrow a section of the array as a slice");
    analyze_slice(&ys[1 .. 4]);

    // Ví dụ về slice rỗng `&[]`
    let empty_array: [u32; 0] = [];
    assert_eq!(&empty_array, &[]);
    assert_eq!(&empty_array, &[][..]); // giống như trên, nhưng dài dòng hơn

    // Truy cập tới chỉ mục ngoài giới hạn của mảng sẽ gây ra 
    // lỗi biên dịch.
    //println!("{}", xs[5]);
}
```
Xem thêm tại đây:

[Borrowing][borrow_ownership]

[borrow_ownership]: ../scope/borrow.md
