# Kiểm thử đơn vị (Unit testing)

Các bài tests trong Rust là các hàm để kiểm tra xem các đoạn code chức năng có hoạt động
giống như mong đợi hay không. Phần thân của các hàm test thường sẽ thực hiên một vài cài đặt,
thực thi đoạn code mà chúng ta muốn kiểm thử, sau đó khẳng định xem kết quả thực thi có giống như chúng ta
mong đợi hay không.

Hầu hết các bài unit tests thường sẽ đặt trong `tests` [mod][mod] với `#[cfg(test)]` [attribute][attribute].
Các hàm tests được đánh dấu bằng thuộc tính `#[test]`.

Các bài tests sẽ thất bại khi có bất kì đoạn nào bên trong hàm test [panics][panic]. Dưới đây là một số [macros][macros] hỗ trợ:

- `assert!(expression)` - panics nếu có biểu thức được đánh giá là `false`.
- `assert_eq!(left, right)` và `assert_ne!(left, right)` - tương ứng với kiểm tra tính bằng nhau và khác nhau của biểu thức trái và phải.

```rust,ignore
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

// Dưới đây là một hàm cộng bị sai, mục đích nó là để chạy lỗi
// trong ví dụ này.
#[allow(dead_code)]
fn bad_add(a: i32, b: i32) -> i32 {
    a - b
}

#[cfg(test)]
mod tests {
    // Lưu ý kỹ thuật hữu ích này: import các module khác từ scope bên ngoài (để dùng cho các mod tests).
    use super::*;

    #[test]
    fn test_add() {
        assert_eq!(add(1, 2), 3);
    }

    #[test]
    fn test_bad_add() {
        // Macro assert sau sẽ khởi chạy và đoạn test sẽ thất bại
        // Hãy lưu ý rằng, các hàm private cũng có thể được test!
        assert_eq!(bad_add(1, 2), 3);
    }
}
```

Các bài tests có thể được thực thi bằng lệnh `cargo test`.

```shell
$ cargo test

running 2 tests
test tests::test_bad_add ... FAILED
test tests::test_add ... ok

failures:

---- tests::test_bad_add stdout ----
        thread 'tests::test_bad_add' panicked at 'assertion failed: `(left == right)`
  left: `-1`,
 right: `3`', src/lib.rs:21:8
note: Run with `RUST_BACKTRACE=1` for a backtrace.


failures:
    tests::test_bad_add

test result: FAILED. 1 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out
```

## Các bài tests và `?`

Không có ví dụ về unit test nào phía trên có kiểu trả về. Tuy nhiên ở phiên bản Rust 2018,
các bài unit tests của bạn đã có thể trả về `Return<()>`, thứ mà cho phép ta sử dụng `?` bên trong nó! Điều này
có thể khiến cho các bài tests trở nên ngắn gọn hơn.

```rust,editable
fn sqrt(number: f64) -> Result<f64, String> {
    if number >= 0.0 {
        Ok(number.powf(0.5))
    } else {
        Err("negative floats don't have square roots".to_owned())
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_sqrt() -> Result<(), String> {
        let x = 4.0;
        assert_eq!(sqrt(x)?.powf(2.0), x);
        Ok(())
    }
}
```

Đọc ["The Edition Guide"][editionguide] để biết thêm thông tin chi tiết.

## Testing panics (kiểm thử các panics)

Để kiểm tra các hàm nên panic trong một số trường hợp nhất định, sử dụng thuộc tính
`#[should_panic]`. Thuộc tính này chấp nhận đối số tùy chọn `expected = ` với giá trị là
văn bản của thông điệp khi panic. Nếu hàm của bạn có thể panic theo nhiều cách khác nhau, điều này giúp
đảm bảo rằng bài test đang kiểm thử chính xác lỗi panic.

```rust,ignore
pub fn divide_non_zero_result(a: u32, b: u32) -> u32 {
    if b == 0 {
        panic!("Divide-by-zero error");
    } else if a < b {
        panic!("Divide result is zero");
    }
    a / b
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_divide() {
        assert_eq!(divide_non_zero_result(10, 2), 5);
    }

    #[test]
    #[should_panic]
    fn test_any_panic() {
        divide_non_zero_result(1, 0);
    }

    #[test]
    #[should_panic(expected = "Divide result is zero")]
    fn test_specific_panic() {
        divide_non_zero_result(1, 10);
    }
}
```

Khi chạy các bài tests ta thu được:

```shell
$ cargo test

running 3 tests
test tests::test_any_panic ... ok
test tests::test_divide ... ok
test tests::test_specific_panic ... ok

test result: ok. 3 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

   Doc-tests tmp-test-should-panic

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

## Thực hiện các bài tests cụ thể

Để chạy các bài tests cụ thể, ta có thể sẽ phải chỉ định tên của bài test với lệnh `cargo test`.

```shell
$ cargo test test_any_panic
running 1 test
test tests::test_any_panic ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 2 filtered out

   Doc-tests tmp-test-should-panic

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

Để chạy nhiều bài tests, ta có thể sẽ phải chỉ định một phần của tên bài test khớp với tất cả
bài tests mà ta muốn chạy.

```shell
$ cargo test panic
running 2 tests
test tests::test_any_panic ... ok
test tests::test_specific_panic ... ok

test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 1 filtered out

   Doc-tests tmp-test-should-panic

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

## Phớt lờ (Ignoring) các bài tests

Các bài tests có thể được đánh dấu bằng thuộc tính `#[ignore]` để có thể bỏ qua một vài bài tests. Hoặc là chạy
các tests đó với lệnh `cargo test -- --ignored`.

```rust
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_add() {
        assert_eq!(add(2, 2), 4);
    }

    #[test]
    fn test_add_hundred() {
        assert_eq!(add(100, 2), 102);
        assert_eq!(add(2, 100), 102);
    }

    #[test]
    #[ignore]
    fn ignored_test() {
        assert_eq!(add(0, 0), 0);
    }
}
```

```shell
$ cargo test
running 3 tests
test tests::ignored_test ... ignored
test tests::test_add ... ok
test tests::test_add_hundred ... ok

test result: ok. 2 passed; 0 failed; 1 ignored; 0 measured; 0 filtered out

   Doc-tests tmp-ignore

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

$ cargo test -- --ignored
running 1 test
test tests::ignored_test ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

   Doc-tests tmp-ignore

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

[attribute]: ../attribute.md
[panic]: ../std/panic.md
[macros]: ../macros.md
[mod]: ../mod.md
[editionguide]: https://doc.rust-lang.org/edition-guide/rust-2018/error-handling-and-panics/question-mark-in-main-and-tests.html
