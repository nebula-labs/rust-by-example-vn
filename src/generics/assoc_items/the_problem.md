# The Problem

Một `trait` được khai báo với tính chất generic đối với kiểu container của nó có các yêu cầu về kiểu -
người sử dụng `trait` *phải* chỉ định tất cả các kiểu generic của nó.

Trong ví dụ dưới đây, `trait` `Contains` cho phép sử dụng các kiểu generic `A` và `B`.
Sau đó, trait được triển khai cho kiểu `Container`, chỉ định kiểu `i32` cho `A` và `B`
để có thể sử dụng với hàm difference().

Bởi vì `Contains` là generic, chúng ta buộc phải chỉ định rõ ràng *tất cả* các
kiểu generic cho `fn difference()`. Trong thực tế, chúng ta muốn có một cách để
biểu thị rằng `A` và `B` được xác định bởi *đầu vào* `C`. Bạn sẽ thấy trong phần tiếp
theo, associated types cho ta khả năng đó.

```rust,editable
struct Container(i32, i32);

// Trait kiểm tra xem 2 phần tử có được lưu trữ bên trong container hay không.
// Trait cũng trích xuất giá trị đầu tiên hoặc cuối cùng.
trait Contains<A, B> {
    fn contains(&self, _: &A, _: &B) -> bool; // Yêu cầu `A` và `B` rõ ràng.
    fn first(&self) -> i32; // Không yêu cầu rõ ràng `A` hoặc `B`.
    fn last(&self) -> i32;  // Không yêu cầu rõ ràng `A` hoặc `B`.
}

impl Contains<i32, i32> for Container {
    // Trả về `true` nếu các số được lưu trữ bằng nhau.
    fn contains(&self, number_1: &i32, number_2: &i32) -> bool {
        (&self.0 == number_1) && (&self.1 == number_2)
    }

    // Lấy số đầu tiên.
    fn first(&self) -> i32 { self.0 }

    // Lấy số cuối cùng.
    fn last(&self) -> i32 { self.1 }
}

// `C` chứa `A` và `B`. Vì vậy, phải biểu thị lại `A` và
// `B` sẽ khá là bất tiện.
fn difference<A, B, C>(container: &C) -> i32 where
    C: Contains<A, B> {
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

### Xem thêm:

[`struct`s][structs], và [`trait`s][traits]

[structs]: ../../custom_types/structs.md
[traits]: ../../trait.md