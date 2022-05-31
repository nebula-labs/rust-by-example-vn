# Tuples

Một tuple là một tập các phần tử có thứ tự mà chúng có thể có kiểu dữ liệu khác nhau. Tuples được tạo bằng cách
sử dụng cặp dấu ngoặc đơn `()` bên ngoài, và tất cả những gì nằm trong đó là những phần tử của tuple. 
Bản thân mỗi tuple là một giá trị có type signature (chữ ký kiểu dữ liệu) là `(T1, T2, ...)`, 
trong đó `T1`,` T2` là các kiểu dữ liệu của các phần tử trong tuple. Các hàm có thể
sử dụng tuples cho mục đích trả về nhiều giá trị, vì các tuple có thể chứa một số lượng phần tử bất kỳ.

```rust,editable
// Tuples có thể được sử dụng như đối số của hàm cũng như có thể dùng làm giá trị trả về.
fn reverse(pair: (i32, bool)) -> (bool, i32) {
    // từ khóa `let` có thể được dùng để gán, liên kết (binding) các phần tử của tuple với các biến
    let (integer, boolean) = pair;

    (boolean, integer)
}

// Struct dưới đây được dùng cho mục bài tập thực hành bên dưới
#[derive(Debug)]
struct Matrix(f32, f32, f32, f32);

fn main() {
    // Đây là một tuple với nhiều giá trị có kiểu dữ liệu khác nhau.
    let long_tuple = (1u8, 2u16, 3u32, 4u64,
                      -1i8, -2i16, -3i32, -4i64,
                      0.1f32, 0.2f64,
                      'a', true);

    // Các giá trị có thể được lấy ra từ tuple thông qua chỉ mục.
    println!("long tuple first value: {}", long_tuple.0);
    println!("long tuple second value: {}", long_tuple.1);

    // Một tuple có thể là phần tử của một tuple cha.
    let tuple_of_tuples = ((1u8, 2u16, 2u32), (4u64, -1i8), -2i16);

    // Tuples có thể in được dữ liệu.
    println!("tuple of tuples: {:?}", tuple_of_tuples);
    
    // Nhưng các tuple dài (có nhiều hơn 12 phần tử) lại không thể in ra dữ liệu
    // let too_long_tuple = (1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13);
    // println!("too long tuple: {:?}", too_long_tuple);
    // TODO ^ Thử xóa 2 dòng comment trên để xem trình biên dịch báo lỗi gì.

    let pair = (1, true);
    println!("pair is {:?}", pair);

    println!("the reversed pair is {:?}", reverse(pair));

    // Để tạo một tuple chỉ có một phần tử, bắt buộc cần phải có dấu phẩy ở cuối
    // để phân biệt tuple đó với một literal nằm trong một cặp dấu ngoặc đơn.
    println!("one element tuple: {:?}", (5u32,));
    println!("just an integer: {:?}", (5u32));

    // Có thể phá cấu trúc (destructured) của tuple thành các phần tử bên trong nó bằng cách 
    // dùng binding (đã nói ở trên).
    let tuple = (1, "hello", 4.5, true);

    let (a, b, c, d) = tuple;
    println!("{:?}, {:?}, {:?}, {:?}", a, b, c, d);

    let matrix = Matrix(1.1, 1.2, 2.1, 2.2);
    println!("{:?}", matrix);

}
```

### Thực hành

 1. Hãy triển khai `fmt::Display` trait cho `Matrix` struct ở ví dụ trên,
    từ đó bạn có thể đổi việc in dữ liệu từ định dạng debug `{:?}` sang định dạng
    display `{}`, output mong đợi sẽ như sau:

    ```text
    ( 1.1 1.2 )
    ( 2.1 2.2 )
    ```

    Bạn có thể sẽ cần mở lại ví dụ về [print display][print_display] ở đây.
 2. Thêm một hàm `transpose` (chuyển vị) sử dụng hàm `reverse` đã viết ở trên làm mẫu, hàm
    này sẽ chấp nhận một matrix làm đối số, và trả về một matrix mà so với matrix đầu vào,
    các hàng được thay thế bằng các cột và ngược lại. Ví dụ như sau:

    ```rust,ignore
    println!("Matrix:\n{}", matrix);
    println!("Transpose:\n{}", transpose(matrix));
    ```

    kết quả mong đợi trên output như sau:

    ```text
    Matrix:
    ( 1.1 1.2 )
    ( 2.1 2.2 )
    Transpose:
    ( 1.1 2.1 )
    ( 1.2 2.2 )
    ```

[print_display]: ../hello/print/print_display.md

