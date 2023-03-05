# Rust By Example
Phiên bản tiếng Việt của https://doc.rust-lang.org/stable/rust-by-example - Học lập trình Rust thông qua các ví dụ (Có Live code editor)

Phiên bản tiếng Anh gốc ở https://github.com/rust-lang/rust-by-example

Dịch giả: Nguyễn Đức Chính - Nebula Labs <eyescryptoinsights@gmail.com>

## Cách sử dụng 
Nếu bạn muốn đọc online cuốn sách này, hãy ghé thăm [bản gốc tiếng Anh của nó tại đây][site-en].

Tạm thời phiên bản tiếng Việt này chưa có website cho bạn ghé thăm, nên bạn có thể đọc bản tiếng Việt bằng cách [cài đặt Rust], rồi sau đó chạy các lệnh sau trong Terminal:

```bash
$ git clone https://github.com/nebula-labs/rust-by-example-vn.git
$ cd rust-by-example-vn
$ cargo install mdbook
$ mdbook build
$ mdbook serve
```

Để có thể chạy được code của các ví dụ, bạn phải kết nối với internet. Tuy nhiên, bạn cũng có thể đọc tất cả nội dung ngoại tuyến, ngoại trừ việc chạy code.

[site-en]: https://doc.rust-lang.org/stable/rust-by-example
[cài đặt Rust]: https://www.rust-lang.org/tools/install

## Tiến độ

Có 198 file, query bằng cách `find . -type f -name "*.md" | wc -l`.

Ở thời điểm 01/03/2022, có 3 người cùng dịch, mỗi người dịch 2 file, tốc độ sẽ là 6 file/tuần. Như vậy sẽ mất khoảng hơn 8 tháng để dịch xong. Với thêm 2 người nữa, bản dịch này sẽ chỉ mất 5 tháng. 8 người dịch sẽ đưa thời gian xuống còn 3 tháng.