# Rust by Example

[Rust][rust] là một ngôn ngữ lập trình hệ thống hiện đại tập trung vào tính an toàn, tốc độ và đồng thời. 
Nó đạt đươc các tiêu chí này bằng cách làm bộ nhớ trở nên an toàn mà không sử dụng đến garbage collection (bộ thu gom rác).

Rust by Example (RBE) là một tập các ví dụ minh họa cho các khái niệm và thư viện tiêu chuẩn của Rust. Để hiểu thêm về
những ví dụ này, đừng quên [cài đặt ngôn ngữ Rust][install] và xem [tài liệu chính thức][std].
Thêm vào đó, nếu bạn tò mò, bạn cũng có thể [kiểm tra mã nguồn của trang web này][home-vn] hoặc [mã nguồn của bản gốc tiếng Anh][home-en].

Nào, chúng ta cùng bắt đầu!

- [Hello World](hello.md) - Bắt đầu với chương trình Hello World truyền thống.

- [Primitives](primitives.md) - Học về số nguyên có dấu, số nguyên không dấu và các kiểu dữ liệu nguyên thủy khác. 

- [Custom Types](custom_types.md) - `struct` và `enum`.

- [Variable Bindings](variable_bindings.md) - Học về mutable bindings, scope và shadowing.

- [Types](types.md) - Học về cách thay đổi và định nghĩa các kiểu dữ liệu.

- [Conversion](conversion.md)

- [Expressions](expression.md)

- [Flow of Control](flow_control.md) - `if`/`else`, `for`, and các luồng điều khiển khác.

- [Functions](fn.md) - Học về Methods, Closures và High Order Functions. 

- [Modules](mod.md) - Tổ chức mã code của bạn bằng cách sử dụng các modules.

- [Crates](crates.md) - Một crate là một đơn vị biên dịch trong Rust. Học về cách tạo library (thư viện).

- [Cargo](cargo.md) - Xem qua một số tính năng cơ bản của Cargo - công cụ quản lý các package Rust chính thức.

- [Attributes](attribute.md) - Attribute (Thuộc tính) là metadata (siêu dữ liệu) được áp dụng cho một số module, crate hoặc item.

- [Generics](generics.md) - Tìm hiểu về cách viết một hàm hoặc kiểu dữ liệu có khả năng hoạt động với nhiều loại đối số.

- [Scoping rules](scope.md) - Scope (Phạm vi) đóng một phần quan trọng trong cái khái niệm về ownership (quyền sở hữu), borrowing (mượn quyền) và lifetimes (vòng đời).

- [Traits](trait.md) - Một trait là một tập các methods được xác định cho một kiểu không xác định (unknown type): `Self`

- [Macros](macros.md)

- [Error handling](error.md) - Học cách Rust xử lý lỗi.

- [Std library types](std.md) - Học về một vài custom types (kiểu dữ liệu tùy chỉnh) được cung cấp bởi `std` library.

- [Std misc](std_misc.md) - Học thêm về một vài kiểu dữ liệu tùy chỉnh cho việc xử lý tệp (file) và luồng (threads).

- [Testing](testing.md) - Tất cả các loại thử nghiệm chương trình trong Rust.

- [Unsafe Operations](unsafe.md)

- [Compatibility](compatibility.md)

- [Meta](meta.md) - Documentation (Tài liệu) và Benchmarking (Đo điểm chuẩn).

[rust]: https://www.rust-lang.org/
[install]: https://www.rust-lang.org/tools/install
[std]: https://doc.rust-lang.org/std/
[home-en]: https://github.com/rust-lang/rust-by-example
[home-vn]: https://github.com/EyesCrypto-Insights/rust-by-example-vn