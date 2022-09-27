# while

Từ khóa `while` có thể được sử dụng để chạy một vòng lặp trong khi điều kiện còn đúng.

Hãy thử viết trò chơi [FizzBuzz][fizzbuzz] nổi tiếng bằng cách sử dụng vòng lặp `while`.

```rust,editable
fn main() {
    // A counter variable
    let mut n = 1;

    // Loop while `n` is less than 101
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

        // Increment counter
        n += 1;
    }
}
```

[fizzbuzz]: https://en.wikipedia.org/wiki/Fizz_buzz

