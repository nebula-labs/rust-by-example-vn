# Phân biệt các traits được nạp chồng

Một kiểu có thể triển khai nhiều trait khác nhau. Vậy nếu hai trait đều yêu cầu cùng một tên phương thức thì sao? Ví dụ, nhiều trait có thể có một phương thức có tên `get()`. Chúng có thể có kiểu trả về khác nhau!

Tin tốt là: vì mỗi triển khai trait có một khối `impl` của riêng nó, nên bạn sẽ biết rõ rằng đang triển khai phương thức `get` của trait nào.

Còn khi bạn muốn _gọi_ các phương thức đó thì sao? Để phân biệt giữa chúng, chúng ta phải sử dụng cú pháp đầy đủ(Fully Qualified Syntax).

```rust,editable
trait UsernameWidget {
    // Lấy ra tên của người được chọn
    fn get(&self) -> String;
}

trait AgeWidget {
    // Lấy ra tuổi được chọn
    fn get(&self) -> u8;
}

// Một dạng có cả UsernameWidget và AgeWidget
struct Form {
    username: String,
    age: u8,
}

impl UsernameWidget for Form {
    fn get(&self) -> String {
        self.username.clone()
    }
}

impl AgeWidget for Form {
    fn get(&self) -> u8 {
        self.age
    }
}

fn main() {
    let form = Form {
        username: "rustacean".to_owned(),
        age: 28,
    };

    // Nếu bạn bỏ comment dòng này, bạn sẽ nhận được một lỗi nói rằng
    // "nhiều phương thức `get` được tìm thấy". Vì thế, sau tất cả những gì đã làm ở trên, chúng ta đang có nhiều phương thức có tên `get`.
    // println!("{}", form.get());

    let username = <Form as UsernameWidget>::get(&form);
    assert_eq!("rustacean".to_owned(), username);
    let age = <Form as AgeWidget>::get(&form);
    assert_eq!(28, age);
}
```

### Xem thêm:

[The Rust Programming Language chapter on Fully Qualified syntax][trpl_fqsyntax]

[trpl_fqsyntax]: https://doc.rust-lang.org/book/ch19-03-advanced-traits.html#fully-qualified-syntax-for-disambiguation-calling-methods-with-the-same-name
