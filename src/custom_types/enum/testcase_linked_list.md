# Testcase: linked-list (Danh sách liên kết)

Một cách thông dụng để triển khai một danh sách liên kết trong Rust là thông qua `enums`:

```rust,editable
use crate::List::*;

enum List {
    // Cons: Là một tuple struct mà nó bọc một phần tử và một con trỏ để trỏ 
    // tới nút tiếp theo.
    Cons(u32, Box<List>),
    // Nil: Một nút rỗng biểu thị sự kết thúc của linked list 
    Nil,
}

// Triển khai các phương thức cho enum trên
impl List {
    // Tạo một danh sách trống
    fn new() -> List {
        // `Nil` là một biến thể trong `List`
        Nil
    }

    // Hàm này dùng một danh sách và trả về cũng danh sách đó nhưng thêm một 
    // phần tử mới ở đầu
    fn prepend(self, elem: u32) -> List {
        // `Cons` cũng là một biến thể trong List
        Cons(elem, Box::new(self))
    }

    // Trả về độ dài của danh sách
    fn len(&self) -> u32 {
        // `self` phải được đối sánh (match) ở đây, bởi vì hành vi của phương thức này
        // phụ thuộc vào biến thể của `self`.
        // `self` có kiểu `&List`, `*self` có kiểu `List`, đối sánh trên một kiểu cụ thể 
        // `T` được khuyên dùng hơn là đối sánh trên tham chiếu `&T`.
        // Tuy nhiên sau Rust 2018 (phiên bản 1.26 trở đi), bạn có thể sử dụng `self` 
        // và tail (không cần ref), Rust sẽ tự động suy diễn ra &s và ref tail cho bạn.
        // Xem tại https://doc.rust-lang.org/edition-guide/rust-2018/ownership-and-lifetimes/default-match-bindings.html
        match *self {
            // Không lấy được quyền sở hữu của tail, bởi `self` chỉ là đi mượn,
            // nên thay vào đó, ta dùng một tham chiếu tới tail.
            Cons(_, ref tail) => 1 + tail.len(),
            // Trường hợp gốc: Một danh sách trống thì có độ dài = 0
            Nil => 0
        }
        /*
        Với Rust 1.26 trở đi, bạn có thể viết code ngắn gọn và thân thiện hơn như sau:
        match self {
            Cons(_, ref tail) => 1 + tail.len(),
            Nil => 0
        }
        */
    }

    // Trả về biểu diễn của danh sách dưới dạng một String phân bổ trong bộ nhớ heap
    fn stringify(&self) -> String {
        match *self {
            Cons(head, ref tail) => {
                // `format!` tương tự như `print!`, nhưng nó trả về một String 
                // được phân bổ bộ nhớ heap thay vì in dữ liệu ra màn hình.
                format!("{}, {}", head, tail.stringify())
            },
            Nil => {
                format!("Nil")
            },
        }
        /*
        Với Rust 1.26 trở đi:
        match self {
            Cons(head, tail) => {
                format!("{}, {}", head, tail.stringify())
            },
            Nil => {
                format!("Nil")
            },
        }
        */
    }
}

fn main() {
    // Tạo một linked-list trống
    let mut list = List::new();

    // Thêm một số nút phần tử
    list = list.prepend(1);
    list = list.prepend(2);
    list = list.prepend(3);

    // In ra trạng thái cuối cùng của danh sách
    println!("linked list has length: {}", list.len());
    println!("{}", list.stringify());
}
```

### Xem thêm tại đây:

[`Box`][box] và [methods][methods]

[box]: ../../std/box.md
[methods]: ../../fn/methods.md

