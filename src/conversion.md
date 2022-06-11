# Conversion (Chuyển đổi)

Các kiểu dữ liệu nguyên thủy có thể chuyển đổi lẫn nhau thông qua [casting] (ép kiểu).

Rust xử lý việc chuyển đổi giữa các kiểu dữ liệu tùy chỉnh (ví dụ như `struct` và `enum`)
bằng việc sử dụng [traits]. Việc chuyển đổi nói chung là
sẽ sử dụng [`From`] và [`Into`] traits. Tuy nhiên, có những `traits` cụ thể cho các trường 
hợp phổ biến hơn, đặc biệt là khi chuyển đổi từ và sang `String`.

[casting]: types/cast.md
[traits]: trait.md
[`From`]: https://doc.rust-lang.org/std/convert/trait.From.html
[`Into`]: https://doc.rust-lang.org/std/convert/trait.Into.html

