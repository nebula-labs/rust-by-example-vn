# Phạm vi của cấu trúc

Các cấu trúc (struct) có một mức độ phạm vi truy cập được thêm vào đối với các trường của chúng. Phạm vi mặc định là riêng tư (private), và có thể được ghi đè bằng từ khóa `pub`. Phạm vi này chỉ được xem xét khi một cấu trúc được truy cập từ bên ngoài module mà nó được định nghĩa, và mục đích của nó là che thông tin (đóng gói) (encapsulation).

```rust,editable
mod my {
    // Một cấu trúc công khai với một trường công khai của kiểu chung (generic) `T`
    pub struct OpenBox<T> {
        pub contents: T,
    }

    // Một cấu trúc công khai với một trường riêng tư của kiểu chung (generic) `T`
    pub struct ClosedBox<T> {
        contents: T,
    }

    impl<T> ClosedBox<T> {
        // Một phương thức khởi tạo công khai
        pub fn new(contents: T) -> ClosedBox<T> {
            ClosedBox {
                contents: contents,
            }
        }
    }
}

fn main() {
    // Các cấu trúc công khai với các trường công khai có thể được khởi tạo một cách tự nhiên như sau
    let open_box = my::OpenBox { contents: "public information" };

    // và các trường của chúng có thể được truy cập một cách bình thường.
    println!("The open box contains: {}", open_box.contents);

    // Các cấu trúc công khai với các trường riêng tư không thể được khởi tạo bằng cách sử dụng tên trường.
    // Lỗi! `ClosedBox` có các trường riêng tư
    //let closed_box = my::ClosedBox { contents: "classified information" };
    // TODO ^ Hãy thử bỏ ghi chú của dòng trên này

    // Tuy nhiên, các cấu trúc với các trường riêng tư có thể được tạo bằng cách sử dụng
    // phương thức khởi tạo công khai
    let _closed_box = my::ClosedBox::new("classified information");

    // và các trường riêng tư của một cấu trúc dù có phạm vi công khai thì cũng không thể được truy cập.
    // Lỗi! Trường `contents` là riêng tư
    //println!("The closed box contains: {}", _closed_box.contents);
    // TODO ^ Hãy thử bỏ ghi chú của dòng trên này
}
```

### Xem thêm:

[generics][generics] và [methods][methods]

[generics]: ../generics.md
[methods]: ../fn/methods.md
