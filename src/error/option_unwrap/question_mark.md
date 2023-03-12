# Unpacking options with `?`

Bạn có thể unpack `Option` bằng cách sử dụng câu lệnh `match`, nhưng thường thì sử dụng toán tử `?` sẽ dễ dàng hơn. Nếu `x` là một `Option`, thì khi đánh giá `x?` sẽ trả về giá trị bên trong nếu `x` là `Some`, ngược lại nó sẽ kết thúc các hàm đang được thực thi và trả về `None`.

```rust,editable
fn next_birthday(current_age: Option<u8>) -> Option<String> {
    // Nếu `current_age` là `None`, hàm này sẽ trả về `None`.
    // Nếu `current_age` là `Some`, giá trị `u8` ở trong sẽ được gán cho `next_age`
    let next_age: u8 = current_age? + 1;
    Some(format!("Next year I will be {}", next_age))
}
```

Bạn có thể nối tiếp nhiều `?` lại với nhau để làm cho mã của bạn trở nên dễ đọc hơn.

```rust,editable
struct Person {
    job: Option<Job>,
}

#[derive(Clone, Copy)]
struct Job {
    phone_number: Option<PhoneNumber>,
}

#[derive(Clone, Copy)]
struct PhoneNumber {
    area_code: Option<u8>,
    number: u32,
}

impl Person {

    // Trả về mã vùng của số điện thoại công việc của người này, nếu nó tồn tại.
    fn work_phone_area_code(&self) -> Option<u8> {
        // Việc này sẽ cần nhiều câu lệnh `match` lồng nhau nếu không sử dụng toán tử `?`.
        // Bạn sẽ cần viết nhiều mã hơn - hãy thử viết lại nó và xem cái nào dễ hơn.
        self.job?.phone_number?.area_code
    }
}

fn main() {
    let p = Person {
        job: Some(Job {
            phone_number: Some(PhoneNumber {
                area_code: Some(61),
                number: 439222222,
            }),
        }),
    };

    assert_eq!(p.work_phone_area_code(), Some(61));
}
```
