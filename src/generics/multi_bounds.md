# Multiple bounds

Trong Rust, chúng ta có thể áp dụng nhiều ràng buộc cho một kiểu dữ liệu bằng cách sử dụng toán tử `+`. Thông thường, các kiểu dữ liệu khác nhau được phân tách bằng dấu `,`.

```rust,editable
use std::fmt::{Debug, Display};

fn compare_prints<T: Debug + Display>(t: &T) {
    println!("Debug: `{:?}`", t);
    println!("Display: `{}`", t);
}

fn compare_types<T: Debug, U: Debug>(t: &T, u: &U) {
    println!("t: `{:?}`", t);
    println!("u: `{:?}`", u);
}

fn main() {
    let string = "words";
    let array = [1, 2, 3];
    let vec = vec![1, 2, 3];

    compare_prints(&string);
    //compare_prints(&array);
    // TODO ^ thử bỏ chú thích(uncommenting) dòng phía trên.

    compare_types(&array, &vec);
}
```

### See also:

[`std::fmt`][fmt] and [`trait`s][traits]

[fmt]: ../hello/print.md
[traits]: ../trait.md