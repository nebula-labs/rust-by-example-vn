# Threads

Rust cung cấp một cơ chế để sinh ra các luồng của hệ điều hành gốc thông qua hàm `spawn`, đối số của hàm này là một bao đóng di động.

```rust,editable
use std::thread;

const NTHREADS: u32 = 10;

// Đây là luồng `main`
fn main() {
    // Tạo một vectơ để lưu trữ các luồng con được tạo ra
    let mut children = vec![];

    for i in 0..NTHREADS {
        // Tạo ra 1 luồng khác
        children.push(thread::spawn(move || {
            println!("this is thread number {}", i);
        }));
    }

    for child in children {
        // Chờ một luồng kết thúc để trả về kết quả.
        let _ = child.join();
    }
}
```

Các chủ đề này sẽ được lên lịch bởi hệ điều hành.
