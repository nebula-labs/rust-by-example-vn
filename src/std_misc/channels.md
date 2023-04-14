# Channels

Rust cung cấp các `channels` bất đồng bộ để giao tiếp giữa các threads.
Channels cho phép một luồng thông tin hai chiều giữa hai điểm đầu cuối: `Sender` và `Receiver`.

```rust,editable
use std::sync::mpsc::{Sender, Receiver};
use std::sync::mpsc;
use std::thread;

static NTHREADS: i32 = 3;

fn main() {
    // Channels có hai điểm đầu cuối: `Sender<T>` và `Receiver<T>`,
    // trong đó `T` là kiểu của message được trao đổi
    // (ghi chú kiểu là dư thừa)
    let (tx, rx): (Sender<i32>, Receiver<i32>) = mpsc::channel();
    let mut children = Vec::new();

    for id in 0..NTHREADS {
        // Sender có thể được sao chép
        let thread_tx = tx.clone();

        // Mỗi thread sẽ gửi id thông qua channel 
        let child = thread::spawn(move || {
            // Thread dành quyền kiểm soát `thread_tx`
            // Mỗi thread thêm một message vào trong channel 
            thread_tx.send(id).unwrap();

            // Gửi message là một hành động non-blocking, thread sẽ tiếp tục
            // ngay lập tức sau khi gửi message
            println!("thread {} finished", id);
        });

        children.push(child);
    }

    // Tại đây, tất cả các message được thu thập
    let mut ids = Vec::with_capacity(NTHREADS as usize);
    for _ in 0..NTHREADS {
        // Phương thức `recv` nhận từng message từ channel
        // `recv` will block the current thread if there are no messages available
        ids.push(rx.recv());
    }
    
    // Đợi cho các thread hoàn thất tất cả công việc còn lại
    for child in children {
        child.join().expect("oops! the child thread panicked");
    }

    // Cho thấy thứ tự gửi đi của các message
    println!("{:?}", ids);
}
```
