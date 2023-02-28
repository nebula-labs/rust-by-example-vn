# Build Scripts

Đôi khi một bản build bình thường từ `cargo` là không đủ. Crate của bạn có lẽ cần một số điều kiện tiên quyết trước khi `cargo` sẽ biên dịch thành công, những thứ như là tạo mã, hoặc một số mã gốc cần được biên dịch. Để giải quyết vấn đề này chúng tôi đã thêm các build script mà Cargo có thể chạy.

Để thêm một build script vào gói của bạn, nó có thể được chỉ định trong 
`Cargo.toml` như sau:

```toml
[package]
...
build = "build.rs"
```
Nếu không, Cargo sẽ tìm tệp `build.rs` trong thư mục dự án theo mặc định.

## Cách sử dụng một build script

Build script chỉ đơn giản là một tệp Rust khác sẽ được biên dịch và gọi trước khi biên dịch bất cứ thứ gì đó trong package(gói).
Do đó, nó có thể được sử dụng để đáp ứng các điều kiện tiên quyết trong crate của bạn.

Cargo cung cấp script với những input qua biến môi trường [được chỉ định ở đây] để có thể sử dụng.

Script cung cấp output qua stdout. Tất cả các dòng in ra được ghi vào `target/debug/build/<pkg>/output`. Hơn nữa, những dòng với tiền tố là `cargo:` sẽ được Cargo thông dịch trực tiếp và do đó có thể được sử dụng để xác định các tham số cho quá trình biên dịch của gói.

Để biết thêm thông số kỹ thuật và ví dụ, hãy đọc [Cargo specification][cargo_specification].

[được chỉ định ở đây]: https://doc.rust-lang.org/cargo/reference/environment-variables.html#environment-variables-cargo-sets-for-build-scripts

[cargo_specification]: https://doc.rust-lang.org/cargo/reference/build-scripts.html