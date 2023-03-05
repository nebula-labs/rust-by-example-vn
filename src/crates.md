# Crates

Một crate là một đơn vị biên dịch trong Rust. Bất cứ khi nào `rustc some_file.rs` được gọi, `some_file.rs` được xem như _tập tin crate_. Nếu `some_file.rs` có các khai báo `mod` bên trong nó, thì nội dung của các tập tin module sẽ được chèn vào những nơi khai báo `mod` trong tập tin crate được tìm thấy, _trước khi_ chạy trình biên dịch nó. Nói cách khác, các module _không_ được biên dịch một cách riêng lẻ, mà chỉ các crate mới được biên dịch.

Một crate có thể được biên dịch thành một mã nhị phân hoặc thành một thư viện. Mặc định, `rustc` sẽ tạo ra một mã nhị phân từ một crate. Hành vi này có thể bị ghi đè bằng cách truyền tham số `--crate-type` đến `lib`.
