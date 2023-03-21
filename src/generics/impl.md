# Implementation

Tương tự như các hàm(function), các triển khai cũng yêu cầu sự cẩn thận để giữ cho chúng có tính tổng quát(generic).

```rust
struct S; // Kiểu dữ liệu cụ thể struct `S`
struct GenericVal<T>(T); // Kiểu giữ liệu chung(generic) `GenericVal`

// triển khai của GenericVal trong đó chúng ta chỉ định rõ ràng các kiểu tham số:
impl GenericVal<f32> {} // Chỉ định rõ ràng kiểu `f32`
impl GenericVal<S> {} // Chỉ định rõ ràng kiểu `S` được định nghĩa ở trên

// `<T>` Phải đặt trước kiểu để giữ tính tổng quát
impl<T> GenericVal<T> {}
```

```rust,editable
struct Val {
    val: f64,
}

struct GenVal<T> {
    gen_val: T,
}

// triển khai của Val
impl Val {
    fn value(&self) -> &f64 {
        &self.val
    }
}

// triển khai của GenVal cho kiểu dữ liệu chung `T`
impl<T> GenVal<T> {
    fn value(&self) -> &T {
        &self.gen_val
    }
}

fn main() {
    let x = Val { val: 3.0 };
    let y = GenVal { gen_val: 3i32 };

    println!("{}, {}", x.value(), y.value());
}
```

### See also:

[functions returning references][fn], [`impl`][methods], and [`struct`][structs]


[fn]: ../scope/lifetime/fn.md
[methods]: ../fn/methods.md
[specialization_plans]: https://blog.rust-lang.org/2015/05/11/traits.html#the-future
[structs]: ../custom_types/structs.md