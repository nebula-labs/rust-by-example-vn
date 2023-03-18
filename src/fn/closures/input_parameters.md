# Sử dụng closure dưới dạng tham số đầu vào (input parameters)

Trong hầu hết các trường hợp, Rust compiler (trình biên dịch) có thể tự động hiểu được kiểu dữ liệu của biến
nhưng khi chúng ta định nghĩa các hàm thì chúng ta bắt buộc phải khai báo kiểu dữ liệu
của tham số và kiểu dữ liệu trả về một cách rõ ràng. Do đó, khi chúng ta viết một hàm và
truyền một closure (khối đóng) vào làm tham số đầu vào, thì Rust sẽ yêu cầu chúng ta phải chú thích
kiểu dữ liệu của closure đó bằng một trong số các trait được cung cấp dưới đây,
các trait này sẽ được xác định dựa trên cách mà closure sử dụng các captured variables (biến được bắt) trong nó.
Các trait mà chúng ta có thể sử dụng để chú thích kiểu dữ liệu của closure là:

- `Fn`: closure sử dụng các captured variables dưới dạng tham chiếu (&T).
- `FnMut`: closure sử dụng các captured variables dưới dạng tham chiếu có thể thay đổi được (&mut T).
- `FnOnce`: closure sử dụng các captured variables dưới dạng tham trị (T).

Các trait này được sắp xếp theo thứ tự giảm dần của mức độ hạn chế, tức là `Fn` là giới hạn ít chặt chẽ nhất và `FnOnce` là giới hạn chặt chẽ nhất.

Trên cơ sở từng biến một, rust compiler sẽ bắt giữ các biến theo cách ít hạn chế nhất có thể.

Giả sử một tham số closure được khai báo với kiểu là `FnOnce`, điều này có nghĩa là closure đó có thể bắt giữ (capture)[^†] các biến bằng tham chiếu `&T`, tham chiếu thay đổi được `&mut T` và tham trị `T`,
nhưng compiler sẽ lựa chọn cách bắt giữ các biến dựa vào cách mà các captured variables được sử dụng trong closure đó.

Trong ngôn ngữ rust, một khi ta có thể di chuyển (move) được ownership của một biến thì ta cũng có thể thực hiện các loại vay mượn (borrow) ownership khác đối với biến đó,
tuy nhiên điều ngược lại sẽ không đúng. Nếu một tham số closure được khai báo là `Fn` thì nó không thể bắt giữ biến bằng tham chiếu thay đổi được `&mut T` hoặc tham trị `T`
mà chỉ có thể bằng tham chiếu `&T`.

Ở ví dụ dưới đây, bạn hãy thử lần lượt thay đổi các kiểu khai báo `Fn`, `FnMut` và `FnOnce` đối với closure `F` để xem thử có điều gì xảy ra không nhé:

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

    // Bắt giữ 2 biến: `greeting` theo tham chiếu và
    // `farewell` theo tham trị.
    let diary = || {
        // `greeting` được bắt theo tham chiếu trong closure này nên yêu cầu khai báo bằng `Fn`.
        println!("I said {}.", greeting);

        // Ở đây có sự thay đổi giá trị của captured variable `farewell`,
        // nên `farewell` được closure này bắt theo tham chiếu thay đổi được.
        // Do đó closure phải được khai báo bằng `FnMut`.
        farewell.push_str("!!!");
        println!("Then I screamed {}.", farewell);
        println!("Now I can sleep. zzzzz");

        // Gọi hàm drop để bắt buộc closure này bắt biến `farewell`
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
    Việc bắt giữ một biến trong closure là một cách để lưu trữ giá trị của một biến trong phạm vi mà closure được khai báo
    và sử dụng biến đó. Khi closure được gọi, nó sẽ truy cập và sử dụng các giá trị được bắt giữ.
