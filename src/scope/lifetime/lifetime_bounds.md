# Bounds

Tương tự như việc các kiểu generic có thể bị bounded (ràng buộc), lifetimes (bản thân nó cũng là generic) cũng
có thể sử dụng các bounds. Kí tự `:` sẽ có một ý nghĩa hơi khác ở đây, còn kí tự `+` thì vẫn có ý nghĩa như cũ.
Chú ý cách đọc của các cú pháp dưới đây như sau:

1. `T: 'a`: _Tất cả_ các tham chiếu trong `T` phải tồn tại lâu hơn lifetime `'a`[^†].
2. `T: Trait + 'a`: Kiểu `T` phải implement trait `Trait` và _tất cả_ các tham chiếu trong `T` phải phải tồn tại lâu hơn lifetime `'a`.

Ví dụ dưới đây sẽ áp dụng biểu diễn cách các cú pháp phía trên được sử dụng ở sau từ khoá `where`

```rust,editable
use std::fmt::Debug; // Trait dùng để bound.

#[derive(Debug)]
struct Ref<'a, T: 'a>(&'a T);
// Struct `Ref` bao gồm 1 tham chiếu đến kiểu generic `T` với lifetime
// không xác định là `'a`. `T` bị bounded sao cho *các tham chiếu* trong `T`
// phải tồn tại lâu hơn `'a`. Thêm vào đó, lifetime
// của `Ref` không được vượt quá `'a`.

// Hàm generic thực thi lệnh in sử dụng trait `Debug`
fn print<T>(t: T) where
    T: Debug {
    println!("`print`: t is {:?}", t);
}

// Dưới đây là một tham chiếu đến `T`, trong đó `T` implements `Debug`
// và tất cả *các tham chiếu* trong `T` phải tồn tại lâu hơn `'a`. Hơn nữa,
// `'a` phải tồn tại lâu hơn lifetime của hàm.

fn print_ref<'a, T>(t: &'a T) where
    T: Debug + 'a {
    println!("`print_ref`: t is {:?}", t);
}

fn main() {
    let x = 7;
    let ref_x = Ref(&x);

    print_ref(&ref_x);
    print(ref_x);
}
```

### Đọc thêm:

[generics][generics], [bounds trong generics][bounds], and
[kết hợp nhiều bounds trong generics][multibounds]

[generics]: ../../generics.md
[bounds]: ../../generics/bounds.md
[multibounds]: ../../generics/multi_bounds.md

[^†]:
    Lời người dịch:
    Đây là một ràng buộc về lifetime trong Rust, được áp dụng cho một kiểu T và một lifetime 'a.
    Nó đảm bảo rằng nếu T chứa bất kỳ tham chiếu nào, thì tham chiếu đó phải tồn tại trong lifetime của 'a.
