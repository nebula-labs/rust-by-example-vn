# while

Từ khóa `while` có thể được sử dụng để chạy một vòng lặp với một điều kiện, 
vòng lặp sẽ không kết thúc cho đến khi điều kiện trở thành sai.

Hãy thử viết trò chơi [FizzBuzz][fizzbuzz] nổi tiếng bằng cách sử dụng vòng lặp `while`.

```rust,editable
fn main() {
    // Biến đếm
    let mut n = 1;

    // Vòng lặp white với điều kiện `n` nhỏ hơn 101
    while n < 101 {
        if n % 15 == 0 {
            println!("fizzbuzz");
        } else if n % 3 == 0 {
            println!("fizz");
        } else if n % 5 == 0 {
            println!("buzz");
        } else {
            println!("{}", n);
        }

        // Tăng biến đếm lên 1 
        n += 1;
    }
}
```

[fizzbuzz]: https://en.wikipedia.org/wiki/Fizz_buzz

