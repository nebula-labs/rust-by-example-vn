# Dependencies

Hầu hết các chương trình đều phụ thuộc vào một vài thư viện nào đó. Nếu bạn đã từng tự tay quản lí các thư viện phụ thuộc này, bạn sẽ biết việc này có thể rất phiền toái. May mắn thay, hệ sinh thái Rust đi kèm với công cụ cargo! cargo có thể quản lý các phụ thuộc cho một dự án.

Để tạo mới một project Rust,

```rust,mdbook-runnable,editable
# một tệp nhị phân
cargo new foo

# tạo thư viện
cargo new --lib bar
```

Trong phần tiếp theo của chương này, giả sử rằng chúng ta đang tạo 1 tập nhị phân, chứ không phải một thư viện, song các khái niệm là giống nhau.

Sau khi chạy các câu lệnh phía trên, bạn sẽ thấy một hệ thống phân cấp tệp tin như sau: 

```rust,mdbook-runnable,editable
.
├── bar
│   ├── Cargo.toml
│   └── src
│       └── lib.rs
└── foo
    ├── Cargo.toml
    └── src
        └── main.rs
```
`main.rs` là tệp tin gốc của dự án `foo` vừa được tạo - không có gì mới ở đây. `Cargo.toml` là tệp tin chứa cấu hình của `cargo` đối với dự án này. Nếu bạn xem bên trong tệp tin, bạn sẽ thấy giống như thế này: 
```rust
[package]
name = "foo"
version = "0.1.0"
authors = ["mark"]

[dependencies]
```
Trường `name` bên dưới `[package]` xác định tên của dự án. Tên dự án cũng được sử dụng nếu bạn xuất bản crate lên `crates.io`. Nó cũng là tên của tệp nhị phân đầu ra khi được biên dịch.

Trường `version` xác định phiên bản của crate theo quy chuẩn [Semantic Versioning](https://semver.org/).

Trường `authors` là một danh sách các tác giả được sử dụng khi xuất bản crate.

Phần `[dependencies]` để bạn tùy ý thêm các thư viện phụ thuộc vào dự án của mình.

Ví dụ, giả sử rằng chúng ta muốn chương trình của mình có một CLI tuyệt vời. Bạn có thể tìm kiếm thấy rất nhiều package trên [crates.io](https://crates.io/) (Địa chỉ chính thức của hệ thống đăng ký gói của Rust). Một lựa chọn phổ biến là [clap](https://crates.io/crates/clap). Tại thời điểm viết bài này, phiên bản mới nhất của `clap` được công bố là `2.27.1`. Để thêm một phục thuộc cho chương trình của mình, chúng ta đơn giản chỉ cần thêm vào tệp `Cargo.toml` ở phía dưới phần `dependencies`:`clap = "2.27.1"`.  Và thế là xong. Bạn có thể bắt đầu sử dụng `clap` trong chương trình của mình.

`cargo` cũng hỗ trợ [các kiểu dependency khác nữa](https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html). Ở đây là một thí dụ nhỏ:

```rust
[package]
name = "foo"
version = "0.1.0"
authors = ["mark"]

[dependencies]
clap = "2.27.1" # from crates.io
rand = { git = "https://github.com/rust-lang-nursery/rand" } # từ repo online.
bar = { path = "../bar" } # từ một tệp dẫn trong hệ thống tệp cục bộ.
```
`cargo` không chỉ là một trình quản lí các dependency. Tất cả các tùy chọn cấu hình đều có sẵn và được liệt kê trong [đặc tả định dạng](https://doc.rust-lang.org/cargo/reference/manifest.html) của `Cargo.toml`

Để  build project của mình chúng ta có thể  thực thi bằng câu lệnh `cargo build` ở bất kì đâu trong thư mục project ( ngay cả trong các thư mục con). Chúng ta có thể chạy `cargo run` để build và run. Lưu ý rằng những câu lệnh này sẽ giải quyết tất cả các dependency, tải các crate nếu cần, và build tất cả mọi thứ, bảo gồm crate của mình. ( Lưu ý rằng nó chỉ build lại những gì mà nó chưa được build, tương tự như `make`).

Thì đấy! Đó là tất cả về dependency.