# Variadics

Một *variadic* interface có số lượng đối số tùy ý. Ví dụ `println!` có thể  nhận số lượng đối số tùy ý, như được xác định bởi định dạng string.

Chúng tôi có thể mở rộng macro `calculate!` từ phần trước thành variadic.

```rust
macro_rules! calculate {
    // pattern cho một `eval`
    (eval $e:expr) => {
        {
            let val: usize = $e; // Buộc các kiểu phải là số nguyên
            println!("{} = {}", stringify!{$e}, val);
        }
    };
    // Phân rã nhiều `eval` bằng cách đệ quy.
    (eval $e:expr, $(eval $es:expr),+) => {{
        calculate! { eval $e }
        calculate! { $(eval $es),+ }
    }};
}
fn main() {
    calculate! { // Nhìn ma! Variadic `calculate!`!
        eval 1 + 2,
        eval 3 + 4,
        eval (2 * 3) + 1
    }
}
```
Output:
```
1 + 2 = 3
3 + 4 = 7
(2 * 3) + 1 = 7

```