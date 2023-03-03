# Sử dụng thư viện

Để liên kết một crate đến thư viện mới này bạn có thể sử dụng cờ `--extern` của `rustc`. Tất cả các mục của nó sẽ được nhập vào dưới một module có tên giống với thư viện. Module này thường hoạt động tương tự như các module khác.

```rust,ignore
// extern crate rary; // Có thể yêu cầu phiên bản Rust 2015 edition hoặc cũ hơn

fn main() {
    rary::public_function();

    // Lỗi! `private_function` là một hàm riêng tư trong thư viện
    //rary::private_function();

    rary::indirect_access();
}
```

```txt
# Nếu library.rlib là đường dẫn đến thư viện đã được biên dịch, giả sử nó
# nằm cùng thư mục với executable.rs, bạn có thể biên dịch và chạy như sau:
$ rustc executable.rs --extern rary=library.rlib && ./executable
called rary's `public_function()`
called rary's `indirect_access()`, that
> called rary's `private_function()`
```
