# Associated types

Việc sử dụng "Associated types" giúp code dễ đọc hơn bằng cách chuyển
các kiểu bên trong cục bộ vào một trait như là các kiểu *output*.
Cú pháp định nghĩa `trait`:

```rust
// `A` và `B` được định nghĩa trong trait thông qua từ khóa `type`.
// (Lưu ý: `type` trong ngữ cảnh này khác với `type` khi sử dụng cho các aliases).
trait Contains {
    type A;
    type B;

    // Cập nhật cú pháp để tham chiếu đến các kiểu mới này theo cách chung chung (generically).
    fn contains(&self, _: &Self::A, _: &Self::B) -> bool;
}
```

Lưu ý rằng các hàm sử dụng `trait` `Contains` không cần phải biểu thị `A` hoặc `B` nữa:

```rust,ignore
// Không sử dụng associated types
fn difference<A, B, C>(container: &C) -> i32 where
    C: Contains<A, B> { ... }

// Sử dụng associated types
fn difference<C: Contains>(container: &C) -> i32 { ... }
```

Tiếp theo, chúng ta sẽ viết lại ví dụ từ phần trước bằng cách sử dụng associated types:

```rust,editable
struct Container(i32, i32);

// Trait kiểm tra xem 2 phần tử có được lưu trữ bên trong container hay không.
// Trait cũng trích xuất giá trị đầu tiên hoặc cuối cùng.
trait Contains {
    // Định nghĩa kiểu generics mà các method có thể sử dụng.
    type A;
    type B;

    fn contains(&self, _: &Self::A, _: &Self::B) -> bool;
    fn first(&self) -> i32;
    fn last(&self) -> i32;
}

impl Contains for Container {
    // Chỉ định kiểu dữ liệu `A` và `B`. Nếu kiểu dữ liệu `input`
    // là `Container(i32, i32)`, kiểu dữ liệu `output` sẽ được xác định
    // là `i32` và `i32`.
    type A = i32;
    type B = i32;

    // `&Self::A` và `&Self::B` cũng hợp lệ ở đây.
    fn contains(&self, number_1: &i32, number_2: &i32) -> bool {
        (&self.0 == number_1) && (&self.1 == number_2)
    }
    // Lấy số đầu tiên.
    fn first(&self) -> i32 { self.0 }

    // Lấy số cuối cùng.
    fn last(&self) -> i32 { self.1 }
}

fn difference<C: Contains>(container: &C) -> i32 {
    container.last() - container.first()
}

fn main() {
    let number_1 = 3;
    let number_2 = 10;

    let container = Container(number_1, number_2);

    println!("Container có chứa {} và {}: {}",
        &number_1, &number_2,
        container.contains(&number_1, &number_2));
    println!("Số đầu: {}", container.first());
    println!("Số cuối: {}", container.last());
    
    println!("Hiệu 2 số là: {}", difference(&container));
}
```