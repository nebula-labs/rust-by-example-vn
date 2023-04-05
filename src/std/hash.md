# HashMap

Trong khi vector lưu trữ giá trị bằng các chỉ mục số nguyên, thì `HashMap` lưu trữ giá trị bằng key. Key của `HashMap` có thể là booleans, integers, strings, hoặc bất kì kiểu dữ liệu nào khác mà được triển khai trait `Eq` và `Hash`. Chi tiết hơn về điều này sẽ được đề cập ở phần tiếp theo.

Tương tự như vector, `HashMap` có thể mở rộng được, nhưng HashMaps cũng có thể thu nhỏ chính chúng khi chúng có quá nhiều không gian lưu trữ dư thừa. Bạn có thể tạo một HashMap với một dung lượng ban đầu nhất định bằng cách sử dụng `HashMap::with_capacity(uint)`, hoặc sử dụng `HashMap::new()` để tạo một HashMap với dung lượng khởi tạo mặc định(được đề xuất).

```rust,editable
use std::collections::HashMap;

fn call(number: &str) -> &str {
    match number {
        "798-1364" => "We're sorry, the call cannot be completed as dialed. 
            Please hang up and try again.",
        "645-7689" => "Hello, this is Mr. Awesome's Pizza. My name is Fred.
            What can I get for you today?",
        _ => "Hi! Who is this again?"
    }
}

fn main() { 
    let mut contacts = HashMap::new();

    contacts.insert("Daniel", "798-1364");
    contacts.insert("Ashley", "645-7689");
    contacts.insert("Katie", "435-8291");
    contacts.insert("Robert", "956-1745");

    // Lấy một tham chiếu và trả về Option<&V>
    match contacts.get(&"Daniel") {
        Some(&number) => println!("Calling Daniel: {}", call(number)),
        _ => println!("Don't have Daniel's number."),
    }

    // `HashMap::insert()` trả về `None`
    // nếu giá trị được thêm là mới, ngược lại trả về là `Some(value)`
    contacts.insert("Daniel", "164-6743");

    match contacts.get(&"Ashley") {
        Some(&number) => println!("Calling Ashley: {}", call(number)),
        _ => println!("Don't have Ashley's number."),
    }

    contacts.remove(&"Ashley"); 

    // `HashMap::iter()` trả về một bộ lặp đưa ra các cặp 
    // (&'a key, &'a value) theo thứ tự bất kỳ.
    for (contact, &number) in contacts.iter() {
        println!("Calling {}: {}", contact, call(number)); 
    }
}
```

Để biết thêm thông tin về hashing và hash maps (đôi khi được gọi là hash tables), hãy xem [Hash Table Wikipedia][wiki-hash]

[wiki-hash]: https://en.wikipedia.org/wiki/Hash_table