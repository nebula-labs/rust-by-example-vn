# Argument parsing

Matching (`match`) có thể được sử dụng để phân tích cú pháp (parse) các tham số đơn giản (simple arguments) <br/>

```rust,editable
use std::env;

fn increase(number: i32) {
    println!("{}", number + 1);
}

fn decrease(number: i32) {
    println!("{}", number - 1);
}

fn help() {
    // Hiển thị thông báo hướng dẫn sử dụng: 
    // Có 2 tính năng chính:
    // 1. Kiểm tra xem giá trị truyền vào có phải là number và là đáp án hay không, trong ví dụ là số 42
    // 2. Tăng hoặc giảm giá trị truyền vào 1 đơn vị
    println!("usage:
match_args <string>
    Kiểm tra xem giá trị truyền vào có đúng là đáp án hay không.
match_args {{increase|decrease}} <integer>
    Tăng hoặc giảm giá trị truyền vào một đơn vị.");
}

fn main() {
    let args: Vec<String> = env::args().collect();

    match args.len() {
        // Không có tham số nào được truyền vào
        1 => {
            println!("Tên tôi là 'match_args'. Hãy thử truyền thêm các tham số vào câu lệnh!"); 
        },
        // Truyền vào 1 tham số đầu tiên
        2 => {
            match args[1].parse() {
                Ok(42) => println!("Đáp án chính xác!"),
                _ => println!("Đây không phải đáp án."),
            }
        },
        // 1 câu lệnh và 1 tham số được truyền vào
        3 => {
            let cmd = &args[1];
            let num = &args[2];
            // Phân giải giá trị truyền vào -> check xem có phải number không
            let number: i32 = match num.parse() {
                Ok(n) => {
                    n
                },
                Err(_) => {
                    eprintln!("Lỗi: Tham số truyền vào không phải là số nguyên (interger)");
                    help();
                    return;
                },
            };
            // Phân giải câu lệnh, kiểm tra tính đúng đắn
            match &cmd[..] {
                "increase" => increase(number),
                "decrease" => decrease(number),
                _ => {
                    eprintln!("Lỗi: Câu lệnh không hợp lệ");
                    help();
                },
            }
        },
        // Các trường hợp khác
        _ => {
            // Hiển thị thông báo hướng dẫn
            help();
        }
    }
}
```

```bash
$ ./match_args Rust
Đây không phải đáp án.
$ ./match_args 42
Đáp án chính xác!
$ ./match_args do something
Lỗi: Tham số truyền vào không phải là số nguyên (interger)
usage:
match_args <string>
    Kiểm tra xem giá trị truyền vào có đúng là đáp án hay không.
match_args {increase|decrease} <integer>
    Tăng hoặc giảm giá trị truyền vào một đơn vị.
$ ./match_args do 42
error: invalid command
usage:
match_args <string>
    Kiểm tra xem giá trị truyền vào có đúng là đáp án hay không.
match_args {increase|decrease} <integer>
    Tăng hoặc giảm giá trị truyền vào một đơn vị.
$ ./match_args increase 42
43
```
