# Error handling

Xử lý lỗi là quá trình xử lý khả năng lỗi của một chương trình. Ví dụ, lỗi không thể đọc được một tệp tin và sau đó tiếp tục sử dụng đầu vào là *bad*, rõ ràng nó sẽ gây ra vấn đề. Phát hiện và quản lý rõ ràng các lỗi đó giúp bảo vệ chương trình từ những mối nguy hiểm khác. 

Có nhiều cách khác nhau để xử lý lỗi trong Rust, được mô tả trong các phần con dưới đây. Tất cả chúng đều có ít hoặc nhiều sự khác biệt và khác cách sử dụng. Như một quy tắc chung:

Việc sử dụng `panic` rõ ràng chỉ hữu ích cho việc kiểm thử(test) và xử lý các lỗi không thể khắc phục được. Trong quá trình tạo mẫu thì nó có thể rất hữu ích nhưng trong các trường hợp này thì `unimplemented` cung cấp thông tin mô tả rõ ràng hơn. Trong các bài kiểm thử, `panic` là một cách hợp lý để thể hiện sự thất bại rõ ràng.

Kiểu `Option` được sử dụng khi giá trị là tùy chọn hay trong trường hợp thiếu giá trị của câu một điều kiện lỗi. Ví dụ như thư mục gốc - `/` và `C:` không có một thư mục cha nào. Khi xử lý với kiểu `Option`, `unwrap` là hợp lý trong quá trình tạo mẫu và các trường hợp mà chắc chắn sẽ có một giá trị hợp lệ. Tuy nhiên `expect` sẽ hữu ích hơn vì nó cho phép bạn chỉ định một thông báo lỗi trong trường hợp có lỗi xảy ra.

Khi có khả năng xảy ra lỗi và người gọi phải xử lý vấn đề, sử dụng `Result`. Bạn cũng có thể sử dụng `unwrap` và `expect` (vui lòng không làm điều này trừ khi đó là một bài kiểm thử hoặc một nguyên mẫu nhanh(quick prototype)).

Để biết thêm thông tin chi tiết về xử lý lỗi, xin tham khảo phần xử lý lỗi trong [official book][book].

[book]: https://doc.rust-lang.org/book/ch09-00-error-handling.html