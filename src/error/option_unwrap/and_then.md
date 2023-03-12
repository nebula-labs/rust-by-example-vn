# Combinators: `and_then`

`map()` được mô tả là một cách có thể xâu chuỗi để đơn giản hóa các câu lệnh `match`. Tuy nhiên, việc sử dụng `map()` trên một hàm trả về là `Option<T>` sẽ dẫn đến kết quả là `Option<Option<T>>` lồng nhau. Chuỗi nhiều các hàm gọi với nhau có thể trở nên khó hiểu. Đó là lúc một bộ kết hợp khác được gọi là `and_then()`, được biết đến trong một số ngôn ngữ là flatmap xuất hiện.

`and_then()` gọi đầu vào hàm của nó với giá trị được bọc(wrap) và trả về kết quả. Nếu `Option` là `None`, thì thay vào đó, nó sẽ trả về `None`.

Trong ví dụ sau, `cookable_v2()` trả về một kết quả là `Option<Food>`. Sử dụng `map()` thay vì `and_then()` sẽ đưa ra `Option<Option<Food>>`, đây là loại không hợp lệ cho `eat()`.

```rust,editable
#![allow(dead_code)]

#[derive(Debug)] enum Food { CordonBleu, Steak, Sushi }
#[derive(Debug)] enum Day { Monday, Tuesday, Wednesday }

// Chúng tôi không có nguyên liệu để làm Sushi.
fn have_ingredients(food: Food) -> Option<Food> {
    match food {
        Food::Sushi => None,
        _           => Some(food),
    }
}

// Chúng tôi có công thức cho mọi thứ trừ Cordon Bleu.
fn have_recipe(food: Food) -> Option<Food> {
    match food {
        Food::CordonBleu => None,
        _                => Some(food),
    }
}

// Để làm một món ăn, chúng ta cần cả công thức và nguyên liệu.
// Chúng ta có thể biểu diễn logic bằng một chuỗi matches:
fn cookable_v1(food: Food) -> Option<Food> {
    match have_recipe(food) {
        None       => None,
        Some(food) => have_ingredients(food),
    }
}

// Điều này có thể được viết lại một cách tiện lợi hơn với `and_then()`:
fn cookable_v2(food: Food) -> Option<Food> {
    have_recipe(food).and_then(have_ingredients)
}

fn eat(food: Food, day: Day) {
    match cookable_v2(food) {
        Some(food) => println!("Yay! On {:?} we get to eat {:?}.", day, food),
        None       => println!("Oh no. We don't get to eat on {:?}?", day),
    }
}

fn main() {
    let (cordon_bleu, steak, sushi) = (Food::CordonBleu, Food::Steak, Food::Sushi);

    eat(cordon_bleu, Day::Monday);
    eat(steak, Day::Tuesday);
    eat(sushi, Day::Wednesday);
}
```

### See also:

[closures][closures], [`Option`][option], and [`Option::and_then()`][and_then]

[closures]: ../../fn/closures.md
[option]: https://doc.rust-lang.org/std/option/enum.Option.html
[and_then]: https://doc.rust-lang.org/std/option/enum.Option.html#method.and_then
