# Tìm kiếm thông qua các iterator (trình vòng lặp)

`Iterator::find` là một hàm lặp qua một trình vòng lặp rồi tìm kiếm giá trị đầu tiên thỏa mãn một số điều kiện nào đó. Nếu không có giá trị nào thỏa mãn điều kiện, nó sẽ trả về  `None`. Điểm nhấn của nó:

```rust
pub trait Iterator {
    // Kiểu lặp đi lặp lại.
    type Item;

    // `find` truyền `&mut self` nghĩa là người gọi có thể mượn và sửa đổi,
    // nhưng không được tiêu thụ.
    fn find<P>(&mut self, predicate: P) -> Option<Self::Item> where
        // `FnMut` có nghĩa là bất kì biến nào được chụp nhiều nhất có thể được sửa đổi, không được sử dụng. 
        // &Self::Item cho biết cho biết nó nhận các đối số vào closure bằng tham chiếu
        P: FnMut(&Self::Item) -> bool;
}
```
```rust
fn main() {
    let vec1 = vec![1, 2, 3];
    let vec2 = vec![4, 5, 6];

    // Phương thức `iter()` cho vecs sẽ trả về kiểu `&i32`.
    let mut iter = vec1.iter();
    // Phương thức `into_iter()` cho vecs sẽ trả về kiểu `i32`.
    let mut into_iter = vec2.into_iter();

    // Phương thức`iter()` cho vecs sẽ trả về kiểu `&i32`, và chúng tôi muốn tham chiếu tới một phần tử của nó, 
    // vì thế chúng tôi phải destructure `&&i32` thành `i32`.

    println!("Find 2 in vec1: {:?}", iter     .find(|&&x| x == 2));
    // Phương thức `into_iter()` cho vecs sẽ trả về kiểu `i32`, và chúng tôi muốn tham chiếu tới một trong các mục của nó,
    // Vì thế, chúng tôi phải destructure `&i32` thành `i32`
    println!("Find 2 in vec2: {:?}", into_iter.find(| &x| x == 2));

    let array1 = [1, 2, 3];
    let array2 = [4, 5, 6];

    // Phương thức `iter()` cho mảng sẻ trả sẽ về kiểu `&i32`
    println!("Find 2 in array1: {:?}", array1.iter()     .find(|&&x| x == 2));
    // Phương thức `into_iter()` cho mảng sẽ trả về kiểu `i32`
    println!("Find 2 in array2: {:?}", array2.into_iter().find(|&x| x == 2));
}
```
`Iterator::find` cung cấp cho bạn một tham chiếu đến giá trị. Nhưng nếu bạn muốn lấy `*chỉ số`*` của giá trị, hãy sử dụng `Iterator::position`.

```rust
fn main() {
    let vec = vec![1, 9, 3, 3, 13, 2];

    // Phương thức `iter()` cho vecs sẽ trả về kiểu `&i32` và phương thức `position()` không cung cấp một tham chiếu nào, vì thế
    // chúng ta phải destructure `&i32` thành `i32`
    let index_of_first_even_number = vec.iter().position(|&x| x % 2 == 0);
    assert_eq!(index_of_first_even_number, Some(5));
    
    // Phương thức `into_iter()` cho vecs trả về kiểu `i32` và phương thức `position()` không cung cấp một tham chiếu nào, vì thế
    // chúng ta phải destructure....
    let index_of_first_negative_number = vec.into_iter().position(|x| x < 0);
    assert_eq!(index_of_first_negative_number, None);
}
```
### Xem thêm:
[std::iter::Iterator::find](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.find)
[std::iter::Iterator::find_map](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.find_map)
[std::iter::Iterator::position](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.position)
[std::iter::Iterator::rposition](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.rposition)