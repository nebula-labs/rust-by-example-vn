# for loops

## for và range

Cấu trúc `for in` có thể được sử dụng để chạy một vòng lặp thông qua một `Iterator`. Một trong những cách dễ nhất để tạo một iterator là sử dụng ký hiệu phạm vi (range notation) là `a..b`. Điều này sẽ giúp iterator lướt qua các giá trị từ `a` (bao gồm cả `a`) đến `b` (không bao gồm `b`) trong mỗi một bước lặp.

Hãy thử viết FizzBuzz bằng `for` thay cho `while`.

```rust,editable
fn main() {
    // `n` sẽ lấy các giá trị: 1, 2, ..., 100 tại mỗi lần lặp
    for n in 1..101 {
        if n % 15 == 0 {
            println!("fizzbuzz");
        } else if n % 3 == 0 {
            println!("fizz");
        } else if n % 5 == 0 {
            println!("buzz");
        } else {
            println!("{}", n);
        }
    }
}
```

Ngoài ra, `a..=b` có thể được sử dụng cho một phạm vi mà nó bao gồm cả hai điểm đầu và cuối. Phần code bên trên có thể được viết lại là:

```rust,editable
fn main() {
    // `n` sẽ lấy các giá trị: 1, 2, ..., 100 tại mỗi lần lặp
    for n in 1..=100 {
        if n % 15 == 0 {
            println!("fizzbuzz");
        } else if n % 3 == 0 {
            println!("fizz");
        } else if n % 5 == 0 {
            println!("buzz");
        } else {
            println!("{}", n);
        }
    }
}
```

## for và iterators

Cấu trúc `for in` có thể tương tác với một `Iterator` theo một số cách. 
Như đã thảo luận trong phần về trait [Iterator][iter], theo mặc định, vòng lặp `for` sẽ 
áp dụng hàm `into_iter` cho collection. Tuy nhiên, đây không phải là phương tiện duy nhất để chuyển đổi collection thành iterator.

`into_iter`, `iter` và `iter_mut` đều xử lý việc chuyển đổi một collection thành iterator theo những cách khác nhau, bằng cách cung cấp các chế độ xem khác nhau về dữ liệu bên trong.

* `iter` - Nó sẽ mượn từng phần tử của collection qua mỗi lần lặp lại. Do đó collection sẽ không bị ảnh hưởng và có sẵn để sử dụng lại sau vòng lặp.

```rust,editable
fn main() {
    let names = vec!["Bob", "Frank", "Ferris"];

    for name in names.iter() {
        match name {
            &"Ferris" => println!("There is a rustacean among us!"),
            // TODO ^ Hãy thử xoá & và chỉ matching với "Ferris"
            _ => println!("Hello {}", name),
        }
    }
    
    println!("names: {:?}", names);
}
```

* `into_iter` - Nó sẽ sử dụng collection để trên mỗi lần lặp lại, dữ liệu chính xác được cung cấp. Một khi collection đã được sử dụng, nó không còn có sẵn để sử dụng lại vì nó đã được 'moved' trong vòng lặp.

```rust,editable,ignore,mdbook-runnable
fn main() {
    let names = vec!["Bob", "Frank", "Ferris"];

    for name in names.into_iter() {
        match name {
            "Ferris" => println!("There is a rustacean among us!"),
            _ => println!("Hello {}", name),
        }
    }
    
    println!("names: {:?}", names);
    // FIXME ^ Comment dòng trên lại
}
```

* `iter_mut` - Nó có thể vay mượn từng phần tử của collection, cho phép collection được sửa đổi tại chỗ.

```rust,editable
fn main() {
    let mut names = vec!["Bob", "Frank", "Ferris"];

    for name in names.iter_mut() {
        *name = match name {
            &mut "Ferris" => "There is a rustacean among us!",
            _ => "Hello",
        }
    }

    println!("names: {:?}", names);
}
```

Trong các đoạn mã trên, hãy lưu ý đến `match`, đó là sự khác biệt chính trong các loại lặp. 
Sự khác biệt về loại tất nhiên ngụ ý dẫn đến các hành động khác nhau có thể được thực hiện.

### Xem thêm tại đây:

[Iterator][iter]

[iter]: ../trait/iter.md

