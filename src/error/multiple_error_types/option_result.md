# Lấy `Result` ra khỏi `Option`

Cách cơ bản nhất để xử lý loại lỗi hỗn hợp là nhúng chúng vào với nhau.

```rust
use std::num::ParseIntError;

fn double_first(vec: Vec<&str>) -> Option<Result<i32, ParseIntError>> {
    vec.first().map(|first| {
        first.parse::<i32>().map(|n| 2 * n)
    })
}

fn main() {
    let numbers = vec!["42", "93", "18"];
    let empty = vec![];
    let strings = vec!["tofu", "93", "18"];

    println!("The first doubled is {:?}", double_first(numbers));

    println!("The first doubled is {:?}", double_first(empty));
    // Error 1: Input vector rỗng.

    println!("The first doubled is {:?}", double_first(strings));
    // Error 2: phần tử không thể chuyển thành một số.
}
```
Có những lúc chúng ta muốn dừng việc xử lý khi có lỗi xảy ra( giống như khi sử dụng `?`) nhưng  vẫn tiếp tục xử lý khi `Option` là `None`. Mội vài combinator rất hữu ích để  hoán đổi `Result` và `Option`.

```rust
use std::num::ParseIntError;

fn double_first(vec: Vec<&str>) -> Result<Option<i32>, ParseIntError> {
    let opt = vec.first().map(|first| {
        first.parse::<i32>().map(|n| 2 * n)
    });

    opt.map_or(Ok(None), |r| r.map(Some))
}

fn main() {
    let numbers = vec!["42", "93", "18"];
    let empty = vec![];
    let strings = vec!["tofu", "93", "18"];

    println!("The first doubled is {:?}", double_first(numbers));
    println!("The first doubled is {:?}", double_first(empty));
    println!("The first doubled is {:?}", double_first(strings));
}
```
