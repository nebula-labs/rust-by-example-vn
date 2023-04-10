# File I/O

Cấu trúc `File` đại diện cho một tập tin đã được mở (nó bao gồm một bộ chỉ số tập tin), 
và cung cấp quyền đọc and/or ghi vào tập tin cơ sở.

Bởi vì nhiều điều có thể xảy ra sai sót khi thực hiện I/O trên tập tin, 
tất cả các phương thức của `File` đều trả về kiểu `io::Result<T>`, 
đó là một tên viết tắt cho `Result<T, io::Error>`.

Điều này làm cho việc thất bại của tất cả các hoạt động I/O trở nên rõ ràng(*explicit*). 
Nhờ điều này, lập trình viên có thể thấy được tất cả các đường dẫn thất bại và 
được khuyến khích xử lý chúng một cách chủ động.