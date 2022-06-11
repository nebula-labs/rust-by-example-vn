# Aliasing (Đặt bí danh cho kiểu)

Từ khóa `type` có thể được sử dụng để tạo ra một tên mới cho một kiểu đã tồn tại. Các kiểu
phải có tên dạng `UpperCamelCase`, nếu không trình biên dịch sẽ ném ra một cảnh báo. Ngoại
lệ cho quy tắc trên là các kiểu dữ liệu nguyên thủy: `usize`, `f32`, etc.

```rust,editable
// `NanoSecond`, `Inch`, và `U64` là các tên mới cho `u64`.
type NanoSecond = u64;
type Inch = u64;
type U64 = u64;

fn main() {
    // `NanoSecond` = `Inch` = `U64` = `u64`.
    let nanoseconds: NanoSecond = 5 as U64;
    let inches: Inch = 2 as U64;

    // Lưu ý rằng kiểu bí danh *không* cung cấp bất kỳ bổ sung nào, bởi vì
    // bí danh là *không phải* là một kiểu mới.
    // Như ở dưới đây ta có thể cộng được các đơn vị khác nhau, bởi bản chất
    // chúng đều có kiểu là số nguyên `u64` chứ không phải như tên bí danh là kiểu dữ liệu
    // cho đơn vị thời gian hay đơn vị độ dài.
    println!("{} nanoseconds + {} inches = {} unit?",
             nanoseconds,
             inches,
             nanoseconds + inches);
}
```

Mục đích chính sử dụng các bí danh là để giảm thiểu việc dùng boilerplate; ví dụ như kiểu `IoResult<T>`
là một bí danh thay thế cho kiểu `Result<T, IoError>`.

### Xem thêm tại đây:

[Attributes](../attribute.md)
