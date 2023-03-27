# Domain Specific Languages (DSLs)

Một DSL là một "ngôn ngữ mini" được nhúng vào một macro. Chúng hoàn toàn là hợp lệ
trong Rust bởi vì hệ thống macro mở rộng thành cấu trúc Rust bình thường, nhưng chúng
trông giống một "ngôn ngữ nhỏ". Điều này cho phép bạn xác định cú pháp ngắn gọn hoặc
trực quan cho một số chức năng đặc biệt (trong giới hạn).

Giả sử khi chúng ta muốn tạo ra một API dùng để tính toán. Chúng ta muốn cung cấp
một phép toán và kết quả sẽ được in ra console.

```rust,editable
macro_rules! calculate {
    (eval $e:expr) => {
        {
            let val: usize = $e; // Băt buộc kiểu dữ liệu phải là số nguyên
            println!("{} = {}", stringify!{$e}, val);
        }
    };
}

fn main() {
    calculate! {
        eval 1 + 2 // `eval` không phải là một từ khóa của Rust!
    }

    calculate! {
        eval (1 + 2) * (3 / 4)
    }
}
```

Kết quả:

```txt
1 + 2 = 3
(1 + 2) * (3 / 4) = 0
```

Đây là một ví dụ rất đơn giản, nhưng nhiều interface phức tạp hơn đang được phát
triển như là [`lazy_static`](https://crates.io/crates/lazy_static) hoặc
[`clap`](https://crates.io/crates/clap).

Ngoài ra, cần lưu ý hai cặp dấu ngoặc nhọn trong macro. Cặp bên ngoài là
một phần của cú pháp của `macro_rules!`, bên cạnh `()` hoặc `[]`.
