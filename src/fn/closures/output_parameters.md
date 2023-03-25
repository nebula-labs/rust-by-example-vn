# Sử dụng closure dưới dạng tham số đầu ra (output parameters)

Không những closure có thể sử dụng dưới dạng tham số truyền vào, mà còn có thể sử dụng closure dưới dạng kết quả trả về của một hàm.
Tuy nhiên, các kiểu closure ẩn danh theo định nghĩa là không xác định nên ta phải dùng cú pháp `impl Trait` để return closure.

Các trait hợp lệ có thể được dùng để khai báo kiểu trả về là closure là:

- `Fn`
- `FnMut`
- `FnOnce`

Hơn nữa, từ khóa `move` phải được sử dụng để chỉ ra rằng tất cả các giá trị được capture đều bằng tham trị. Điều này là bắt buộc vì
bất kỳ việc capture giá trị nào theo tham chiếu sẽ bị loại bỏ ngay khi hàm thoát ra, dẫn đến các lỗi về tham chiếu không hợp lệ trong closure.

```rust,editable
fn create_fn() -> impl Fn() {
    let text = "Fn".to_owned();

    move || println!("This is a: {}", text)
}

fn create_fnmut() -> impl FnMut() {
    let text = "FnMut".to_owned();

    move || println!("This is a: {}", text)
}

fn create_fnonce() -> impl FnOnce() {
    let text = "FnOnce".to_owned();

    move || println!("This is a: {}", text)
}

fn main() {
    let fn_plain = create_fn();
    let mut fn_mut = create_fnmut();
    let fn_once = create_fnonce();

    fn_plain();
    fn_mut();
    fn_once();
}
```

### Đọc thêm:

[`Fn`][fn], [`FnMut`][fnmut], [Generics][generics] và [impl Trait][impltrait].

[fn]: https://doc.rust-lang.org/std/ops/trait.Fn.html
[fnmut]: https://doc.rust-lang.org/std/ops/trait.FnMut.html
[generics]: ../../generics.md
[impltrait]: ../../trait/impl_trait.md
