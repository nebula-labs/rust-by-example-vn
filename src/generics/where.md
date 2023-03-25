# Mệnh đề where

Một bound (ràng buộc) cũng có thể được định nghĩa bằng cách sử dụng mệnh đề `where` ngay trước dấu `{`, thay vì phải định nghĩa tại ngay lần đề cập đầu tiên của một kiểu.
Ngoài ra, các mệnh đề `where` có thể áp dụng bound cho các kiểu tuỳ ý, chứ không chỉ riêng cho các tham số kiểu (type parameters).

Việc sử dụng mệnh đề `where` sẽ hữu dụng khi rơi vào những trường hợp sau:

- Khi muốn rõ ràng hơn bằng cách tách phần chỉ định các kiểu generic ra riêng và chỉ định bound ra riên

```rust,ignore
impl <A: TraitB + TraitC, D: TraitE + TraitF> MyTrait<A, D> for YourType {}
// Biểu thị bounds bằng mệnh đề `where`
impl <A, D> MyTrait<A, D> for YourType where
    A: TraitB + TraitC,
    D: TraitE + TraitF {}
```

- Khi mà việc sử dụng mệnh đề `where` sẽ trực quan dễ đọc hơn việc sử dụng cú pháp thông thường:

Phần `impl` trong ví dụ dưới đây sẽ không thể được biểu thị trực tiếp nếu không dùng mệnh đề `where`

```rust,editable
use std::fmt::Debug;
trait PrintInOption {
    fn print_in_option(self);
}
// Ở đoạn này, nếu không dùng `where`, ta sẽ phải khai báo như sau `T: Debug` hoặc là
// dùng một cách gián tiếp khác để khai báo, do đó ta sử dụng mệnh đề `where` để dễ đọc hơn:
impl<T> PrintInOption for T where
    Option<T>: Debug {
    // Chúng ta muốn `Option<T>: Debug` sẽ là bound ở đây bởi vì đó là thứ sẽ được in ra.
    // Nếu sử dụng cách khác sẽ dẫn đến việc sử dụng một bound không đúng.
    fn print_in_option(self) {
        println!("{:?}", Some(self));
    }
}
fn main() {
    let vec = vec![1, 2, 3];
    vec.print_in_option();
}
```

### Đọc thêm:

[RFC][where], [`struct`][struct], và [`trait`][trait]

[struct]: ../custom_types/structs.md
[trait]: ../trait.md
[where]: https://github.com/rust-lang/rfcs/blob/master/text/0135-where.md
