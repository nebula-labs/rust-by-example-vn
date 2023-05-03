# Testcase: unit clarification
# Bộ kiểm thử: Sự tường minh của Đơn vị đo lường

Một phương pháp hữu ích cho việc chuyển đổi đơn vị có thể được thực hiện bằng cách
triển khai trait `Add` với một tham số có kiểu dữ liệu ảo. Dưới đây, `Trait` `Add` được
triển khai như một ví dụ:

```rust,ignore
// Cấu trúc này sẽ áp dụng: `Self + RHS = Output`
// trong đó RHS sẽ mặc định là Self nếu không được chỉ định trong việc triển khai
pub trait Add<RHS = Self> {
    type Output;

    fn add(self, rhs: RHS) -> Self::Output;
}

// `Output` phải có kiểu `T<U>` sao cho `T<U> + T<U> = T<U>`.
impl<U> Add for T<U> {
    type Output = T<U>;
    ...
}
```

Toàn bộ đoạn mã được triển khai:

```rust,editable
use std::ops::Add;
use std::marker::PhantomData;

/// Tạo các enums trống để định nghĩa các loại đơn vị.
#[derive(Debug, Clone, Copy)]
enum Inch {}
#[derive(Debug, Clone, Copy)]
enum Mm {}
/// `Length` là một kiểu dữ liệu có tham số ảo `Unit`, và
/// không phải là generic theo đơn vị chiều dài (trong trường hợp này là f64)
/// `f64` vốn đã được triển khai các trait `Clone` and `Copy`.
#[derive(Debug, Clone, Copy)]
struct Length<Unit>(f64, PhantomData<Unit>);

/// Trait `Add` định nghĩa hành vi (behavior) của phép tính `+`.
impl<Unit> Add for Length<Unit> {
    type Output = Length<Unit>;

    // Hàm add() trả về một struct `Length` chứa tổng.
    fn add(self, rhs: Length<Unit>) -> Length<Unit> {
        // `+` thực thi trait `Add` của kiểu `f64`.
        Length(self.0 + rhs.0, PhantomData)
    }
}

fn main() {
    // Chỉ định biến `one_foot` có tham số mang kiểu dữ liệu ảo là `Inch`.
    let one_foot:  Length<Inch> = Length(12.0, PhantomData);
    // `one_meter` có tham số mang kiểu dữ liệu ảo là `Mm`.
    let one_meter: Length<Mm>   = Length(1000.0, PhantomData);

    // `+` goi tới phương thức `add()` mà ta đã triển khai cho struct `Length<Unit>`.
    //
    // Vì struct `Length` đã được triển khai trait `Copy`, hàm `add()` sẽ không tiêu thụ 2 biến
    // `one_foot` và `one_meter` mà chỉ copy chúng trở thành `self` and `rhs`.
    let two_feet = one_foot + one_foot;
    let two_meters = one_meter + one_meter;

    // Phép cộng hoạt động.
    println!("one foot + one_foot = {:?} in", two_feet.0);
    println!("one meter + one_meter = {:?} mm", two_meters.0);

    // Các phép tính vô lý sẽ thất bại như là một điều hiển nhiên:
    // Lỗi tại thời điểm biên dịch: không phù hợp kiểu dữ liệu.
    // let one_feter = one_foot + one_meter;
}
```

### Tham khảo:

[Borrowing (`&`)], [Bounds (`X: Y`)], [enum], [impl & self],
[Overloading], [ref], [Traits (`X for Y`)], and [TupleStructs].

[Borrowing (`&`)]: ../../scope/borrow.md
[Bounds (`X: Y`)]: ../../generics/bounds.md
[enum]: ../../custom_types/enum.md
[impl & self]: ../../fn/methods.md
[Overloading]: ../../trait/ops.md
[ref]: ../../scope/borrow/ref.md
[Traits (`X for Y`)]: ../../trait.md
[TupleStructs]: ../../custom_types/structs.md
[std::marker::PhantomData]: https://doc.rust-lang.org/std/marker/struct.PhantomData.html
