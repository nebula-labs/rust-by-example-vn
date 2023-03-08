# Combinators: `map`

`match` là một phương thức xử lý hợp lệ của `Option`. Tuy nhiên, bạn có thể cảm thấy chán khi sử dụng chúng nhiều, đặc biệt là với các hoạt động chỉ đánh giá cho một đầu vào. Trong những trường hợp này, [combinators][combinators] có thể được sử dụng để quản lý luồng điều khiển một cách linh hoạt.

`Option` có sẵn một phương thức gọi là `map()`, một combinator cho việc đơn giản hóa việc ánh xạ `Some -> Some` và `None -> None`. Nhiều lần gọi `map()` có thể được nối với nhau để có tăng thêm tính linh hoạt.

Trong ví dụ sau, `process()` thay thế tất cả các hàm trước nó trong khi vẫn giữ nguyên tính gọn gàng.

```rust,editable
#![allow(dead_code)]

#[derive(Debug)] enum Food { Apple, Carrot, Potato }

#[derive(Debug)] struct Peeled(Food);
#[derive(Debug)] struct Chopped(Food);
#[derive(Debug)] struct Cooked(Food);

// Bóc vỏ đồ ăn. Nếu không có thì trả về `None`.
// Ngược lại, trả về đồ ăn đã được bóc vỏ.
fn peel(food: Option<Food>) -> Option<Peeled> {
    match food {
        Some(food) => Some(Peeled(food)),
        None       => None,
    }
}


// Cắt đồ ăn. Nếu không có thì trả về `None`.
// Ngược lại, trả về đồ ăn đã được cắt.
fn chop(peeled: Option<Peeled>) -> Option<Chopped> {
    match peeled {
        Some(Peeled(food)) => Some(Chopped(food)),
        None               => None,
    }
}


// Nấu đồ ăn. Ở đây, chúng ta dùng `map()` thay vì `match` cho các trường hợp xử lý.
fn cook(chopped: Option<Chopped>) -> Option<Cooked> {
    chopped.map(|Chopped(food)| Cooked(food))
}


// Một hàm để bóc vỏ, cắt và nấu đồ ăn cùng một lúc.
// Chúng ta nối nhiều lần sử dụng `map()` để đơn giản hóa code.
fn process(food: Option<Food>) -> Option<Cooked> {
    food.map(|f| Peeled(f))
        .map(|Peeled(f)| Chopped(f))
        .map(|Chopped(f)| Cooked(f))
}

// Kiểm tra xem liệu có đồ ăn hay không trước khi thử ăn nó!
fn eat(food: Option<Cooked>) {
    match food {
        Some(food) => println!("Mmm. I love {:?}", food),
        None       => println!("Oh no! It wasn't edible."),
    }
}

fn main() {
    let apple = Some(Food::Apple);
    let carrot = Some(Food::Carrot);
    let potato = None;

    let cooked_apple = cook(chop(peel(apple)));
    let cooked_carrot = cook(chop(peel(carrot)));
    // Bây giờ, hãy thử hàm `process()` đơn giản hơn.
    // Let's try the simpler looking `process()` now.
    let cooked_potato = process(potato);

    eat(cooked_apple);
    eat(cooked_carrot);
    eat(cooked_potato);
}
```

### Xem thêm:

[closures][closures], [`Option`][option], [`Option::map()`][map]

[combinators]: https://doc.rust-lang.org/reference/glossary.html#combinator
[closures]: ../../fn/closures.md
[option]: https://doc.rust-lang.org/std/option/enum.Option.html
[map]: https://doc.rust-lang.org/std/option/enum.Option.html#method.map
