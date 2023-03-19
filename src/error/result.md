# `Result`

[`Result`][result], kiểu dữ liệu diễn tả được *lỗi* xảy ra (nếu có) là phiên bản nâng cấp
 của kiểu [`Option`][option], chỉ có thể trả ra một thực thể có tồn tại hay không.

Bởi vì, `Result<T, E>` có thể chỉ đến 1 trong 2 kết quả:

* `Ok(T)`: Một thực thể `T` được tìm thấy
* `Err(E)`: Một lỗi được tìm thấy là `E`

Theo thông lệ, Kết quả mong muốn sẽ là `Ok` trong khi kết quả không mong đợi sẽ là `Err`.

Tương tự `Option`, `Result` đi kèm với rất nhiều phương thức. Ví dụ như `unwrap()`,
sẽ trả về thực thể `T` hoặc `panic`. Tùy vấn đề cần xử lí mà ta kết hợp cả `Result` và `Option`. 

Khi dùng Rust, bạn sẽ thường gặp các phương thức trả về kiểu dữ liệu `Result`, 
như là phương thức [`parse()`][parse]. Không phải lúc nào cũng có thể chuyển đổi
một chuỗi kí tự thành một kiểu dữ liệu khacsm nên `parse()` sẽ trả về một
kiểu dữ liệu `Result` chỉ ra lỗi nếu có
be possible to parse a string into the other type, so `parse()` returns a
`Result` indicating possible failure.

Hãy cùng xem chuyện gì sẽ xảy ra khi ta `parse()` một chuỗi nhé:

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
một thông điệp lỗi khó hiểu.

Để nâng cao chất lượng của thông điệp lỗi, ta chuyên biệt hóa kiểu dữ liệu trả về
và xử lí lỗi kĩ càng hơn

## Using `Result` in `main`

Kiểu dữ liệu `Result` còn có thể là kiểu dữ liệu trả về của hàm `main`
nếu ta xử lí chuyên biệt. Thường thì hàm `main` sẽ có dạng:

```rust
fn main() {
    println!("Hello World!");
}
```
Tuy nhiên hàm `main` cũng trả về kiểu `Result`. Nếu một lỗi xảy ra trong hàm `main`,
nó sẽ trả về mã lỗi và in ra một bản trình bày về lỗi (sử dụng trait [`Debug`]).
Ví dụ sau đây là một trường hợp minh chứng và cũng đụng đến một vài khía cạnh được
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
