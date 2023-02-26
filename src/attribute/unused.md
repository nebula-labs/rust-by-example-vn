# dead_code

Trình biên dịch cung cấp một dead_code [lint](https://en.wikipedia.org/wiki/Lint_%28software%29) sẽ cảnh báo về các hàm không được sử dụng. Một thuộc tính có thể được sử dụng để vô hiệu hóa lint.

```rust,mdbook-runnable,editable
fn used_function() {}

// `#[allow(dead_code)]` là một thuộc tính vô hiệu hóa `dead_code` lint
#[allow(dead_code)]
fn unused_function() {}

fn noisy_unused_function() {}
// FIXME ^ Thêm một thuộc tính để  vô hiệu hóa cảnh báo

fn main() {
    used_function();
}
```
Lưu ý rằng trong các chương trình thực tế, bạn nên loại bỏ dead code. Trong những ví dụ trên chúng tôi sẽ cho phép dead code ở một số nơi bởi do tính chất tương tác của các ví dụ.