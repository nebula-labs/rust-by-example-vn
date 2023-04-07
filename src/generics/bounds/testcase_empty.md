# Testcase: empty bounds
# Bộ kiểm thử: ràng buộc rỗng

Một hệ quả của cách các ràng buộc (bounds) hoạt động là ngay cả khi một `trait` 
không bao gồm bất cứ hàm nào, ta vẫn có thể sử dụng nó như một ràng buộc. 
`Eq` và `Copy` là những ví dụ của các `trait` như vậy từ thư viện `std`.

```rust,editable
struct Cardinal;
struct BlueJay;
struct Turkey;

trait Red {}
trait Blue {}

impl Red for Cardinal {}
impl Blue for BlueJay {}

// Những hàm này chỉ có giá trị với các kiểu dữ liệu đã triển khai các trait naỳ.
// Thực tế là dù trait không có bất kì hàm nào cũng chẳng sao.
fn red<T: Red>(_: &T)   -> &'static str { "red" }
fn blue<T: Blue>(_: &T) -> &'static str { "blue" }

fn main() {
    let cardinal = Cardinal;
    let blue_jay = BlueJay;
    let _turkey   = Turkey;

    // Hàm `red()` không thể được sử dụng trên thực thể "giẻ cùi làm"
    // và ngược lại bởi các ràng buộc (bounds).
    println!("Chim hồng y giáo chủ có màu {}", red(&cardinal));
    println!("Chim giẻ cùi làm có màu {}", blue(&blue_jay));
    //println!("Gà tây có màu {}", red(&_turkey));
    // ^ TODO: Hãy thử bỏ comment dòng trên.
}
```

### Tham khảo thêm:

[`std::cmp::Eq`][eq], [`std::marker::Copy`][copy], and [`trait`s][traits]

[eq]: https://doc.rust-lang.org/std/cmp/trait.Eq.html
[copy]: https://doc.rust-lang.org/std/marker/trait.Copy.html
[traits]: ../../trait.md
