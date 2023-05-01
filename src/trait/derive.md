# Derive

Trình biên dịch Rust có thể cung cấp các triển khai cơ bản cho một số trait thông qua `#[derive]` [attribute][attribute]. Những trait này vẫn có thể được triển khai thủ công nếu cần một hành vi phức tạp hơn.

Dưới đây là một danh sách các trait có thể được dẫn xuất:

- Các trait so sánh:
  [`Eq`][eq], [`PartialEq`][partial-eq], [`Ord`][ord], [`PartialOrd`][partial-ord].
- [`Clone`][clone], để tạo ra kiểu `T` từ `&T` thông qua bản sao.
- [`Copy`][copy], để cho phép kiểu có 'copy semantics' thay vì 'move semantics'.
- [`Hash`][hash], để tính toán hàm băm từ `&T`.
- [`Default`][default], để tạo một thể hiện trống của kiểu dữ liệu.
- [`Debug`][debug], để định dạng một giá trị sử dụng trình định dạng `{:?}`.

```rust,editable
// `Centimeters`, một cấu trúc tuple có thể so sánh
#[derive(PartialEq, PartialOrd)]
struct Centimeters(f64);

// `Inches`, một cấu trúc tuple có thể in ra
#[derive(Debug)]
struct Inches(i32);

impl Inches {
    fn to_centimeters(&self) -> Centimeters {
        let &Inches(inches) = self;

        Centimeters(inches as f64 * 2.54)
    }
}

// `Seconds`, một cấu trúc tuple không có thuộc tính bổ sung
struct Seconds(i32);

fn main() {
    let _one_second = Seconds(1);

    // Error! `Seconds` không thể được in ra; nó không triển khai trait `Debug`
    //println!("One second looks like: {:?}", _one_second);
    // TODO ^ thử bỏ chú thích dòng trên

    // Error! `Seconds` không thể được so sánh; nó không triển khai trait `PartialEq`
    //let _this_is_true = (_one_second == _one_second);
    // TODO ^ thử bỏ chú thích dòng trên

    let foot = Inches(12);

    println!("One foot equals {:?}", foot);

    let meter = Centimeters(100.0);

    let cmp =
        if foot.to_centimeters() < meter {
            "smaller"
        } else {
            "bigger"
        };

    println!("One foot is {} than one meter.", cmp);
}
```

### Xem thêm:

[`derive`][derive]

[attribute]: ../attribute.md
[eq]: https://doc.rust-lang.org/std/cmp/trait.Eq.html
[partial-eq]: https://doc.rust-lang.org/std/cmp/trait.PartialEq.html
[ord]: https://doc.rust-lang.org/std/cmp/trait.Ord.html
[partial-ord]: https://doc.rust-lang.org/std/cmp/trait.PartialOrd.html
[clone]: https://doc.rust-lang.org/std/clone/trait.Clone.html
[copy]: https://doc.rust-lang.org/core/marker/trait.Copy.html
[hash]: https://doc.rust-lang.org/std/hash/trait.Hash.html
[default]: https://doc.rust-lang.org/std/default/trait.Default.html
[debug]: https://doc.rust-lang.org/std/fmt/trait.Debug.html
[derive]: https://doc.rust-lang.org/reference/attributes.html#derive
