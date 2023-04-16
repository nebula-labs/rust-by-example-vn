# Trả về kiểu Traits với từ khóa `dyn`

Trình biên dịch của Rust cần biết cần bao nhiêu bộ nhớ để chứa kiểu dữ liệu trả về của mỗi hàm. Điều đó có nghĩa là mọi hàm mà bạn viết phải trả về một kiểu cố định. Không như những ngôn ngữ lập trình khác, nếu bạn có một trait ví dụ `Animal`, bạn không thể viết một hàm mà trả về kiểu `Animal`, vì những triển khai (implementation) khác nhau của nó sẽ cần kích thước bộ nhớ khác nhau.

Tuy nhiên, có một cách giải quyết khác. Thay vì phải trả về trực tiếp một đối tượng trait, các hàm của ta sẽ trả về một `Box` _chứa_ một vài `Animal`. `Box` chỉ là một tham chiếu đến một vài vùng nhớ trên heap. Bởi vì một tham chiếu có một kích thước tĩnh cố định và được biết từ trước và trình biên dịch có thể đảm bảo tham chiếu đó trỏ đến vùng nhớ trên heap của `Animal`, ta có thể trả về một trait từ hàm của mình!

Rust sẽ cố gắng tường minh nhất có thể quá trình nó cấp phát bộ nhớ trên heap. Vì vậy nếu hàm của bạn trả về một con trỏ đến trait trên vùng nhớ theo cách này, bạn cần khai báo kiểu trả về với từ khóa `dyn`, ví dụ: `Box<dyn Animal>`.

```rust,editable
struct Sheep {}
struct Cow {}

trait Animal {
    // Đặc trưng phương thức của đối tượng (instance)
    fn noise(&self) -> &'static str;
}

// Implement `Animal` cho struct `Sheep`.
impl Animal for Sheep {
    fn noise(&self) -> &'static str {
        "baaaaah!"
    }
}

// Implement trait `Animal` cho struct `Cow`.
impl Animal for Cow {
    fn noise(&self) -> &'static str {
        "moooooo!"
    }
}

// Trả về một vài struct mà đã implement Animal, nhưng ta sẽ không biết cụ thể struct nào tại thời điểm biên dịch (compile time).
fn random_animal(random_number: f64) -> Box<dyn Animal> {
    if random_number < 0.5 {
        Box::new(Sheep {})
    } else {
        Box::new(Cow {})
    }
}

fn main() {
    let random_number = 0.234;
    let animal = random_animal(random_number);
    println!("You've randomly chosen an animal, and it says {}", animal.noise());
}

```
