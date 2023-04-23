# macro_rules!

Rust cung cấp một hệ thống marco mạnh mẽ cho phép thực hiện lập trình meta. Như bạn
đã thấy ở chương trước, macro cũng giống một hàm, ngoại trừ việc tên của nó
kết thúc với dấu `!`, nhưng thay vì tạo ra một lệnh gọi hàm, macro được
chèn vào trong mã nguồn và được biên dịch với phần còn lại của chương trình.
Tuy nhiên, khác với macro trong C và những ngôn ngữ khác, macro của Rust được chèn
vào cây cú pháp trừu tượng, thay vì tiền xử lý chuỗi, vì thế bạn không gặp
phải những lỗi về độ ưu tiên.

Macro đƯợc tạo bằng cách dùng macro `macro_rules!`.

```rust,editable
// Đây là một macro đơn giản tên là `say_hello`.
macro_rules! say_hello {
    // `()` cho thấy macro này không cần bất kỳ tham số nào.
    () => {
        // Macro này sẽ chèn vào nội dung của khối lệnh này.
        println!("Hello!")
    };
}

fn main() {
    // Đoạn mã này sẽ dẫn đến lệnh `println!("Hello")`
    say_hello!()
}
```

Vậy tại sao marco lại hữu dụng?

1. Tránh lặp lại các mã tương tự nhau. Có nhiều trường hợp bạn có thể cần những chức năng tương tự
   nhau tại nhiều nơi nhưng dưới các dạng khác nhau. Thông thường, viết
   macro là cách hữu dụng nhất để tránh lặp mã nguồn. (Điều này sẽ được bàn thêm sau)

2. Ngôn ngữ miền chuyên biệt. Macro cho phép bạn định nghĩa một cú pháp đặc biệt cho
   một mục đích chuyên biệt. (Điều này sẽ được bàn thêm sau)

3. Phương thức trừu tượng bất định. Đôi lúc bạn muốn định nghĩa một phương thức trừu tượng có
   số lượng tham số linh hoạt. Một ví dụ là `println!` có thể nhận bất kỳ số lượng tham số nào,
   tùy thuộc vào định dạng của chuỗi. (Điều này sẽ được bàn thêm sau)