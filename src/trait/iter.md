# Iterators

Trait [`Iterator`][iter] được dùng để triển khai bộ lặp vào các tập hợp như là các mảng.


Trait chỉ yêu cầu một phương thức để xác định phần tử `next`,
thứ có thể được định nghĩa thủ công trong đoạn `impl` hoặc tự động
được định nghĩa (như trong các mảng và dải).

Là một sự tiện lợi trong nhiều tính huống, cấu trúc `for`
biến các tập hợp thành iterators bằng phương thức [`.into_iter()`][intoiter].

```rust,editable
struct Fibonacci {
    curr: u32,
    next: u32,
}

// Cài `Iterator` cho `Fibonacci`.
// `Iterator` trait chỉ yêu cầu một phương thức để xác định phần tử `next`.
impl Iterator for Fibonacci {
    // Chúng ta có thể chỉ đến loại này bằng cách dùng Self::Item
    type Item = u32;

    // Tại đây, chúng ta xác định thứ tự bằng cách dùng `.curr` và `.next`.
    // Kiểu trả về là `Option<T>`:
    //     * Khi `Iterator` kết thúc, biến thể `None` được trả về.
    //     * Ngược lại, giá trị tiếp theo sẽ được bọc trong biến thể `Some` và được trả về.
    // Chúng ta dùng Self::Item là kiểu trả về , vì thế chúng ta có thể thay đổi 
    // kiểu mà không cần cập nhật phương thức.
    fn next(&mut self) -> Option<Self::Item> {
        let current = self.curr;

        self.curr = self.next;
        self.next = current + self.next;

        // Vì không có điểm kết thúc của chuỗi Fibonacci, `Iterator` 
        // sẽ không bao giờ trả về biến thể `None`, và `Some` luôn được trả về.
        Some(current)
    }
}

// Trả về trình tạo chuỗi Fibonacci
fn fibonacci() -> Fibonacci {
    Fibonacci { curr: 0, next: 1 }
}

fn main() {
    // `0..3` là một `Iterator` mà tạo ra: 0, 1, and 2.
    let mut sequence = 0..3;

    println!("Four consecutive `next` calls on 0..3");
    println!("> {:?}", sequence.next());
    println!("> {:?}", sequence.next());
    println!("> {:?}", sequence.next());
    println!("> {:?}", sequence.next());

    // `for` chạy qua từng phần tử trong `Iterator` đến khi nó trả về `None`.
    // Mỗi giá trị `Some` được unwrapped được gán vào một biến  (ở đây là `i`).
    println!("Iterate through 0..3 using `for`");
    for i in 0..3 {
        println!("> {}", i);
    }

    // Phương thức `take(n)` giúp tạo `Iterator` rút gọn với chỉ `n` giá trị đầu tiên của nó.
    println!("The first four terms of the Fibonacci sequence are: ");
    for i in fibonacci().take(4) {
        println!("> {}", i);
    }

    // Phương thức `skip(n)` giúp tạo `Iterator` rút gọn bằng cách bỏ qua `n` giá trị đầu tiên của nó.
    println!("The next four terms of the Fibonacci sequence are: ");
    for i in fibonacci().skip(4).take(4) {
        println!("> {}", i);
    }

    let array = [1u32, 3, 3, 7];

    // Phương thức `iter` tạo ra một `Iterator` từ một chuỗi/lát cắt (array/slice).
    println!("Iterate the following array {:?}", &array);
    for i in array.iter() {
        println!("> {}", i);
    }
}
```

[intoiter]: https://doc.rust-lang.org/std/iter/trait.IntoIterator.html
[iter]: https://doc.rust-lang.org/core/iter/trait.Iterator.html