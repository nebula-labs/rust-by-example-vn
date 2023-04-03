# Định danh (designators)

Các đối số của một macro sẽ được đánh dấu bằng tiền tố dấu đô la `$` và chú thích với kiểu bằng designator.

```rust,editable
macro_rules! create_function {
    // Macro này nhận đối số với designator là `ident` và
    // tạo ra một hàm có tên là `$func_name`
    // Designator `ident` được dùng cho tên của biến hoặc là tên của hàm
    ($func_name:ident) => {
        fn $func_name() {
            // Macro `stringify!` chuyển đổi một biến với định danh là `ident` thành chuỗi string.
            println!("You called {:?}()",
                     stringify!($func_name));
        }
    };
}
// Tạo ra một hàm tên là `foo` và `bar` với macro được định nghĩa ở trên.
create_function!(foo);
create_function!(bar);
macro_rules! print_result {
    // Macro này sẽ nhận vào một biểu thức (expression) với kiểu là `expr` và sẽ in
    // biểu thức đó ra dưới dạng chuỗi string và kèm theo đó là kết quả của nó
    // Designator `expr` được dùng cho các biểu thức (expressions)
    ($expression:expr) => {
        // Macro `stringify!` sẽ chuyển biểu thức (expression) thành chuỗi string
        println!("{:?} = {:?}",
                 stringify!($expression),
                 $expression);
    };
}
fn main() {
    foo();
    bar();
    print_result!(1u32 + 1);
    // Nhắc lại là các blocks cũng là các biểu thức (expressions)
    print_result!({
        let x = 1u32;
        x * x + 2 * x - 1
    });
}
```

Dưới đây là một vài Designator có sẵn:

- `block`
- `expr` được dùng cho các biểu thức (expressions)
- `ident` được dùng cho tên của biến hoặc hàm
- `item`
- `literal` được dùng cho các hằng số literal (literal constants)
- `pat` (_pattern_)
- `path`
- `stmt` (_statement_)
- `tt` (_token tree_)
- `ty` (_type_)
- `vis` (_visibility qualifier_)

Nếu muốn coi một danh sách đầy đủ của các Designator, hãy xem ở đây [Rust Reference]

[rust reference]: https://doc.rust-lang.org/reference/macros-by-example.html
