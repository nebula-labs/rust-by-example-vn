# Testcase: List

Việc triển khai `fmt::Display` cho một cấu trúc mà trong đó mỗi phần tử phải 
được xử lý tuần tự là một công việc khó. Vấn đề đó là mỗi một `write!` macro 
sẽ tạo ra một `fmt::Result`. Để giải quyết điều này đòi hỏi phải xử lý *tất cả* các kết quả. 
Rust cung cấp toán tử `?` cho mục đích này.

Sử dụng `?` với `write!` sẽ như sau:

```rust,ignore
// `?` sẽ thử `write!` xem nó có lỗi hay không. Nếu có lỗi,
// trả về một lỗi. Ngược lại, sẽ tiếp tục thực hiện bình thường.
write!(f, "{}", value)?;
```

Với việc sử dụng `?`, việc triển khai `fmt::Display` cho một `Vec`
trở nên thuận tiện hơn:

```rust,editable
use std::fmt; // Import `fmt` module.

// Định nghĩa một cấu trúc `List` chứa một `Vec`.
struct List(Vec<i32>);

impl fmt::Display for List {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        // Tạo ra một tham chiếu tới `vec` đầu vào.
        let vec = &self.0;

        write!(f, "[")?;

        // Lặp qua từng phần tử `v` trong `vec`, đồng thời gán index (chỉ mục) 
        // của `v` trong `count`.
        for (count, v) in vec.iter().enumerate() {
            // Với mỗi phần tử trừ phần tử đầu tiên, đều thêm một dấu phẩy ở phía sau.
            // Sử dụng toán tử ? để trả về nếu có lỗi.
            if count != 0 { write!(f, ", ")?; }
            write!(f, "{}", v)?;
        }

        // Đóng ngoặc và trả về một giá trị fmt::Result.
        write!(f, "]")
    }
}

fn main() {
    let v = List(vec![1, 2, 3]);
    println!("{}", v);
}
```

### Thực hành

Hãy thử thay đổi chương trình trên để index của mỗi phần tử trong vectơ cũng được in ra. Output mới sẽ như sau:

```rust,ignore
[0: 1, 1: 2, 2: 3]
```

### Xem thêm tại đây:

[`for`][for], [`ref`][ref], [`Result`][result], [`struct`][struct],
[`?`][q_mark], và [`vec!`][vec]

[for]: ../../../flow_control/for.md
[result]: ../../../std/result.md
[ref]: ../../../scope/borrow/ref.md
[struct]: ../../../custom_types/structs.md
[q_mark]: ../../../std/result/question_mark.md
[vec]: ../../../std/vec.md

