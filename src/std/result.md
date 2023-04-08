# `Result`

Ta có thể thấy rằng enum `Option` có thể được dùng như là giá trị trả về từ các hàm có thể có lỗi,
trong đó `None` có thể được dùng để trả về để biểu thị rằng có lỗi xảy ra. Tuy nhiên đôi khi việc diễn tả tại sao
một thao tác bị lỗi lại quan trọng hơn. Để làm được điều này, ta đã có enum `Result`.

Enum `Result<T, E>` có hai biến thể:

- `Ok(value)` để biểu thị một thao tác đã thành công và kèm theo đó là
  `value` được trả về bởi thao tác đó. (`value` có kiểu `T`)
- `Err(why)` để biểu thị một thao tác đã thất bại và kèm theo đó là `why`, thứ
  được kì vọng để giải thích nguyên nhân gây nên lỗi. (`why` có kiểu `E`)

```rust,editable,ignore,mdbook-runnable
mod checked {
    // "Các lỗi" toán học mà chúng ta muốn bắt
    #[derive(Debug)]
    pub enum MathError {
        DivisionByZero,
        NonPositiveLogarithm,
        NegativeSquareRoot,
    }

    pub type MathResult = Result<f64, MathError>;

    pub fn div(x: f64, y: f64) -> MathResult {
        if y == 0.0 {
            // Thao tác này sẽ `thất bại`, thay vào đó hãy trả về nguyên nhân của
            // sự thất bại được gói bên trong `Err`
            Err(MathError::DivisionByZero)
        } else {
            // Thao tác này là hợp lệ, trả về kết quả được gói bên trong `Ok`
            Ok(x / y)
        }
    }

    pub fn sqrt(x: f64) -> MathResult {
        if x < 0.0 {
            Err(MathError::NegativeSquareRoot)
        } else {
            Ok(x.sqrt())
        }
    }

    pub fn ln(x: f64) -> MathResult {
        if x <= 0.0 {
            Err(MathError::NonPositiveLogarithm)
        } else {
            Ok(x.ln())
        }
    }
}

// `op(x, y)` === `sqrt(ln(x / y))`
fn op(x: f64, y: f64) -> f64 {
    // Dưới đây là ba tầng match lồng nhau!
    match checked::div(x, y) {
        Err(why) => panic!("{:?}", why),
        Ok(ratio) => match checked::ln(ratio) {
            Err(why) => panic!("{:?}", why),
            Ok(ln) => match checked::sqrt(ln) {
                Err(why) => panic!("{:?}", why),
                Ok(sqrt) => sqrt,
            },
        },
    }
}

fn main() {
    // Liệu thao tác có thất bại không?
    println!("{}", op(1.0, 10.0));
}
```
