# Generics

*Generics* là chủ đề về việc khái quát hóa các kiểu dữ liệu và các hàm trong các trường hợp bao quát hơn. Điều này rất hữu ích cho việc giảm sự trùng lặp của mã theo nhiều cách, nhưng lại yêu cầu cú pháp phức tạp hơn. Cụ thể, việc sử dụng generic bắt buộc phải được chú ý rất kĩ để xác định các kiểu dữ liệu nào là kiểu generic được xem xét hợp lệ. Việc sử dụng generic đơn giản và phổ biến nhất là dùng các tham số có dạng kiểu dữ liệu.

Tham số có dạng kiểu dữ liệu được chỉ định là generic bằng cách sử dụng dấu ngoặc nhọn và [Camel case][camelcase]: `<Aaa, Bbb, ...>`. "Tham số kiểu generic" thường được biểu diễn dưới dạng `<T>`. Trong Rust, "generic" cũng miêu tả bất cứ thực thể gì chấp nhận một hoặc nhiều tham số kiểu generic, có dạng `<T>`. Bất kỳ kiểu nào được chỉ định là tham số kiểu generic đều mang tính khái quát, và tất cả những kiểu còn lại phải cụ thể (không generic).

Ví dụ, định nghĩa một *hàm generic* `foo` nhận đối số `T` đối với bất kỳ kiểu nào:

```rust,ignore
fn foo<T>(arg: T) { ... }
```

Do `T` đã được chỉ định là tham số kiểu generic bằng cách sử dụng `<T>`, nó được coi là khái quát khi sử dụng ở đây như là `(arg: T)`. Trường này đúng ngay cả khi `T` đã được định nghĩa trước đó như là một struct.

Ví dụ này thể hiện một số cú pháp trong thực tế:

```rust,editable
// Một kiểu dữ liệu riêng biệt `A`.
struct A;

// Khi định nghĩ kiểu `Single`, lần sử dụng đầu tiên của `A` không được đi kèm với `<A>`.
// Vì thế, `Single` là một kiểu dữ liệu riêng biệt, and kiểu `A` ở trên.
struct Single(A);
//            ^ Đây là lúc `Single` lần đầu sử dụng kiểu `A`.

// Ở đây, `<T>` đi kèm với lần sử dụng đầu tiên của `T`, vì vậy `SingleGen` là một kiểu dữ liệu generic.
// Bởi vì tham số kiểu `T` là khái quát, nó có thể là bất cứ thứ gì, bao gồm 
// cả kiểu dữ liệu riêng biệt A được định nghĩa ở trên.

struct SingleGen<T>(T);

fn main() {
    // `Single` là kiểu dữ liệu cụ thể và rõ ràng có tham số A.
    let _s = Single(A);
    
    // Tạo một biến `_char` có kiểu `SingleGen<char>`
    // và gán nó với giá trị `SingleGen('a')`.
    // Trong trường hợp này, `SingleGen` có một tham số đã được chỉ định rõ ràng.
    let _char: SingleGen<char> = SingleGen('a');

    // Kiểu `SingleGen` cũng có thể chứa kiểu tham số được ngầm chỉ định
    let _t    = SingleGen(A); // Sử dụng `A` được xác định ở trên.
    let _i32  = SingleGen(6); // Sử dụng `i32`.
    let _char = SingleGen('a'); // Sử dụng `char`.
}
```

### Tham khảo:

[`structs`][structs]

[structs]: custom_types/structs.md
[camelcase]: https://en.wikipedia.org/wiki/CamelCase
