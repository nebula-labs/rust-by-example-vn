# `?`

Việc xử lý một chuỗi các kết quả sử dụng match có thể làm cho đoạn mã của bạn trở nên khá lộn xộn và khó đọc; may mắn là, toán tử `?` có thể để làm cho mọi thứ trở nên đẹp đẽ và dễ đọc hơn. `?` được sử dụng ở cuối một biểu thức trả về là một `Result`, và nó tương đương với một biểu thức match, trong đó nhánh `Err(err)` mở rộng thành một `return Err(From::from(err))`, và nhánh `Ok(ok)` mở rộng thành một biểu thức `ok`.

```rust,editable,ignore,mdbook-runnable
mod checked {
    #[derive(Debug)]
    enum MathError {
        DivisionByZero,
        NonPositiveLogarithm,
        NegativeSquareRoot,
    }

    type MathResult = Result<f64, MathError>;

    fn div(x: f64, y: f64) -> MathResult {
        if y == 0.0 {
            Err(MathError::DivisionByZero)
        } else {
            Ok(x / y)
        }
    }

    fn sqrt(x: f64) -> MathResult {
        if x < 0.0 {
            Err(MathError::NegativeSquareRoot)
        } else {
            Ok(x.sqrt())
        }
    }

    fn ln(x: f64) -> MathResult {
        if x <= 0.0 {
            Err(MathError::NonPositiveLogarithm)
        } else {
            Ok(x.ln())
        }
    }

    // Hàm trung gian
    fn op_(x: f64, y: f64) -> MathResult {
        // nếu `div` "lỗi", `DivisionByZero` sẽ được trả về
        let ratio = div(x, y)?;

        // nếu `ln` "lỗi", `NonPositiveLogarithm` sẽ được trả về
        let ln = ln(ratio)?;

        sqrt(ln)
    }

    pub fn op(x: f64, y: f64) {
        match op_(x, y) {
            Err(why) => panic!("{}", match why {
                MathError::NonPositiveLogarithm
                    => "logarithm of non-positive number",
                MathError::DivisionByZero
                    => "division by zero",
                MathError::NegativeSquareRoot
                    => "square root of negative number",
            }),
            Ok(value) => println!("{}", value),
        }
    }
}

fn main() {
    checked::op(1.0, 10.0);
}
```

Đảm bảo rằng bạn sẽ kiểm tra [documentation][docs], vì có nhiều phương pháp để map/compose `Result`.

[docs]: https://doc.rust-lang.org/std/result/index.html