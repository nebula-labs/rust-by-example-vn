# Sử dụng closure dưới dạng tham số đầu vào (input parameters)

Mặc dù, Rust sẽ tự chọn cách capture các biến trong quá trình biên dịch mà không cần chúng ta phải khai báo kiểu tường minh,
nhưng Rust sẽ không chấp nhận sự không rõ ràng này khi chúng ta định nghĩa hàm. Do đó, khi chúng ta viết một hàm và
truyền một closure (khối đóng) vào làm tham số đầu vào, thì kiểu dữ liệu hoàn chỉnh của closure đó phải được kí hiệu bằng cách sử dụng một trong
một vài trait sau đây, các trait này sẽ được xác định dựa trên cách mà closure sử dụng các giá trị mà nó capture được.
Theo mức độ thứ tự giảm dần của mức độ hạn chế các trait đó lần lượt là:

- `Fn`: closure sử dụng các giá trị mà nó capture được dưới dạng tham chiếu (&T).
- `FnMut`: closure sử dụng các giá trị mà nó capture được dưới dạng tham chiếu có thể thay đổi được (&mut T).
- `FnOnce`: closure sử dụng các giá trị mà nó capture được dưới dạng tham trị (T).

Trên cơ sở từng biến một, rust compiler sẽ capture các biến theo cách ít hạn chế nhất có thể.

Giả sử một tham số closure được khai báo với kiểu là `FnOnce`, điều này có nghĩa là closure đó có thể capture[^†] các biến bằng tham chiếu `&T`, tham chiếu thay đổi được `&mut T` và tham trị `T`,
nhưng compiler sẽ lựa chọn cách capture các biến dựa vào cách mà các captured variables được sử dụng trong closure đó.

Điều này có thể xảy ra bởi vì một khi ta có thể di chuyển (move) một biến thì ta cũng có thể thực hiện các loại vay mượn (borrow) khác đối với biến đó,
tuy nhiên điều ngược lại sẽ không đúng. Nếu một tham số được khai báo là `Fn` thì việc capture biến bằng tham chiếu thay đổi được `&mut T` hoặc tham trị `T` là không thể.
Tuy nhiên, `&T` được phép sử dụng trong trường hợp này.

Ở ví dụ dưới đây, bạn hãy thử lần lượt thay đổi các kiểu khai báo `Fn`, `FnMut` và `FnOnce` để xem thử có điều gì xảy ra không nhé:

```rust,editable
// Đây là hàm nhận tham số truyền vào là một closure và thực thi closure đó.
// <F> là kí hiệu rằng F là một tham số có kiểu generic
fn apply<F>(f: F) where
    // Closure này không có input và cũng không trả về gì cả
    F: FnOnce() {
    // ^ TODO: Hãy thử thay `FnOnce` thành `Fn` hoặc `FnMut`.

    f();
}

// Đây là hàm nhận tham số truyền vào là một closure và trả về `i32`.
fn apply_to_3<F>(f: F) -> i32 where
    // Closure này nhận vào một `i32` và trả về một `i32`.
    F: Fn(i32) -> i32 {

    f(3)
}

fn main() {
    use std::mem;

    let greeting = "hello";
    // Một kiểu dữ liệu không thể sao chép.
    // Phương thức `to_owned` tạo dữ liệu được sở hữu bởi biến farewell từ dữ liệu được mượn
    let mut farewell = "goodbye".to_owned();

    // Capture 2 biến: `greeting` theo tham chiếu và
    // `farewell` theo tham trị.
    let diary = || {
        // `greeting` được capture theo tham chiếu trong closure này nên yêu cầu khai báo bằng `Fn`.
        println!("I said {}.", greeting);

        // Ở đây có sự thay đổi giá trị của captured variable `farewell`,
        // nên `farewell` được closure này capture theo tham chiếu thay đổi được.
        // Do đó closure phải được khai báo bằng `FnMut`.
        farewell.push_str("!!!");
        println!("Then I screamed {}.", farewell);
        println!("Now I can sleep. zzzzz");

        // Gọi hàm drop để bắt buộc closure này capture biến `farewell`
        // bằng tham trị. Do đó closure phải được khai báo bằng `FnOnce`.
        mem::drop(farewell);
    };

    // Gọi hàm có chức năng thực thi closure truyền vào.
    apply(diary);

    // `double` đáp ứng điều kiện ràng buộc về trait của `apply_to_3`.
    let double = |x| 2 * x;

    println!("3 doubled: {}", apply_to_3(double));
}
```

### See also:

[`std::mem::drop`][drop], [`Fn`][fn], [`FnMut`][fnmut], [Generics][generics], [where][where] and [`FnOnce`][fnonce]

[drop]: https://doc.rust-lang.org/std/mem/fn.drop.html
[fn]: https://doc.rust-lang.org/std/ops/trait.Fn.html
[fnmut]: https://doc.rust-lang.org/std/ops/trait.FnMut.html
[fnonce]: https://doc.rust-lang.org/std/ops/trait.FnOnce.html
[generics]: ../../generics.md
[where]: ../../generics/where.md

[^†]:
    Chú thích của người dịch: "Việc capture một biến trong closure là một cách để lưu trữ giá trị của một biến trong phạm vi mà closure được khai báo
    và sử dụng biến đó. Khi closure được gọi, nó sẽ truy cập và sử dụng các giá trị được capture."
