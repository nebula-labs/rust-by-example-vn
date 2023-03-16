# Higher Order Functions

Rust cung cấp Higher Order Functions (HOF - Hàm bậc cao). HOF là function nhận
vào một hay nhiều function và/hoặc trả về kết quả là một function. Các HOF và
các vòng lặp lười (lazy iterator) thêm gia vị cho các tính năng của Rust.

```rust,editable
fn is_odd(n: u32) -> bool {
    n % 2 == 1
}

fn main() {
    println!("Find the sum of all the squared odd numbers under 1000");
    let upper = 1000;

    // Cách tiếp cận mệnh lệnh (imperative approach)
    // Khai báo một biến tích lũy
    let mut acc = 0;
    // Lặp: 0, 1, 2, ... tới vô tận
    for n in 0.. {
        // Bình phương số n
        let n_squared = n * n;

        if n_squared >= upper {
            // Thoát khỏi vòng lặp nếu đạt tới giới hạn trên
            break;
        } else if is_odd(n_squared) {
            // Nếu n là số lẻ, cộng dồn n vào biến acc
            acc += n_squared;
        }
    }
    println!("imperative style: {}", acc);

    // Cách tiếp cận function (functional approach)
    let sum_of_squared_odd_numbers: u32 =
        (0..).map(|n| n * n)                             // Bình phương tất cả các số tự nhiên
             .take_while(|&n_squared| n_squared < upper) // Nhỏ hơn giới hạn
             .filter(|&n_squared| is_odd(n_squared))     // Là số lẻ
             .sum();                                     // Và tính tổng
    println!("functional style: {}", sum_of_squared_odd_numbers);
}
```

[Option][option]
và
[Iterator][iter]
đều triển khai HOF.

[option]: https://doc.rust-lang.org/core/option/enum.Option.html
[iter]: https://doc.rust-lang.org/core/iter/trait.Iterator.html