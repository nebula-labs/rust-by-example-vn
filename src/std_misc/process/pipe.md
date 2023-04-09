# Pipes

`std::Child` struct đại diện cho một tiến trình con đang chạy, và đưa ra các điều khiển
`stdin`, `stdout` và `stderr` để tương tác với tiến trình thông qua pipes.

```rust,ignore
use std::io::prelude::*;
use std::process::{Command, Stdio};

static PANGRAM: &'static str =
"the quick brown fox jumped over the lazy dog\n";

fn main() {
    // Khởi tạo câu lệnh `wc`
    let process = match Command::new("wc")
                                .stdin(Stdio::piped())
                                .stdout(Stdio::piped())
                                .spawn() {
        Err(why) => panic!("couldn't spawn wc: {}", why),
        Ok(process) => process,
    };

    // Truyền một chuỗi vào `stdin` của `wc`.
    //
    // `stdin` có kiểu là `Option<ChildStdin>`, nhưng vì chúng ta biết chắc rằng có một thể hiện của stdin
    // must have one, we can directly `unwrap` it.
    match process.stdin.unwrap().write_all(PANGRAM.as_bytes()) {
        Err(why) => panic!("couldn't write to wc stdin: {}", why),
        Ok(_) => println!("sent pangram to wc"),
    }

    // Bởi vì `stdin` không tồn tại sau khi được gọi ở trên,
    // nó được loại bỏ và pipe được đóng lại
    //
    // Điều này rất quan trọng, nếu không thì `wc` sẽ không bắt đầu xử lí dữ liệu được nhập vào

    // `stdout` cũng có kiểu là `Option<ChildStdout>` nên cũng phải được unwrap.
    let mut s = String::new();
    match process.stdout.unwrap().read_to_string(&mut s) {
        Err(why) => panic!("couldn't read wc stdout: {}", why),
        Ok(_) => print!("wc responded with:\n{}", s),
    }
}
```
