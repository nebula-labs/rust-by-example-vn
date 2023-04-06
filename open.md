# `open`

Hàm `open` có thể được sử dụng để mở một tập tin ở chế độ chỉ được đọc.

Môt `File` sở hữu một tài nguyên, đó là mô tả tập tin và sẽ tự động đóng khi không còn (sử dụng) `drop`.

```rust,editable,ignore
use std::fs::File;
use std::io::prelude::*;
use std::path::Path;

fn main() {
    // Tạo đường dẫn đến tập tin mong muốn
    let path = Path::new("hello.txt");
    let display = path.display();

    // Mở đường dẫn theo chế độ chỉ đọc, trả về `io::Result<File>`
    let mut file = match File::open(&path) {
        Err(why) => panic!("couldn't open {}: {}", display, why),
        Ok(file) => file,
    };

    // Đọc nội dung tập tin thành một chuỗi, trả về `io::Result<usize>`
    let mut s = String::new();
    match file.read_to_string(&mut s) {
        Err(why) => panic!("couldn't read {}: {}", display, why),
        Ok(_) => print!("{} contains:\n{}", display, s),
    }

    // `file` ra ngoài phạm vi, và tập tin sẽ bị đóng lại
    
}
```

Dưới đây là kết quả thành công mong đợi:

```shell
$ echo "Hello World!" > hello.txt
$ rustc open.rs && ./open
hello.txt contains:
Hello World!
```

(Bạn được khuyến khích thử nghiệm ví dụ trước đó trong các điều kiện thất bại 
khác nhau: hello.txt không tồn tại, hoặc hello.txt không đọc được, vv.)