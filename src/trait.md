# Traits

`Trait` là tập hợp các phương thức được định nghĩa cho một loại không xác định: `Self`. Chúng có thể truy cập các phương thức khác được khai báo trong cùng một trait.

Các trait có thể được triển khai cho bất kỳ loại dữ liệu nào. Trong ví dụ bên dưới, chúng tôi định nghĩa `Animal`, là một nhóm các phương thức. Sau đó, `trait` `Animal`  được triển khai cho kiểu dữ liệu `Sheep`, cho phép sử dụng các phương thức của `Animal` với dữ liệu kiểu `Sheep`.

```rust,editable
struct Sheep { naked: bool, name: &'static str }

trait Animal {
    // Mô tả hàm liên kết; `Self` đề cập đến kiểu được triển khai.
    fn new(name: &'static str) -> Self;

    // Mô tả phương thức; chúng sẽ trả về một chuỗi.
    fn name(&self) -> &'static str;
    fn noise(&self) -> &'static str;

    // Các Traits có thể cung cấp các định nghĩa phương thức mặc định.
    fn talk(&self) {
        println!("{} says {}", self.name(), self.noise());
    }
}

impl Sheep {
    fn is_naked(&self) -> bool {
        self.naked
    }

    fn shear(&mut self) {
        if self.is_naked() {
            // Các phương thức triển khai có thể sử dụng các phương thức trait của phương thức triển khai.
            println!("{} is already naked...", self.name());
        } else {
            println!("{} gets a haircut!", self.name);

            self.naked = true;
        }
    }
}

// Triển khai trait `Động vật` cho `Cừu`.
impl Animal for Sheep {
    // `Self` là loại triển khai của `Sheep`.
    fn new(name: &'static str) -> Sheep {
        Sheep { name: name, naked: false }
    }

    fn name(&self) -> &'static str {
        self.name
    }

    fn noise(&self) -> &'static str {
        if self.is_naked() {
            "baaaaah?"
        } else {
            "baaaaah!"
        }
    }
    
    // Các phương thức trait mặc định có thể được ghi đè.
    fn talk(&self) {
        // Ví dụ, chúng ta có thể thêm một vài khoảng pause
        println!("{} pauses briefly... {}", self.name, self.noise());
    }
}

fn main() {
    // Chú thích kiểu dữ liệu là cần thiết trong trường hợp này.
    let mut dolly: Sheep = Animal::new("Dolly");
    // TODO ^ Hãy thử bỏ chú thích

    dolly.talk();
    dolly.shear();
    dolly.talk();
}
```
