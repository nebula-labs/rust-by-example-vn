# RAII

Biến trong Rust sẽ có vai trò nhiều hơn việc chỉ lưu tài nguyên trên stack: chúng còn _sở hữu_ tài nguyên đó.
ví dụ như `Box<T>` sở hữu tài nguyên trên heap. Rust tuân thủ [RAII][raii]
(Resource Acquisition Is Initialization) do đó khi một đối tượng ra khỏi scope của nó,
destructor của nó sẽ được gọi và tài nguyên của nó sở hữu được giải phóng.

Hành vi này giúp đảm bảo chống lại các lỗi _rò rỉ tài nguyên (resource leak)_, do đó bạn không cần phải tự giải phóng
tài nguyên một cách thủ công hoặc lo lắng về rò rỉ bộ nhớ nữa! Dưới đây là một ví dụ minh họa ngắn:

```rust,editable
// raii.rs
fn create_box() {
    // Cấp phát một integer trên heap
    let _box1 = Box::new(3i32);

    // `_box1` được xoá ở đây và giải phóng khỏi bộ nhớ
}

fn main() {
    // Cấp phát một integer trên heap
    let _box2 = Box::new(5i32);

    // Một scope lồng nhau:
    {
        // Cấp phát một integer trên heap
        let _box3 = Box::new(4i32);

        // `_box3` được xoá ở đây và giải phóng khỏi bộ nhớ
    }

    // Tạo thêm nhiều biến box nữa
    // Không cần phải tự giải phóng bộ nhớ một cách thủ công!
    for _ in 0u32..1_000 {
        create_box();
    }

    // `_box2` được xoá ở đây và giải phóng khỏi bộ nhớ
}
```

Tất nhiên là ta có thể kiểm tra lại các lỗi về bộ nhớ bằng cách sử dụng [`valgrind`][valgrind]:

```shell
$ rustc raii.rs && valgrind ./raii
==26873== Memcheck, a memory error detector
==26873== Copyright (C) 2002-2013, and GNU GPL'd, by Julian Seward et al.
==26873== Using Valgrind-3.9.0 and LibVEX; rerun with -h for copyright info
==26873== Command: ./raii
==26873==
==26873==
==26873== HEAP SUMMARY:
==26873==     in use at exit: 0 bytes in 0 blocks
==26873==   total heap usage: 1,013 allocs, 1,013 frees, 8,696 bytes allocated
==26873==
==26873== All heap blocks were freed -- no leaks are possible
==26873==
==26873== For counts of detected and suppressed errors, rerun with: -v
==26873== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 2 from 2)
```

Không có lỗi rò rỉ tài nguyên nào!

## Destructor

Khái niệm về destructor trong Rust được cung cấp thông qua trait [`Drop`].
Destructor được gọi khi tài nguyên ra khỏi phạm vi. Trait này không nhất thiết phải
được implement cho tất cả các kiểu mà chỉ cần implement cho kiểu của bạn khi bạn cần
thực hiện một logic riêng với destructor của kiểu đó.

Hãy chạy ví dụ dưới đây để xem cách mà trait [`Drop`] hoạt động. Bất cứ khi nào biến trong hàm
`main` ra khỏi scope của hàm thì destructor mà đã tuỳ chỉnh sẽ được gọi.

```rust,editable
struct ToDrop;

impl Drop for ToDrop {
    fn drop(&mut self) {
        println!("ToDrop is being dropped");
    }
}

fn main() {
    let x = ToDrop;
    println!("Made a ToDrop!");
}
```

### Đọc thêm:

[Box][box]

[raii]: https://en.wikipedia.org/wiki/Resource_Acquisition_Is_Initialization
[box]: ../std/box.md
[valgrind]: http://valgrind.org/info/
[`drop`]: https://doc.rust-lang.org/std/ops/trait.Drop.html
