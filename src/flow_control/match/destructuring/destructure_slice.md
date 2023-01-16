# arrays/slices

Cũng giống như tuple, các array và slice cũng có thể được phân tách bằng `match`:

```rust,editable
fn main() {
    // Hãy thử thay đổi giá trị trong array, hoặc biến nó thành một slice!
    let array = [1, -2, 6];

    match array {
        // Gán phần tử thứ 2 và thứ 3 cho các biến tương ứng
        [0, second, third] =>
            println!("array[0] = 0, array[1] = {}, array[2] = {}", second, third),

        // Các giá trị đơn lẻ có thể được bỏ qua bằng _
        [1, _, third] => println!(
            "array[0] = 1, array[2] = {} and array[1] was ignored",
            third
        ),

        // Bạn cũng có thể gán một vài thứ và bỏ qua phần còn lại
        [-1, second, ..] => println!(
            "array[0] = -1, array[1] = {} and all the other ones were ignored",
            second
        ),
        
        // Dòng này sẽ không được biên dịch
        // [-1, second] => ...

        // Hoặc cũng có thể lưu chúng trong một array/slice khác (kiểu của nó phụ thuộc 
        // vào giá trị được match)
        [3, second, tail @ ..] => println!(
            "array[0] = 3, array[1] = {} and the other elements were {:?}",
            second, tail
        ),

        // Kết hợp các patterns nói ở trên, ở đây chúng ta có thể gán giá trị của 
        // phần tử thứ nhất và thứ hai, và lưu phần còn lại trong một array đơn
        [first, middle @ .., last] => println!(
            "array[0] = {}, middle = {:?}, array[2] = {}",
            first, middle, last
        ),
    }
}
```

### Xem thêm tại đây:

[Arrays và Slices](../../../primitives/array.md) và `@` trong [Binding](../binding.md) 
