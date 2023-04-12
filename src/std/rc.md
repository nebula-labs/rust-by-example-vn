# `Rc`

Khi ta cần có nhiều ownership, `Rc` (Reference Counting - bộ đếm tham chiếu) có thể được sử dụng. `Rc` sẽ theo dõi số lượng các tham chiếu, có nghĩa là số lượng các owners của một giá trị sẽ được chứa bên trong một `Rc`.

Số lượng tham chiếu của một `Rc` sẽ tăng lên 1 bất cứ khi nào `Rc` đó bị sao chép (cloned), và giảm đi 1 khi có một bản sao của `Rc` bị ra khỏi scope. Tại thời điểm mà số lượng tham chiếu của `Rc` trở về 0 (nghĩa là không còn owners nào), cả `Rc` và giá trị được tham chiếu sẽ bị huỷ.

Khi sao chép một `Rc` sẽ không xảy ra deep copy. Nó chỉ tạo ra thêm một con trỏ khác đến giá trị được chứa bên trong và tăng thêm số lượng tham chiếu.

```rust,editable
use std::rc::Rc;

fn main() {
    let rc_examples = "Rc examples".to_string();
    {
        println!("--- rc_a is created ---");

        let rc_a: Rc<String> = Rc::new(rc_examples);
        println!("Reference Count of rc_a: {}", Rc::strong_count(&rc_a));

        {
            println!("--- rc_a is cloned to rc_b ---");

            let rc_b: Rc<String> = Rc::clone(&rc_a);
            println!("Reference Count of rc_b: {}", Rc::strong_count(&rc_b));
            println!("Reference Count of rc_a: {}", Rc::strong_count(&rc_a));

            // Hai `Rc` bằng nhau khi và chỉ khi các giá trị bên trong bằng nhau
            println!("rc_a and rc_b are equal: {}", rc_a.eq(&rc_b));

            // Chúng ta có thể sử dụng các methods của giá trị bên trong một cách trực tiếp
            println!("Length of the value inside rc_a: {}", rc_a.len());
            println!("Value of rc_b: {}", rc_b);

            println!("--- rc_b is dropped out of scope ---");
        }

        println!("Reference Count of rc_a: {}", Rc::strong_count(&rc_a));

        println!("--- rc_a is dropped out of scope ---");
    }

    // Lỗi! `rc_examples` đã bị moved đến `rc_a`
    // Và sau đó khi `rc_a` bị huỷ, `rc_examples` cũng bị huỷ theo
    // println!("rc_examples: {}", rc_examples);
    // TODO ^ Hãy thử bỏ chú thích khỏi dòng này
}
```

### Đọc thêm:

[std::rc][1] and [std::sync::arc][2].

[1]: https://doc.rust-lang.org/std/rc/index.html
[2]: https://doc.rust-lang.org/std/sync/struct.Arc.html
