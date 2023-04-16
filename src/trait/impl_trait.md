# impl Trait

`impl Trait` có thể được dùng trong hai vai trò:

1. như là một kiểu đối số
2. như là một kiểu trả về

## Dùng như là một kiểu đối số

Nếu hàm của bạn có chung một đặc điểm nhưng bạn không quan tâm đến kiểu cụ thể, thì bạn có thể đơn giản hóa việc khai báo hàm bằng cách sử dụng `impl Trait` làm kiểu đối số.

Ví dụ, hãy xem xét đoạn mã dưới đây:

```rust,editable
fn parse_csv_document<R: std::io::BufRead>(src: R) -> std::io::Result<Vec<Vec<String>>> {
    src.lines()
        .map(|line| {
            // Với từng dòng trong file
            line.map(|line| {
                // Nếu dòng được đọc thành công, xử lý nó, nếu không thì trả về lỗi
                line.split(',') // Tách dòng thành những phần nhỏ hơn được chia tách bởi dấu phẩy
                    .map(|entry| String::from(entry.trim())) // Xóa khoảng trắng trước và sau
                    .collect() // Tổng hợp các chuỗi của dòng thành một Vec<String>
            })
        })
        .collect() // Tổng hợp các dòng thành một Vec<Vec<String>>
}
```

`parse_csv_document` là một hàm tổng quát, cho phép nó nhận bất kỳ kiểu gì có cài BufRead, như là `BufReader<File>` hay `[u8]`,
mà không quan trọng `R` là kiểu gì, và `R` chỉ được dùng để định nghia kiểu của `src`, vì thế phương thức có thể được viết:

```rust,editable
fn parse_csv_document(src: impl std::io::BufRead) -> std::io::Result<Vec<Vec<String>>> {
    src.lines()
        .map(|line| {
            // Với từng dòng trong file
            line.map(|line| {
                // Nếu dòng được đọc thành công, xử lý nó, nếu không thì trả về lỗi
                line.split(',') // Tách dòng thành những phần nhỏ hơn được chia tách bởi dấu phẩy
                    .map(|entry| String::from(entry.trim())) // Xóa khoảng trắng trước và sau
                    .collect() // Tổng hợp các chuỗi của dòng thành một Vec<String>
            })
        })
        .collect() // Tổng hợp các dòng thành một Vec<Vec<String>>
}
```

Lưu ý rằng sử dụng `impl Trait` như là một kiểu tham số có nghĩa là bạn không thể nêu rõ hình thức phương thức được sử dụng, ví dụ `parse_csv_document::<std::io::Empty>(std::io::empty())` sẽ không hoạt động với ví dụ thứ hai.


## Dùng như một kiểu trả về

Nếu hàm của bạn trả về một kiểu có cài `MyTrait`, bạn có thể viết kiểu trả về
của nó lầ `-> impl MyTrait`. Điều này giúp đơn giản hóa kiểu trả về của bạn rất nhiều!

```rust,editable
use std::iter;
use std::vec::IntoIter;

// Hàm này kết hợp hai `Vec<i32>` và trả về một iterator.
// Hãy xem kiểu trả về của nó phức tạp thế nào!
fn combine_vecs_explicit_return_type(
    v: Vec<i32>,
    u: Vec<i32>,
) -> iter::Cycle<iter::Chain<IntoIter<i32>, IntoIter<i32>>> {
    v.into_iter().chain(u.into_iter()).cycle()
}

// Đây là một hàm tương tựm , như kiểu trả về của nó sử dụng `impl Trait`.
// Hãy xem nó đơn giản hơn đến mức nào!
fn combine_vecs(
    v: Vec<i32>,
    u: Vec<i32>,
) -> impl Iterator<Item=i32> {
    v.into_iter().chain(u.into_iter()).cycle()
}

fn main() {
    let v1 = vec![1, 2, 3];
    let v2 = vec![4, 5];
    let mut v3 = combine_vecs(v1, v2);
    assert_eq!(Some(1), v3.next());
    assert_eq!(Some(2), v3.next());
    assert_eq!(Some(3), v3.next());
    assert_eq!(Some(4), v3.next());
    assert_eq!(Some(5), v3.next());
    println!("all done");
}
```
Quan trọng hơn nữa, một số kiểu trong Rust không thể ghi ra. Ví dụ, mỗi closure đều
có kiểu trả về của nó. Trước cú pháp `impl Trait`, bạn phải cấp phát trên heap để
trả về một closure. Nhưng giờ bạn có thể làm điều đó một cách tĩnh, như thế này:

```rust,editable
// Trả về một hàm cho cộng thêm `y` vào đầu vào
fn make_adder_function(y: i32) -> impl Fn(i32) -> i32 {
    let closure = move |x: i32| { x + y };
    closure
}

fn main() {
    let plus_one = make_adder_function(1);
    assert_eq!(plus_one(2), 3);
}
```

Bạn có thể dùng `impl Trait` để trả về một iterator sử dụng các closure `map` hoặc 
`filter`! Điều này khiến việc dùng `map` và `filter` dễ hơn. Nhưng các kiểu closure
không có tên, bạn không thể ghi ra chính xác kiểu trả về nếu hàm của bạn trả về 
You can also use `impl Trait` to return an iterator that uses `map` or `filter`
closures! This makes using `map` and `filter` easier. Because closure types don't
. Nhưng với  `impl Trait` bạn có thể làm điều này một cách dễ dàng:

```rust,editable
fn double_positives<'a>(numbers: &'a Vec<i32>) -> impl Iterator<Item = i32> + 'a {
    numbers
        .iter()
        .filter(|x| x > &&0)
        .map(|x| x * 2)
}

fn main() {
    let singles = vec![-3, -2, 2, 3];
    let doubles = double_positives(&singles);
    assert_eq!(doubles.collect::<Vec<i32>>(), vec![4, 6]);
}
```