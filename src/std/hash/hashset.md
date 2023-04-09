# HashSet
`HashSet` được coi như là một `HashMap` nếu chúng ta chỉ quan tâm đến các key( `HashSet<T>` thực tế chỉ là một wrapper(bao bọc) xung quanh `HashMap<T,()>`).

Bạn sẽ hỏi "Ý nghĩa của việc này là gì?". "Tôi có thể đơn giản là lưu trữ các key trong Vec".

Điểm đặc biệt của `HashSet` là nó đảm bảo không có các phần tử trùng lặp. Đó là điều mà bất kì một collection set nào cũng đáp ứng.`HashSet` chỉ là một triển khai của nó (collection). (Xem thêm: [BTreeSet](https://doc.rust-lang.org/std/collections/struct.BTreeSet.html))

Nếu bạn thêm vào một giá trị đã tồn tại trong `HashSet`, (Nghĩa là giá trị mới bằng với cũ và cả hai đều có cùng hàm hash), thì giá trị mới sẽ thay thế giá trị cũ.

Điều này thật hữu ích khi bạn không muốn có nhiều hơn một thứ gì đó hoặc khi bạn muốn biết liệu bạn đã có thứ gì đó chưa.

Nhưng các set còn có thể làm nhiều hơn thế.
Các Set có 4 thao tác chính( tất cả các lệnh gọi sau đây đều trả về một iterator):

- `union`: lấy tất cả các phần tử chỉ xuất hiện một lần trong cả hai tập.
- `difference`: lấy tất cả các phần tử có trong tập đầu tiên nhưng không có trong tập thứ hai.
- `intersection`: lấy tất cả các phần tử chỉ xuất hiện trong cả hai tập.
- `symmetric_difference`: lấy tất cả các phần tử chỉ xuất hiện trong tập này nhưng không xuất hiện trong tập kia. 

Hãy thử tất cả những thao tác đó trong ví dụ sau:

```rust
use std::collections::HashSet;

fn main() {
    let mut a: HashSet<i32> = vec![1i32, 2, 3].into_iter().collect();
    let mut b: HashSet<i32> = vec![2i32, 3, 4].into_iter().collect();

    assert!(a.insert(4));
    assert!(a.contains(&4));

    // `HashSet::insert()` trả về false nếu
    // đã có giá trị.
    assert!(b.insert(4), "Value 4 is already in set B!");
    // FIXME ^ Comment out this line

    b.insert(5);

    // Nếu kiểu phần tử của một collection triển khai `Debug`,
    // thì collection đó cũng triển khai `Debug`. 
    // Thông thường nó sẽ in các phần tử của nó theo định dạng [elem1, elem2, ...]`
    println!("A: {:?}", a);
    println!("B: {:?}", b);

    // In [1, 2, 3, 4, 5] theo thứ tự tùy ý

    println!("Union: {:?}", a.union(&b).collect::<Vec<&i32>>());

    // Chỗ này sẽ in ra [1]
    println!("Difference: {:?}", a.difference(&b).collect::<Vec<&i32>>());

    // In [2, 3, 4] theo thứ tự tùy ý.
    println!("Intersection: {:?}", a.intersection(&b).collect::<Vec<&i32>>());

    // In [1, 5]
    println!("Symmetric Difference: {:?}",
             a.symmetric_difference(&b).collect::<Vec<&i32>>());
}
```
(Các ví dụ đã được điều chỉnh theo [documentation](https://doc.rust-lang.org/std/collections/struct.HashSet.html#method.difference).)





