# `Result`
[`Result`][result] là phiên bản nâng cấp của kiểu [`Option`][option], mô tả lỗi có thể xảy ra thay vì chỉ mô tả việc kết quả có thể có hoặc không có.

Đúng với nhận định trên, `Result<T, E>` trong Rust có thể có một trong hai kết quả:

* `Ok(T)`: Một thực thể `T` được tìm thấy
* `Err(E)`: Một lỗi được tìm thấy là `E`

Trong Rust, quy ước rằng kết quả mong đợi của một hàm là `Ok`, trong khi kết quả không mong đợi là `Err`.

Tương tự `Option`, `Result` đi kèm với rất nhiều phương thức. Ví dụ như `unwrap()`,
sẽ trả về thực thể `T` hoặc `panic`. Tùy vấn đề cần xử lí mà ta kết hợp cả `Result` và `Option`. 

Khi dùng Rust, bạn sẽ thường gặp các phương thức trả về kiểu dữ liệu `Result`, 
như là phương thức [`parse()`][parse]. Không phải lúc nào cũng có thể chuyển đổi
một chuỗi kí tự thành một kiểu dữ liệu khác nên `parse()` sẽ trả về một
kiểu dữ liệu `Result` cho biết có thể lỗi sẽ xảy ra
be possible to parse a string into the other type, so `parse()` returns a
`Result` indicating possible failure.

Hãy xem xét những gì sẽ xảy ra khi chúng ta chuyển đổi thành công và không thành công một chuỗi bằng phương thức `parse()`:

```rust,editable,ignore,mdbook-runnable
fn multiply(first_number_str: &str, second_number_str: &str) -> i32 {
    // Let's try using `unwrap()` to get the number out. Will it bite us?
    let first_number = first_number_str.parse::<i32>().unwrap();
    let second_number = second_number_str.parse::<i32>().unwrap();
    first_number * second_number
}

fn main() {
    let twenty = multiply("10", "2");
    println!("double is {}", twenty);

    let tt = multiply("t", "2");
    println!("double is {}", tt);
}
```

Trong trường hợp thất bại, `parse()` sẽ trả về một lỗi để `unwrap()`
có thể gọi hàm `panic`. Tiếp theo, hàm `panic` sẽ thoát chương trình và cung cấp
một thông báo lỗi không mong muốn.

Để cải thiện chất lượng thông báo lỗi, chúng ta nên cụ thể hóa hơn về kiểu giá trị trả về
và xử lí lỗi kĩ càng hơn

## Using `Result` in `main`

Kiểu dữ liệu `Result` cũng có thể là kiểu dữ liệu trả về của hàm `main`
nếu được chỉ định rõ ràng. Thông thường, hàm `main` sẽ có dạng:

```rust
fn main() {
    println!("Hello World!");
}
```
Tuy nhiên hàm `main` cũng trả về kiểu `Result`. Nếu một lỗi xảy ra trong hàm `main`,
nó sẽ trả về mã lỗi và in ra một bản trình bày về lỗi (sử dụng trait [`Debug`]).
Ví dụ sau đây là một trường hợp minh chứng và đề cập đến các khía cạnh được
nói qua ở [mục này].

```rust,editable
use std::num::ParseIntError;

fn main() -> Result<(), ParseIntError> {
    let number_str = "10";
    let number = match number_str.parse::<i32>() {
        Ok(number)  => number,
        Err(e) => return Err(e),
    };
    println!("{}", number);
    Ok(())
}
```


[option]: https://doc.rust-lang.org/std/option/enum.Option.html
[result]: https://doc.rust-lang.org/std/result/enum.Result.html
[parse]: https://doc.rust-lang.org/std/primitive.str.html#method.parse
[`Debug`]: https://doc.rust-lang.org/std/fmt/trait.Debug.html
[the following section]: result/early_returns.md
