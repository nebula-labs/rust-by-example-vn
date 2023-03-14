# Capturing

Closures có tính linh hoạt và sẽ thực hiện bất cứ điều gì mà function yêu cầu để làm cho closure hoạt động mà không cần chú thích(annotation). Điều này cho phép việc capturing(khi khai báo closure) thích nghi với các trường hợp sử dụng(use case), đôi khi sử dụng moving và borrowing.
Closures có thể capture(khai báo) các biến thông qua:

* bằng tham chiếu: `&T`
* bằng tham chiếu có thể thay đổi: `&mut T`
* bằng giá trị: `T`

Chúng ưu tiên capture biến bằng tham chiếu và chỉ đi sử dụng cách thấp hơn khi cần thiết.

```rust,editable
fn main() {
    use std::mem;
    
    let color = String::from("green");

    // Một closure để print `color` sẽ ngay lập tức mượn(borrows) (`&`) `color` và
    // lưu borrow và closure vào trong biến `print`. Nó vẫn sẽ được borrow cho đến khi `print` được sử dụng lần cuối 
    // `println!` chỉ yêu cầu đối số bằng tham chiếu không thể thay đổi(immutable) vì vậy nó không đặt ra bất cứ hạn chế nào khác.
    let print = || println!("`color`: {}", color);

    // Gọi closure bằng cách sử dụng borrow.
    print();

    // `color` có thể được mượn(borrow) một lần nữa bằng cách không thay đổi(immutably), vì closure chỉ giữ một tham chiếu không thể thay đổi(immutable) tới `color`. 
    let _reborrow = &color;
    print();

    // Một thao tác move hoặc reborrow được phép sau lần cuối cùng sử dụng `print`
    let _color_moved = color;


    let mut count = 0;
    // Một closure để tăng giá trị của `count` có thể sử dụng `&mut count` hoặc `count`
    // nhưng `&mut count` là hạn chế hơn so nên nó sẽ lấy tham chiếu đến `count`. Ngay lập tức borrows `count`.
    //
    // Cần thêm `mut` cho `inc` bởi vì một tham chiếu `&mut` được lưu dữ liệu bên trong. 
    // Do đó, việc gọi closure làm thay đổi trong closure, điều này yêu cầu một `mut`.
    let mut inc = || {
        count += 1;
        println!("`count`: {}", count);
    };

    // Gọi tới closure bằng cách sử dụng một mutable borrow.
    inc();

    // Closure vẫn mượn(borrows) `count` có thể thay đổi(mutably) bởi vì nó được gọi sau đó.
    // Một nỗ lực để mượn lại(reborrow) sẽ dẫn đến một lỗi.
    // let _reborrow = &count; 
    // ^ TODO: thử bỏ chú thích(uncommenting) dòng phía trên.
    inc();

    // closure không cần phải mượn(borrow) `&mut count`. 
    // Do đó, có thể mượn(reborrow) lại mà không gặp lỗi
    let _count_reborrowed = &mut count; 

    
    // Một kiểu dữ liệu không thể sao chép(A non-copy type).
    let movable = Box::new(3);

    // `mem::drop` yêu cầu `T` phải truyền vào theo giá trị(take by value). 
    // Một kiểu dữ liệu có thể sao chép (copy type) sẽ sao chép vào closure mà không làm thay đổi đối tượng ban đầu.
    // Kiểu dữ liệu không thể sao chép phải được di chuyển(move) và do đó, `movable` sẽ được di chuyển ngay vào closure.
    let consume = || {
        println!("`movable`: {:?}", movable);
        mem::drop(movable);
    };

    // biến `consume` chỉ có thể gọi một lần.
    consume();
    // consume();
    // ^ TODO: Thử bỏ chú thích(uncommenting) dòng phía trên.
}
```

Sử dụng `move` trước dấu '||' thì closure sẽ buộc phải lấy quyền sở hữu(ownership) của các biến được capture:

```rust,editable
fn main() {
    // `Vec` có tính không thể sao chép (non-copy).
    let haystack = vec![1, 2, 3];

    let contains = move |needle| haystack.contains(needle);

    println!("{}", contains(&1));
    println!("{}", contains(&4));

    // println!("There're {} elements in vec", haystack.len());
    // ^ bỏ chú thích(uncommenting) dòng phía trên sẽ trả về lỗi compile
    // bởi vì trình kiểm tra borrow checker không cho phép sử dụng lại biến sau khi nó đã được move.
    
    // Xóa `move` khỏi chữ kí của closure sẽ khiến closure
    // mượn(borrow) biến _haystack_ một cách không thể thay đổi, do đó _haystack_ vẫn 
    // khả dụng và bỏ chú thích(uncommenting) dòng trên sẽ không gây ra lỗi.
}
```

### See also:

[`Box`][box] and [`std::mem::drop`][drop]

[box]: ../../std/box.md
[drop]: https://doc.rust-lang.org/std/mem/fn.drop.html