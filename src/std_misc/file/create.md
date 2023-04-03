# create
Hàm `create` sẽ mở tập tin ở chế độ chỉ ghi (write-only mode). Nếu tập tin này đã tồn tại, dữ liệu trong tập tin sẽ bị mất. Ở chiều ngược lại, nếu tập tin chưa tồn tại thì một tập tin mới sẽ được tạo ra. <br/>

```rust,editable
static WHAT_IS_RUST: &str =
    "Rust là ngôn ngữ lập trình được tạo ra vào năm 2006 bởi Graydon Hoare như một dự án phụ khi đang là developer tại Mozilla.  Rust pha trộn hiệu suất của các ngôn ngữ như C ++ với cú pháp thân thiện hơn, tập trung vào code an toàn và được thiết kế tốt giúp đơn giản hóa việc phát triển. Các phần của trình duyệt Firefox của Mozilla được viết bằng Rust và các nhà phát triển tại Microsoft được cho là sử dụng nó để mã hóa lại các phần của hệ điều hành Windows.
";

use std::fs::File;
use std::io::prelude::*;
use std::path::Path;

fn main() {
    let path = Path::new("what_is_rust.txt");
    let display = path.display();

    // Mở một file mới ở chế độ chỉ ghi, trả về `io::Result<File>`
    let mut file = match File::create(&path) {
        Err(why) => panic!("Không thể khởi tạo tập tin {}: {}", display, why),
        Ok(file) => file,
    };

    // Ghi chuỗi `WHAT_IS_RUST` vào `file`, trả về `io::Result<()>`
    match file.write_all(WHAT_IS_RUST.as_bytes()) {
        Err(why) => panic!("Không thể ghi vào tập tin {}: {}", display, why),
        Ok(_) => println!("Ghi thành công vào tập tin {}", display),
    }
}
```

Dưới đây sẽ là kết quả trả về trong trường hợp thực thi thành công:<br/>

```bash
$ rustc create.rs && ./create
Ghi thành công vào tập tin what_is_rust.txt
$ cat what_is_rust.txt
Rust là ngôn ngữ lập trình được tạo ra vào năm 2006 bởi Graydon Hoare như một dự án phụ khi đang là developer tại Mozilla.  Rust pha trộn hiệu suất của các ngôn ngữ như C ++ với cú pháp thân thiện hơn, tập trung vào code an toàn và được thiết kế tốt giúp đơn giản hóa việc phát triển. Các phần của trình duyệt Firefox của Mozilla được viết bằng Rust và các nhà phát triển tại Microsoft được cho là sử dụng nó để mã hóa lại các phần của hệ điều hành Windows.
```

(Cũng tương tự như nhiều ví dụ trước đây, bạn nên thử lại ví dụ này trong các trường hợp xảy ra lỗi khác để hiểu rõ hơn về cách thức hoạt động của nó.) <br/>
Cấu trúc [OpenOptions](https://doc.rust-lang.org/std/fs/struct.OpenOptions.html) có thể được sử dụng để cấu hình cách một tập tin có thể được mở như thế nào. <br/>
