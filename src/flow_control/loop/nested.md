# Nesting and labels (Vòng lặp lồng nhau và nhãn)

Có thể dùng `break` và `continue` ở các vòng lặp bên ngoài (outer loops) trong khi 
xử lý vòng lặp bên trong (inner loops). Trong những trường hợp này, các vòng lặp phải được 
chú thích bằng nhãn `'label` và sau đó, các nhãn này phải được truyền phía sau 
lệnh `break`/`continue`.

```rust,editable
#![allow(unreachable_code)]

fn main() {
    'outer: loop {
        println!("Entered the outer loop");

        'inner: loop {
            println!("Entered the inner loop");

            // Lệnh break này sẽ chỉ break vòng lặp bên trong bởi vì nó không có nhãn
            //break;

            // Lệnh break này sẽ break vòng lặp ngoài bởi vì nó có nhãn `'outer`
            break 'outer;
        }

        println!("This point will never be reached");
    }

    println!("Exited the outer loop");
}
```