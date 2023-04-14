# Child processes

Cấu trúc `process::Output` thể hiện kết quả trả về của một tiến trình con (child process) sau khi nó kết thúc. Trong khi đó cấu trúc `process::Command` đóng vai trò là một trình xây dựng quá trình (process builder). <br/>

```rust, editable
use std::process::Command;

fn main() {
    // Khởi tạo một tiến trình con mới để thực thi lệnh `rustc --version`
    let output = Command::new("rustc")
        .arg("--version")
        .output().unwrap_or_else(|e| {
            panic!("Đã có lỗi xảy ra khi thực thi tiến trình: {}", e)
    });

    if output.status.success() {
        let s = String::from_utf8_lossy(&output.stdout);

        print!("rustc: Thực thi thành công và giá trị stdout là:\n{}", s);
    } else {
        let s = String::from_utf8_lossy(&output.stderr);

        print!("rustc: Thực thi thất bại và giá trị stderr là:\n{}", s);
    }
}
```

(Bạn nên thử lại ví dụ này trong các trường hợp xảy ra lỗi khác để hiểu rõ hơn về cách thức hoạt động của nó bằng cách truyền một cờ không chính xác tới `rustc` - Rust Compiler.) <br/>