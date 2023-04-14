# Operator Overloading

Trong Rust, rất nhiều toán tử có thể được nạp chồng(overloaded) thông qua các trait. Nghĩa là, một số các toán tử có thể được sử dụng để thực hiện các nhiệm vụ khác nhau dựa trên các đối số đầu vào của chúng. Điều này có thể thực hiện được bởi vì các toán tử là cú pháp(syntactic sugar) cho các lời gọi phương thức. Ví dụ, toán tử `+` trong phép tính `a + b` gọi phương thức `add` (tương đương với `a.add(b)`). Phương thức `add` này là một phần của trait `Add`. Do đó, toán tử `+` có thể được sử dụng bởi bất kì loại dữ liệu nào đã triển khai trait `Add`.

Một danh sách các trait, ví dụ như `Add`, toán tử nạp chồng có thể được tìm thấy trong [`core::ops`][ops].

```rust,editable
use std::ops;

struct Foo;
struct Bar;

#[derive(Debug)]
struct FooBar;

#[derive(Debug)]
struct BarFoo;

// Trait `std::ops::Add` được sử dụng để chỉ định chức năng của toán tử `+`
// Ở đây, chúng ta tạo ra `Add<Bar>` - trait cho phép cộng với RHS của kiểu `Bar`.
// Khối lệnh sau đây thực hiện phép tính: Foo + Bar = FooBar
impl ops::Add<Bar> for Foo {
    type Output = FooBar;

    fn add(self, _rhs: Bar) -> FooBar {
        println!("> Foo.add(Bar) was called");

        FooBar
    }
}

// Bằng cách đảo ngược các kiểu dữ liệu, chúng ta thực hiện việc cộng không giao hoán(non-commutative).
// Ở đây, chúng ta tạo ra `Add<Foo>` - trait cho phép cộng với RHS của kiểu `Foo`.
// Khối lệnh sau đây thực hiện phép tính: Bar + Foo = BarFoo
impl ops::Add<Foo> for Bar {
    type Output = BarFoo;

    fn add(self, _rhs: Foo) -> BarFoo {
        println!("> Bar.add(Foo) was called");

        BarFoo
    }
}

fn main() {
    println!("Foo + Bar = {:?}", Foo + Bar);
    println!("Bar + Foo = {:?}", Bar + Foo);
}
```

### Xem thêm

[Add][add], [Syntax Index][syntax]

[add]: https://doc.rust-lang.org/core/ops/trait.Add.html
[ops]: https://doc.rust-lang.org/core/ops/
[syntax]:https://doc.rust-lang.org/book/appendix-02-operators.html