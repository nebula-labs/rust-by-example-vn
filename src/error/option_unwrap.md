# Option & unwrap

Trong ví dụ trước, ta thấy rằng ta có thể tạo ra lỗi chương trình theo ý muốn.
Ta đã cho chương trình `panic` nếu ta uống nước chanh có đường.
Nhưng nếu ta mong đợi một loại đồ uống nhưng lại không nhận được gì thì sao?
Trường hợp đó cũng tệ không kém, vì vậy nó cần phải được xử lý!

Chúng ta có thể kiểm tra điều này với empty string (`""`) giống như ta đã làm với nước chanh.
Vì ta đang dùng Rust, hãy để compiler chỉ ra các trường hợp không có đồ uống.

Một `enum` tên là `Option<T>` trong thư viện `std` được sử dụng trong trường hợp có thể không có phần tử kiểu T.
Nó có dạng một trong hai "tuỳ chọn":

* `Some(T)`: Một phần tử của kiểu `T` được tìm thấy
* `None`: Không tìm thấy phần tử nào.

Các trường hợp này có thể được xử lý một cách rõ ràng thông qua `match` hoặc một cách ngầm định thông qua
`unwrap`. Xử lý ngầm định sẽ trả về phần tử bên trong hoặc `panic`.

Lưu ý rằng có thể tùy chỉnh `panic` bằng cách sử dụng [expect][expect],
nhưng xử lý ngầm định (`unwrap`) sẽ trả lại cho ta một kết quả ít có ý nghĩa hơn so với việc xử lý rõ ràng.
Trong ví dụ sau đây, việc xử lý rõ ràng giúp ta kiểm soát kết quả tốt hơn,
trong khi vẫn giữ tùy chọn `panic` nếu cần.

```rust,editable,ignore,mdbook-runnable
// Người lớn (adult) thì cái gì cũng uống được.
// Mọi loại đồ uống đều được xử lý rõ ràng bằng cách sử dụng `match`.
fn give_adult(drink: Option<&str>) {
    // Mỗi đồ uống sẽ tương ứng với một hành động cụ thể.
    match drink {
        Some("lemonade") => println!("Yuck! Too sugary."),
        Some(inner)   => println!("{}? How nice.", inner),
        None          => println!("No drink? Oh well."),
    }
}

// Những người còn lại (không phải người lớn) sẽ `panic` khi uống một loại đồ uống có đường.
// Mọi loại đồ uống đều được xử lý ngầm định bằng cách sử dụng `unwrap`.
fn drink(drink: Option<&str>) {
    // `unwrap` trả lại `panic` khi nó nhận được `None`.
    let inside = drink.unwrap();
    if inside == "lemonade" { panic!("AAAaaaaa!!!!"); }

    println!("I love {}s!!!!!", inside);
}

fn main() {
    let water  = Some("water");
    let lemonade = Some("lemonade");
    let void  = None;

    give_adult(water);
    give_adult(lemonade);
    give_adult(void);

    let coffee = Some("coffee");
    let nothing = None;

    drink(coffee);
    drink(nothing);
}
```

[expect]: https://doc.rust-lang.org/std/option/enum.Option.html#method.expect