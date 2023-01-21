# if let

Trong một vài trường hợp sử dụng, khi match enum, `match` đôi khi rất rườm rà. Ví dụ:

```rust
// Biến `optional` có kiểu là `Option<i32>`
let optional = Some(7);

match optional {
    Some(i) => {
        println!("This is a really long string and `{:?}`", i);
        // ^ Thụt đầu dòng ở đây 2 lần để nhận biết việc 
        // tách `i` khỏi option.
    },
    _ => {},
    // ^ Dòng trên bắt buộc phải có, bởi `match` cần cover đầy đủ các trường 
    // hợp có thể có. Nhưng có vẻ nó khá lãng phí không gian ở đây? 
};

```

Sử dụng `if let` sẽ giúp xử lý trường hợp này sạch hơn, ngoài ra còn cho phép chỉ định nhiều loại
failure options:

```rust,editable
fn main() {
    // Tất cả đều có kiểu `Option<i32>`
    let number = Some(7);
    let letter: Option<i32> = None;
    let emoticon: Option<i32> = None;

    // Cấu trúc `if let` ở đây được dùng như sau: "if `let` sẽ destructure `number` thành
    // `Some(i)` mà không cần xét trường hợp `_`.
    if let Some(i) = number {
        println!("Matched {:?}!", i);
    }

    // Nếu bạn cần chỉ định một failure, hãy sử dụng else:
    if let Some(i) = letter {
        println!("Matched {:?}!", i);
    } else {
        // Destructure thất bại. Chuyển sang case failure.
        println!("Didn't match a number. Let's go with a letter!");
    }

    // Điều kiện dùng trong case failure.
    let i_like_letters = false;

    if let Some(i) = emoticon {
        println!("Matched {:?}!", i);
    // Destructure thất bại. Sử dụng điều kiện trong `else if` để xem nhánh 
    // failure thay thế nào nên được sử dụng
    } else if i_like_letters {
        println!("Didn't match a number. Let's go with a letter!");
    } else {
        // Điều kiện bằng false. Nhánh này được sử dụng:
        println!("I don't like letters. Let's go with an emoticon :)!");
    }
}
```

Theo một cách tương tự, không chỉ `Option`, `if let` có thể được sử dụng để match bất kỳ giá trị enum nào:

```rust,editable
// Enum ví dụ của chúng ta
enum Foo {
    Bar,
    Baz,
    Qux(u32)
}

fn main() {
    // Tạo một vài biến bất kỳ với enum bên trên
    let a = Foo::Bar;
    let b = Foo::Baz;
    let c = Foo::Qux(100);
    
    // Biến a match với Foo::Bar
    if let Foo::Bar = a {
        println!("a is foobar");
    }
    
    // Biến b không match với Foo::Bar
    // Nên ở đây không in ra cái gì cả
    if let Foo::Bar = b {
        println!("b is foobar");
    }
    
    // Biến c match với Foo:Qux với một giá trị 
    // Tương tự với Some() ở ví dụ trước
    if let Foo::Qux(value) = c {
        println!("c is {}", value);
    }

    // Binding cũng có thể làm việc trong `if let`
    if let Foo::Qux(value @ 100) = c {
        println!("c is one hundred");
    }
}
```

Một lợi ích khác là `if let` cho phép chúng ta thực hiện việc match với các biến thể enum mà không được tham số hóa (non-parameterized enum variants). Điều này đúng ngay cả trong trường hợp enum không implement hoặc derive trait `PartialEq`. Trong những trường hợp như vậy với `match`, `if Foo::Bar == a` sẽ bị lỗi khi biên dịch, vì các phiên bản của enum không thể đánh đồng được với nhau, tuy nhiên, `if let` sẽ tiếp tục hoạt động bình thường trong case này.

Ở đây có một bài tập cho bạn. Hãy thử sửa lỗi trong đoạn code bên dưới bằng cách sử dụng `if let`:

```rust,editable,ignore,mdbook-runnable
// Enum này được cố tình không implement cũng như derive trait PartialEq.
// Đó là lý do tại sao mà việc so sánh Foo::Bar == a bên dưới sẽ gây ra lỗi.
enum Foo {Bar}

fn main() {
    let a = Foo::Bar;

    // Biến a match với Foo::Bar
    if Foo::Bar == a {
    // ^-- Điều này sẽ sinh ra lỗi biên dịch. Hãy sử dụng `if let`.
        println!("a is foobar");
    }
}
```

### Xem thêm tại đây:

[`enum`][enum], [`Option`][option], và [if let RFC][if_let_rfc]

[enum]: ../custom_types/enum.md
[if_let_rfc]: https://github.com/rust-lang/rfcs/pull/160
[option]: ../std/option.md
