# Partial moves

Trong quá trình [destructuring] (giải cấu trúc) của một biến, cả hai mẫu ràng buộc
`theo cách di chuyển` và `theo tham chiếu` có thể được sử dụng đồng thời.
Điều này sẽ dẫn đến việc _di chuyển một phần_ của biến, có nghĩa
là một số phần của biến sẽ được di chuyển trong khi các phần khác vẫn được giữ nguyên
chỗ. Trong trường hợp như vậy, biến cha không thể được sử dụng sau đó như một
toàn thể, tuy nhiên các phần chỉ được tham chiếu (và không được di chuyển) vẫn có thể được sử dụng.

```rust,editable
fn main() {
    #[derive(Debug)]
    struct Person {
        name: String,
        age: Box<u8>,
    }

    let person = Person {
        name: String::from("Alice"),
        age: Box::new(20),
    };

    // `name` được di chuyển ra khỏi `person`, nhưng `age` được tham chiếu
    let Person { name, ref age } = person;

    println!("Tuổi của người đó là  {}", age);

    println!("Tên của người đó là  {}", name);

    // Lỗi! không được mượn một phần giá trị đã di chuyển: `person` bị di chuyển một phần
    //println!("Cấu trúc person là {:?}", person);

    // không thể sử dụng `person` nhưng có thể sử dụng
    // `person.age` vì nó không được di chuyển
    println!("Tuổi của người đó từ cấu trúc person là {}", person.age);
}
```
(Trong ví dụ này, chúng ta lưu trữ biến `age` trên heap để minh họa về di
chuyển một phần: nếu xóa `ref` trong đoạn mã trên, sẽ xảy ra lỗi vì quyền
sở hữu của `person.age` đã được di chuyển đến biến `age`. Nếu `Person.age` được
lưu trữ trên stack, không cần phải sử dụng `ref` vì định nghĩa của `age` sẽ
sao chép dữ liệu từ `person.age` mà không cần di chuyển nó.)

### Xem thêm:
[destructuring][destructuring]

[destructuring]: ../../flow_control/match/destructuring.md