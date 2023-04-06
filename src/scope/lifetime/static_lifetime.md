# Static

Trong Rust có một vài tên lifetime đặc biệt. Một trong số đó là `'static`. 
Bạn có thể gặp nó trong hai tình huống:

```rust, editable
// Một tham chiếu với 'static lifetime:
let s: &'static str = "hello world";

// 'static được sử dụng làm phần của trait bound:
fn generic<T>(x: T) where T: 'static {}
```

Cả hai trường hợp đều liên quan nhưng khác nhau một chút và đây chính là một nguồn cơn phổ biến gây ra vài sự nhẫm lẫn khi học Rust.  
Dưới đây là một số ví dụ cho mỗi tình huống:

## Reference lifetime

Dưới dạng lifetime của tham chiếu, `'static` chỉ ra rằng dữ liệu được trỏ tới bởi 
tham chiếu tồn tại trong toàn bộ vòng đời của chương trình đang chạy.
Nó vẫn có thể được ép thành một lifetime ngắn hơn.


Có hai cách để tạo một biến với lifetime `'static`, 
và cả hai đều được lưu trữ trong bộ nhớ chỉ đọc của tệp thực thi (binary):

* Khai báo một hằng số với từ khóa `static`.
* Tạo một `string` có kiểu dữ liệu là: `&'static str`.

Xem ví dụ sau để thấy cách sử dụng từng phương pháp:

```rust,editable
//  Tạo một hằng số với lifetime `'static`.
static NUM: i32 = 18;

//Trả về một tham chiếu tới NUM mà lifetime `'static` 
//của nó bị ép thành lifetime của đối số đầu vào.
fn coerce_static<'a>(_: &'a i32) -> &'a i32 {
    &NUM
}

fn main() {
    {
        // Tạo một `chuỗi` văn bản và in nó ra:
        let static_string = "I'm in read-only memory";
        println!("static_string: {}", static_string);

        //Khi `static_string` ra khỏi phạm vi, tham chiếu không thể được sử dụng nữa, 
        //Nhưng dữ liệu vẫn tồn tại trong tệp thực thi (binary) của chương trình.
    }

    {
        // Tạo một số nguyên để sử dụng cho `coerce_static`:
        let lifetime_num = 9;

        // Ép NUM thành lifetime của `lifetime_num`:
        let coerced_static = coerce_static(&lifetime_num);

        println!("coerced_static: {}", coerced_static);
    }

    println!("NUM: {} stays accessible!", NUM);
}
```

## Trait bound

Khi dùng làm một ràng buộc trait, có nghĩa là kiểu dữ liệu không chứa 
bất kỳ tham chiếu non-static nào. Ví dụ, người nhận có thể giữ kiểu đó 
cho đến bất cứ khi nào họ muốn và nó sẽ luôn hợp lệ cho đến khi họ loại bỏ nó.

Quan trọng là bạn hiểu rằng điều này có nghĩa là bất kỳ dữ liệu nào được sở hữu 
luôn luôn đáp ứng được ràng buộc lifetime `'static`, nhưng một tham chiếu đến 
dữ liệu được sở hữu đó thì không đáp ứng điều này:

```rust,editable,compile_fail
use std::fmt::Debug;

fn print_it( input: impl Debug + 'static ) {
    println!( "'static value passed in is: {:?}", input );
}

fn main() {
    //Biến i được sở hữu và không chứa bất kỳ tham chiếu nào, do đó nó là 'static:
    let i = 5;
    print_it(i);

    //oops, tham chiếu &i chỉ có lifetime được xác định bởi phạm vi của hàm main(), 
    //nên nó không phải là 'static:
    print_it(&i);
}
```
Trình biên dịch sẽ báo lỗi với bạn :
```ignore
error[E0597]: `i` không tồn tại trong khoảng thời gian đủ lâu.
  --> src/lib.rs:15:15
   |
15 |     print_it(&i);
   |     ---------^^--
   |     |         |
   |     |         giá trị được mượn không tồn tại đủ lâu
   |      đối số đòi hỏi `i` được mượn với lifetime `'static`
16 | }
   | - `i` bị drop ở đây trong khi nó vẫn được mượn
```

### Xem thêm:

[`'static` constants][static_const]

[static_const]: ../../custom_types/constants.md