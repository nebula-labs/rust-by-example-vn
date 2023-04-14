# Vectors

Các vectơ là các mảng có thể thay đổi kích thước. Giống như các lát cắt, kích thước của chúng không được biết tại thời điểm biên dịch, nhưng chúng có thể tăng hoặc giảm kích thước bất cứ lúc nào. Một vectơ được biểu diễn bằng 3 tham số:
- con trỏ đến dữ liệu
- chiều dài
- dung lượng

Dung lượng cho biết dung lượng bộ nhớ được dành riêng cho vectơ. Vectơ có thể phát triển miễn là chiều dài nhỏ hơn dung lượng. Khi cần vượt qua ngưỡng này, vectơ được cấp phát lại với dung lượng lớn hơn.

```rust,editable,ignore,mdbook-runnable
fn main() {
    // Iterators có thể được gom thành vector
    let collected_iterator: Vec<i32> = (0..10).collect();
    println!("Collected (0..10) into: {:?}", collected_iterator);

    // Macro `vec!` có thể dùng để khởi tạo một vector
    let mut xs = vec![1i32, 2, 3];
    println!("Initial vector: {:?}", xs);

    // Chèn phần tử mới vào cuối vector
    println!("Push 4 into the vector");
    xs.push(4);
    println!("Vector: {:?}", xs);

    // Lỗi! Các vectơ bất biến không thể phát triển
    collected_iterator.push(0);
    // FIXME ^ Comment out this line

    // Phương thức `len` trả về số lượng phần tử hiện được lưu trữ trong một vector
    println!("Vector length: {}", xs.len());

    // Việc lập chỉ mục được thực hiện bằng cách sử dụng dấu ngoặc vuông (việc lập chỉ mục bắt đầu từ 0)
    println!("Second element: {}", xs[1]);

    // `pop` loại bỏ phần tử cuối cùng khỏi vector và trả về nó
    println!("Pop last element: {:?}", xs.pop());

    // Lập chỉ mục ngoài giới hạn kiến trả về lỗi
    println!("Fourth element: {}", xs[3]);
    // FIXME ^ Comment out this line

    // `Vector`s có thể dễ dàng lặp lại
    println!("Contents of xs:");
    for x in xs.iter() {
        println!("> {}", x);
    }

    // Một `Vector` cũng có thể được lặp lại trong khi lặp lại
    // số đếm được liệt kê trong một biến riêng biệt (`i`)
    for (i, x) in xs.iter().enumerate() {
        println!("In position {} we have value {}", i, x);
    }

    // Nhờ có `iter_mut`, `Vector` có thể thay đổi cũng có thể được lặp lại
    // kết thúc theo cách cho phép sửa đổi từng giá trị
    for x in xs.iter_mut() {
        *x *= 3;
    }
    println!("Updated vector: {:?}", xs);
}
```

Có thể tìm thấy nhiều phương thức `Vec` hơn trong module
[std::vec][vec]

[vec]: https://doc.rust-lang.org/std/vec/
