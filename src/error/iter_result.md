# Lặp qua các `Result`

Một `Iter::map` có thể không thành công, ví dụ:

```rust,editable
fn main() {
    let strings = vec!["tofu", "93", "18"];
    let numbers: Vec<_> = strings
        .into_iter()
        .map(|s| s.parse::<i32>())
        .collect();
    println!("Results: {:?}", numbers);
    // Results: [Err(ParseIntError { kind: InvalidDigit }), Ok(93), Ok(18)]
}
```

Hãy thông qua từng bước của chiến lược để xứ lý tình huống này.

## Bỏ qua các phần tử bị lỗi bằng cách sử dụng `filter_map()`

`filter_map` gọi một function và loại các các kêt quả bị lỗi.

```rust,editable
fn main() {
    let strings = vec!["tofu", "93", "18"];
    let numbers: Vec<_> = strings
        .into_iter()
        .filter_map(|s| s.parse::<i32>().ok())
        .collect();
    println!("Results: {:?}", numbers);
    //  [93, 18]
}
```

## Thu thập các phần tử bị lỗi với `map_err()` và `filter_map()`

`map_err` gọi một function nhận vào một error, bằng cách thêm `map_err` vào `filter_map` chúng ta có thể lưu lại các lỗi xảy ra trong lúc thực hiện vòng lặp.

```rust,editable
fn main() {
    let strings = vec!["42", "tofu", "93", "999", "18"];
    let mut errors = vec![];
    let numbers: Vec<_> = strings
        .into_iter()
        .map(|s| s.parse::<u8>())
        .filter_map(|r| r.map_err(|e| errors.push(e)).ok())
        .collect();
    println!("Numbers: {:?}", numbers);
    println!("Errors: {:?}", errors);
    // Numbers: [42, 93, 18]
    // Errors: [ParseIntError { kind: InvalidDigit }, ParseIntError { kind: PosOverflow }] 
}
```

## Kết thúc vòng lặp với `collect()`

`Result` có triển khai `FromIterator`, nên một vector chứa các kết quả (`Vec<Result<T, E>>`)
có thể được chuyển thành một kết quả với vector(`Result<Vec<T>, E>`). Khi một
`Result::Err` được tìm thấy, vòng lặp sẽ bị hủy.

```rust,editable
fn main() {
    let strings = vec!["tofu", "93", "18"];
    let numbers: Result<Vec<_>, _> = strings
        .into_iter()
        .map(|s| s.parse::<i32>())
        .collect();
    println!("Results: {:?}", numbers);
}
```

Một cơ chế khác có thể được sử dụng là `Option`.

## Thu thập các giá trị hợp lệ và các lỗi với `partition()`

```rust,editable
fn main() {
    let strings = vec!["tofu", "93", "18"];
    let (numbers, errors): (Vec<_>, Vec<_>) = strings
        .into_iter()
        .map(|s| s.parse::<i32>())
        .partition(Result::is_ok);
    println!("Numbers: {:?}", numbers);
    println!("Errors: {:?}", errors);
    // Numbers: [Ok(93), Ok(18)]
    // Errors: [Err(ParseIntError { kind: InvalidDigit })]
}
```

Khi nhìn vào các kết quả, bạn sẽ thấy rằng mọi thứ vẫn được bọc bởi `Result`. Vậy nên cần xử lí thêm một chút.

```rust,editable
fn main() {
    let strings = vec!["tofu", "93", "18"];
    let (numbers, errors): (Vec<_>, Vec<_>) = strings
        .into_iter()
        .map(|s| s.parse::<i32>())
        .partition(Result::is_ok);
    let numbers: Vec<_> = numbers.into_iter().map(Result::unwrap).collect();
    let errors: Vec<_> = errors.into_iter().map(Result::unwrap_err).collect();
    println!("Numbers: {:?}", numbers);
    println!("Errors: {:?}", errors);
    // Numbers: [93, 18]
    // Errors: [ParseIntError { kind: InvalidDigit }]
}
```
