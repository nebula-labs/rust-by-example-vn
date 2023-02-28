# Conventions

Trong chương trước, chúng ta đã thấy cấu trúc thư mục phân cấp như sau:

```txt
foo
├── Cargo.toml
└── src
    └── main.rs
```

Tuy nhiên, giả sử rằng chúng ta muốn có hai tệp nhị phân trong cùng một dự án. Vậy thì sao?

`cargo` hỗ trợ điều này. Tên tệp nhị phân mặc định là `main`, như là chúng ta đã thấy trước đó, nhưng bạn có thể thêm những tệp nhị phân bổ sung bằng cách đặt chúng vào trong thư mục `bin/`:

```txt
foo
├── Cargo.toml
└── src
    ├── main.rs
    └── bin
        └── my_other_bin.rs
```

Để yêu cầu `cargo` chỉ biên dịch hay chạy tệp nhị phân này, chúng ta chỉ cần chạy `cargo` với cờ `--bin my_other_bin`, trong đó `my_other_bin` là tên của tệp nhị phân mà chúng ta muốn làm việc.

Ngoài các tệp nhị phân bổ sung, `cargo` hỗ trợ [nhiều tính năng hơn] như benchmarks, tests, và các ví dụ.

Trong chương tiếp theo, chúng ta sẽ xem xét kỹ hơn các bài kiểm tra

[nhiều tính năng hơn]: https://doc.rust-lang.org/cargo/guide/project-layout.html