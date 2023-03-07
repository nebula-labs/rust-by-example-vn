# Multiple error types

Các ví dụ trước luôn rất thuận tiện; những `Result` tương tác với
những `Result`, trong khi những `Option` cũng tương tác với những `Option`.

Đôi khi `Option` lại cần tương tác với `Result`, hoặc
`Result<T, Error1>` cần tương tác với `Result<T, Error2>`. Trong những trường hợp đó,
ta muốn quản lý các loại lỗi khác nhau sao cho những lỗi đó có thể kết hợp được với nhau và dễ dàng tương tác.


Trong đoạn code sau, 2 trường hợp gọi `unwrap` trả về 2 loại lỗi khác nhau.
`Vec::first` trả về `Option`, trong khi `parse::<i32>` trả về
`Result<i32, ParseIntError>`:

```rust,editable,ignore,mdbook-runnable
fn double_first(vec: Vec<&str>) -> i32 {
    let first = vec.first().unwrap(); // Trả về loại lỗi 1
    2 * first.parse::<i32>().unwrap() // Trả về loại lỗi 2
}

fn main() {
    let numbers = vec!["42", "93", "18"];
    let empty = vec![];
    let strings = vec!["tofu", "93", "18"];

    println!("The first doubled is {}", double_first(numbers));

    println!("The first doubled is {}", double_first(empty));
    // Loại lỗi 1: vectơ đầu vào rỗng

    println!("The first doubled is {}", double_first(strings));
    // Loại lỗi 2: phần tử không thể chuyển đổi thành một số
}
```

Trong các phần tiếp theo, chúng ta sẽ thấy một vài phương pháp để xử lý những vấn đề như như trên.