# Sử dụng closure dưới dạng tham số đầu ra (output parameters)

Không những ta có thể sử dụng closure dưới dạng tham số để truyền vào hàm hoặc phương thức,
mà còn có thể sử dụng closure dưới dạng kết quả trả về của một hàm. Mặc dù lý thuyết là vậy, nhưng
bởi vì theo định nghĩa kiểu của closure là unknown nên ta phải dùng cú pháp `impl Trait` thì mới có thể
trả nó về dưới dạng kết quả của một hàm.

Các trait hợp lệ có thể được dùng để khai báo kiểu trả về là closure là:

- `Fn`
- `FnMut`
- `FnOnce`

Ngoài ra, ta phải dùng thêm từ khoá `move` trong trường hợp này để thông báo cho compiler hiểu rằng
các biến được bắt giữ bởi những closure này đều bằng tham trị. Đây là điều cần thiết vì nếu không làm vậy,
các biến được sử dụng dưới dạng tham chiếu bên trong closure trả về sẽ bị huỷ
và xoá khỏi bộ nhớ một khi hàm trả về closure thực thi xong, dẫn đến các lỗi về tham chiếu không hợp lệ trong closure.

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

### See also:

[`Fn`][fn], [`FnMut`][fnmut], [Generics][generics] and [impl Trait][impltrait].

[fn]: https://doc.rust-lang.org/std/ops/trait.Fn.html
[fnmut]: https://doc.rust-lang.org/std/ops/trait.FnMut.html
[generics]: ../../generics.md
[impltrait]: ../../trait/impl_trait.md
