# Structs

Chú thích về thời gian tồn tại trong một structure cũng tương tự như với function:

```rust,editable
// Một kiểu `Borrowed` chứa một tham chiếu tới `i32`.
// Tham chiếu tới `i32` phải tồn tại lâu hơn `Borrowed`.
#[derive(Debug)]
struct Borrowed<'a>(&'a i32);

// Tương tự, cả hai tham chiếu tới `i32` dưới đây phải tồn tại lâu hơn `NamedBorrowed`.
#[derive(Debug)]
struct NamedBorrowed<'a> {
    x: &'a i32,
    y: &'a i32,
}

// Một enum đồng thời có thể là `i32` hoặc tham chiếu tới `i32`.
#[derive(Debug)]
enum Either<'a> {
    Num(i32),
    Ref(&'a i32),
}

fn main() {
    let x = 18;
    let y = 15;

    let single = Borrowed(&x);
    let double = NamedBorrowed { x: &x, y: &y };
    let reference = Either::Ref(&x);
    let number    = Either::Num(y);

    println!("x is borrowed in {:?}", single);
    println!("x and y are borrowed in {:?}", double);
    println!("x is borrowed in {:?}", reference);
    println!("y is *not* borrowed in {:?}", number);
}
```

### Xem thêm:

[`struct`s][structs]

[structs]: ../../custom_types/structs.md