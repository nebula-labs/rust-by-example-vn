# Attributes

Một thuộc tính là metadata(siêu dữ liệu) áp dụng cho 1 số module, crate hoặc mục nào đó. Metadata này có thể được sử dụng để/cho:

<!-- TODO: Link these to their respective examples -->

* [biên dịch mã có điều kiện][cfg]
* [đặt tên crate, phiên bản và loại (một tệp kiểu nhị phân hay là một thư viện)][crate]
* vô hiệu hóa [lints][lint] (những cảnh báo)
* bật các tính năng của trình biên dịch (macros, glob imports, etc.)
* liên kết đến một thư viện ngoài
* đánh dấu các chức năng như là unit tests
* đánh dấu các chức năng sẽ là một phần của benchmark
* [thuộc tính như macro][macros]

Khi các thuộc tính áp dụng cho toàn bộ crate, cú pháp của nó là `#![crate_attribute]`, và khi chúng áp dụng cho một module hoặc mục, cú pháp là `#[item_attribute]`(bỏ đi `!`).

Các thuộc tính có thể nhận các đối số với những cú pháp khác nhau:

* `#[attribute = "value"]`
* `#[attribute(key = "value")]`
* `#[attribute(value)]`

Các thuộc tính có thể có nhiều giá trị và cũng có thể được phân tách trên nhiều dòng:

```rust,ignore
#[attribute(value, value2)]


#[attribute(value, value2, value3,
            value4, value5)]
```

[cfg]: attribute/cfg.md
[crate]: attribute/crate.md
[lint]: https://en.wikipedia.org/wiki/Lint_%28software%29
[macros]: https://doc.rust-lang.org/book/ch19-06-macros.html#attribute-like-macros