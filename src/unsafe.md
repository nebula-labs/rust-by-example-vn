# Những hoạt động không an toàn

Có thể coi phần này như phần giới thiệu cho phần [tài liệu chính thức][unsafe], "hãy cố gắng giảm thiểu số lượng mã không an toàn trong khi viết mã." Với ý nghĩ đó, chúng ta hãy bắt đầu! Các chú thích không an toàn trong Rust được sử dụng để bỏ qua các biện pháp bảo vệ do trình biên dịch đưa ra; cụ thể, có bốn điều chính về code không an toàn:

* dereferencing con trỏ thô
* gọi hàm hoặc phương thức `không an toàn` (bao gồm gọi hàm qua FFI, xem [chương trước](std_misc/ffi.md) của sách)
* truy cập hoặc sửa đổi các biến tĩnh có thể thay đổi
* thực hiện các đặc điểm không an toàn

### Con trỏ thô
Con trỏ thô `*` và tham chiếu `&T` hoạt động tương tự nhau, nhưng các tham chiếu luôn an toàn vì chúng được đảm bảo trỏ đến dữ liệu hợp lệ nhờ trình kiểm tra mượn. Chỉ có thể thực hiện một con trỏ thô bên trong một block không an toàn.

```rust,editable
fn main() {
    let raw_p: *const u32 = &10;

    unsafe {
        assert!(*raw_p == 10);
    }
}
```

### Gọi các hàm không an toàn
Một số hàm có thể được khai báo là `không an toàn`, nghĩa là lập trình viên có trách nhiệm đảm bảo tính chính xác thay vì của trình biên dịch. Một ví dụ về điều này là [`std::slice::from_raw_parts`] sẽ tạo một lát cắt được cung cấp một con trỏ tới phần tử đầu tiên và độ dài của phần tử đó.

```rust,editable
use std::slice;

fn main() {
    let some_vector = vec![1, 2, 3, 4];

    let pointer = some_vector.as_ptr();
    let length = some_vector.len();

    unsafe {
        let my_slice: &[u32] = slice::from_raw_parts(pointer, length);

        assert_eq!(some_vector.as_slice(), my_slice);
    }
}
```

Đối với `slice::from_raw_parts`, một trong những giả định *phải* được duy trì là con trỏ được chuyển trong các điểm tới bộ nhớ hợp lệ và bộ nhớ được trỏ tới là chính xác. Nếu những bất biến này không được duy trì thì hành vi của chương trình sẽ không được xác định và không biết điều gì sẽ xảy ra.


[unsafe]: https://doc.rust-lang.org/book/ch19-01-unsafe-rust.html
[`std::slice::from_raw_parts`]: https://doc.rust-lang.org/std/slice/fn.from_raw_parts.html
