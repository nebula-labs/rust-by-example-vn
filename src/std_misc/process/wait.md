# Wait

Nếu bạn muốn đợi một `process::Child` kết thúc, thì phải gọi
`Child::wait`, sẽ trả về một `process::ExitStatus`.

```rust,ignore
use std::process::Command;

fn main() {
    let mut child = Command::new("sleep").arg("5").spawn().unwrap();
    let _result = child.wait().unwrap();

    println!("reached end of main");
}
```

```bash
$ rustc wait.rs && ./wait
# `wait` tiếp tục chạy thêm 5 giây cho tới khi câu lệnh `sleep 5` kết thúc
reached end of main
```
