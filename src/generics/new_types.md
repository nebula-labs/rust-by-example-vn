# New Type Idiom

Thành ngữ `newtype` đảm bảo tại thời điểm biên dịch rằng giá trị của đúng kiểu được phải cung cấp cho chương trình.

Ví dụ, một hàm xác minh tuổi tác _phải_ được truyền vào một giá trị kiểu `Years`.

```rust, editable
struct Years(i64);

struct Days(i64);

impl Years {
    pub fn to_days(&self) -> Days {
        Days(self.0 * 365)
    }
}


impl Days {
    /// Căt bỏ phần dư của năm, tính năm tròn.
    pub fn to_years(&self) -> Years {
        Years(self.0 / 365)
    }
}

fn old_enough(age: &Years) -> bool {
    age.0 >= 18
}

fn main() {
    let age = Years(5);
    let age_days = age.to_days();
    println!("Đủ lớn {}", old_enough(&age));
    println!("Đủ lớn {}", old_enough(&age_days.to_years()));
    // println!("Đủ lớn {}", old_enough(&age_days));
}
```

Bỏ ghi chú dòng cuối cùng để thấy rằng kiểu được cung cấp phải là `Years`.

Để lấy được giá trị `newtype` theo kiểu cơ sở, bạn có thể sử dụng cú pháp `tuple` hoặc phân rã (destructuring) như sau:

```rust, editable
struct Years(i64);

fn main() {
    let years = Years(42);
    let years_as_primitive_1: i64 = years.0; // Tuple
    let Years(years_as_primitive_2) = years; // Destructuring
}
```

### Xem thêm:

[`structs`][struct]

[struct]: ../custom_types/structs.md
