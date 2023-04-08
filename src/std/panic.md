# `panic!`

`panic!` có thể được sử dụng để gây ra sự kết thúc của trương trình và bắt đầu quá trình giải phóng stack. Trong quá trình giải phóng, bộ thực thi sẽ giải phóng tất cả tài nguyên đang được sử dụng trong luồng bằng cách gọi hàm hủy tất cả các đối tượng của nó.

Vì chúng ta đang xử lý các chương trình chỉ có một luồng, `panic!` sẽ khiến chương trình in ra một thông báo kết thúc chương trình và dừng hoạt động.

```rust,editable,ignore,mdbook-runnable
// Thực hiện lại phép chia số nguyên (/)
fn division(dividend: i32, divisor: i32) -> i32 {
    if divisor == 0 {
        // Phép chia cho 0 gây ra sự kết thúc cho chương trình
        panic!("division by zero");
    } else {
        dividend / divisor
    }
}

// Hàm main
fn main() {
    // Cấp phát một biến số nguyên ở Heap
    let _x = Box::new(0i32);

    // Thao tác này sẽ gây ra lỗi tác vụ
    division(3, 0);

    println!("This point won't be reached!");

    // `_x` sẽ bị hủy tại đây
}
```

Kiểm tra rằng `panic!` không gây rò rỉ bộ nhớ.


```shell
$ rustc panic.rs && valgrind ./panic
==4401== Memcheck, a memory error detector
==4401== Copyright (C) 2002-2013, and GNU GPL'd, by Julian Seward et al.
==4401== Using Valgrind-3.10.0.SVN and LibVEX; rerun with -h for copyright info
==4401== Command: ./panic
==4401== 
thread '<main>' panicked at 'division by zero', panic.rs:5
==4401== 
==4401== HEAP SUMMARY:
==4401==     in use at exit: 0 bytes in 0 blocks
==4401==   total heap usage: 18 allocs, 18 frees, 1,648 bytes allocated
==4401== 
==4401== All heap blocks were freed -- no leaks are possible
==4401== 
==4401== For counts of detected and suppressed errors, rerun with: -v
==4401== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```