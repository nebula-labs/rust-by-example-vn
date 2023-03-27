# Borrowing
Trong đa phần các trường hợp, chúng ta sẽ có thể muốn truy cập dữ liệu mà không cần phải quan tâm nhiều đến quyền sở hữu của nó. Để làm được điều này, Rust cung cấp cho chúng ta một cơ chế gọi là `borrowing` - Mượn trong tiếng Việt. Điều này có nghĩa là thay vì truyền vào giá trị `(T)`, ta chỉ cần truyền vào tham chiếu của nó `(&T)`. <br/>
Trình biên dịch sẽ đảm bảo chắc chắn (thông qua **trình kiểm tra mượn** - `borrow checker`) rằng các tham chiếu sẽ luôn được trỏ đến các đối tượng hợp lệ. Điều này có nghĩa là, trong khi các tham chiếu đến một đối tượng còn tồn tại, thì đối tượng đó không thể bị xóa bỏ. <br/>

```rust,editable
// Function này sẽ lấy ownership của Box và hủy nó
fn eat_box_i32(boxed_i32: Box<i32>) {
    println!("Destroying box that contains {}", boxed_i32);
}

// Function này sẽ borrow một giá trị i32
fn borrow_i32(borrowed_i32: &i32) {
    println!("This int is: {}", borrowed_i32);
}

fn main() {
    // Tạo một Box i32, và một stacked i32
    let boxed_i32 = Box::new(5_i32);
    let stacked_i32 = 6_i32;

    // Mượn nội dung của Box. Ownership sẽ không bị thay đổi,
    // nên nội dung có thể được mượn nhiều lần.
    borrow_i32(&boxed_i32);
    borrow_i32(&stacked_i32);

    {
        // Tham chiếu dữ liệu chứa bên trong Box
        let _ref_to_i32: &i32 = &boxed_i32;

        // Lỗi!
        // Không thể hủy `boxed_i32` do giá trị bên trong nó sẽ được mượn ở phía sau trong cùng 1 scope.
        eat_box_i32(boxed_i32);
        // FIXME ^ Comment dòng này để fix lỗi

        // Thử mượn `_ref_to_i32` khi giá trị bên trong nó đã bị hủy
        borrow_i32(_ref_to_i32);
        // `_ref_to_i32` đã đi ra khỏi scope và không được mượn nữa
    }

    // `boxed_i32` có thể từ bỏ quyền sở hữu - ownership đối với `eat_box` và có thể được tiêu hủy
    eat_box_i32(boxed_i32);
}
```