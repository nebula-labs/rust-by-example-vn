# Iterator::any

`Iterator::any` là một hàm khi được truyền một iterator(trình vòng lặp), sẽ trả về `true` nếu có bất kỳ phần tử nào thỏa mãn mệnh đề. Nếu không thì `false`. Điểm nhấn của nó: 

```rust
pub trait Iterator {
    // Kiểu lặp đi lặp lại.
    type Item;

    // `any` truyền `&mut self` có nghĩa là người gọi có thể mượn và sửa đổi giá trị,
    // nhưng không thể tiêu thụ nó.
    fn any<F>(&mut self, f: F) -> bool where
        // `FnMut` có nghĩa là bất kì biến nào được chụp nhiều nhất có thể được sửa đổi, không được sử dụng. 
        // `&Self::Item` cho biết nó đưa các đối số vào closure bằng giá trị.
        F: FnMut(Self::Item) -> bool;
}
```
```rust
fn main() {
    let vec1 = vec![1, 2, 3];
    let vec2 = vec![4, 5, 6];

    // `Phương thức iter() cho `vecs trả về kiểu `&i32`. Destructure thành `i32`.
    println!("2 in vec1: {}", vec1.iter()     .any(|&x| x == 2));
    // Phương thức `into_iter()` cho vecs trả về kiểu `i32`. Không yêu cầu Destructure.
    println!("2 in vec2: {}", vec2.into_iter().any(|x| x == 2));

    // Phương thức `iter()` chỉ mượn `vec1` và phần tử của nó, vì thế họ có thể sử dụng lại.
    println!("vec1 len: {}", vec1.len());
    println!("First element of vec1 is: {}", vec1[0]);
    // Phương thức `into_iter()` chuyển `vec2` và các phần tử của nó, vì thế chúng 
    // không thể được sử dụng lại
    // println!("First element of vec2 is: {}", vec2[0]);
    // println!("vec2 len: {}", vec2.len());
    // TODO: Thử không comment hai dòng trên và xem biên dịch lỗi..

    let array1 = [1, 2, 3];
    let array2 = [4, 5, 6];

    // Phương thức `iter()` cho mảng trẩ về kiểu `&i32`.
    println!("2 in array1: {}", array1.iter()     .any(|&x| x == 2));
    // Phương thức `into_iter()` cho mảng trả về kiểu `i32`.
    println!("2 in array2: {}", array2.into_iter().any(|x| x == 2));
}
```
### Xem thêm
[std::iter::Iterator::any](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.any)