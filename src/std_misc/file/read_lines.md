# `read_lines`

## Một cách tiếp cận ngây thơ.

Đây có thể là một nỗ lực hợp lý đầu tiên cho việc đọc 
các dòng từ một tệp tin đối với một người mới bắt đầu.

```rust,norun
use std::fs::read_to_string;

fn read_lines(filename: &str) -> Vec<String> {
    let mut result = Vec::new();

    for line in read_to_string(filename).unwrap().lines() {
        result.push(line.to_string())
    }

    result
}
```

Vì phương thức `lines()` trả về một bộ lặp qua các dòng trong tập tin, 
chúng ta cũng có thể thực hiện một phép ánh xạ trực tiếp và thu thập các kết quả, 
tạo ra một biểu thức ngắn gọn và trôi chảy hơn

```rust,norun
use std::fs::read_to_string;

fn read_lines(filename: &str) -> Vec<String> {
    read_to_string(filename) 
        .unwrap()  // panic khi có lỗi đọc tệp có thể xảy ra.
        .lines()  // chia chuỗi thành một terator of string slices 
        .map(String::from)  // chuyển mỗi phần của chuỗi thành một chuỗi mới
        .collect()  // tập hợp chúng lại thành một vector
}
```

Lưu ý rằng trong cả hai ví dụ trên, chúng ta phải chuyển đổi tham chiếu `&str` 
được trả về từ `lines()` thành kiểu được sở hữu `String`, sử dụng `.to_string()` và 
`String::from` tương ứng.

## Một cách tiếp cận hiệu quả hơn

Ở đây chúng ta truyền sở hữu của `File` đã mở vào một ` struct. ` BufReader. 
`BufReader` sử dụng bộ đệm nội bộ để giảm thiểu các phân bổ trung gian.
Chúng ta cũng cập nhật `read_lines` để trả về một bộ lặp thay vì 
phân bổ các đối tượng `String` mới trong bộ nhớ cho mỗi dòng.

```rust,no_run
use std::fs::File;
use std::io::{self, BufRead};
use std::path::Path;

fn main() {
    // Tệp tin hosts.txt phải tồn tại trong đường dẫn hiện tại.
    if let Ok(lines) = read_lines("./hosts.txt") {
        // Sử dụng iterator, trả về một Chuỗi (Tùy chọn)
        for line in lines {
            if let Ok(ip) = line {
                println!("{}", ip);
            }
        }
    }
}

//  Kết quả được bọc trong một Result để cho phép phù hợp với các lỗi
//  Trả về một Iterator đến Reader của các dòng trong tập tin.
fn read_lines<P>(filename: P) -> io::Result<io::Lines<io::BufReader<File>>>
where P: AsRef<Path>, {
    let file = File::open(filename)?;
    Ok(io::BufReader::new(file).lines())
}
```

Chạy chương trình này đơn giản chỉ in các dòng một cách riêng lẻ.
```shell
$ echo -e "127.0.0.1\n192.168.0.1\n" > hosts.txt
$ rustc read_lines.rs && ./read_lines
127.0.0.1
192.168.0.1
```

(Lưu ý rằng vì `File::open` mong đợi một AsRef<Path> chung như đối số, chúng ta xác định 
phương thức `read_lines()` chung của mình với cùng một ràng buộc chung, sử dụng từ khóa `where`.)
Quá trình này hiệu quả hơn việc tạo `String` trong bộ nhớ với tất cả nội dung của tập tin. 
Điều này đặc biệt có thể gây ra vấn đề hiệu suất khi làm việc với các tập tin lớn hơn.

