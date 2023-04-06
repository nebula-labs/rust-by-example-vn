# Arc

Khi mà việc chia sẻ ownership giữa các luồng (thread) là cần thiết, ta có thể sử dụng
`Arc`(Atomically Reference Counted). Struct này thông qua việc implement trait `Clone`
có thể tạo ra con trỏ tham chiếu (reference pointer) đến vị trí của một giá trị nằm trên heap và đồng thời làm tăng
bộ đếm tham chiếu (reference counter). Bởi vì nó chia sẻ ownership giữa các thread, nên khi
reference pointer cuối cùng của một biến bị ra khỏi scope, thì biến đó bị giải phóng.

```rust,editable
use std::time::Duration;
use std::sync::Arc;
use std::thread;

fn main() {
    // Phần khai báo biến dưới đây là nơi mà giá trị của nó được chỉ định.
    let apple = Arc::new("the same apple");

    for _ in 0..10 {
        // Dưới đây không xảy ra sự gán giá trị vì đây là một con trỏ đến
        // một tham chiếu trên bộ nhớ heap.
        let apple = Arc::clone(&apple);

        thread::spawn(move || {
            // Vì sử dụng Arc, các thread con có thể được sinh ra sử dụng dữ liệu đã được cấp phát
            // ở tại vị trí con trỏ Arc trỏ đến.
            println!("{:?}", apple);
        });
    }

    // Đảm bảo rằng tất cả các instances của Arc được in ra ở các thread con.
    thread::sleep(Duration::from_secs(1));
}
```
