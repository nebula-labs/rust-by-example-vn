# Ownership và moves

Bởi vì các biến tự chịu trách nhiệm cho việc giải phóng tài nguyên[^†] của chúng nên
**một tài nguyên chỉ có thể có một owner**. Điều này giúp ngăn chặn việc tài nguyên
bị giải phóng nhiều lần. Hãy lưu ý rằng, không phải tất cả các biến đều sở hữu tài nguyên (ví dụ: [references])

Khi thực hiện các phép gán (`let x = y`) hoặc truyền đối số vào hàm bằng giá trị (`foo(x)`),
_ownership_ của tài nguyên sẽ được chuyển cho đối tượng khác. Trong Rust, ta gọi nó là _move_.

Sau khi move tài nguyên, owner trước đó không thể sử dụng tài nguyên đó nữa. Điều này giúp
tránh hiện tượng tạo ra các con trỏ treo (dangling pointers).

```rust,editable
// Hàm này lấy đi ownership của tài nguyên được cấp phát trên heap
fn destroy_box(c: Box<i32>) {
    println!("Destroying a box that contains {}", c);

    // `c` bị xoá và giải phóng khỏi bộ nhớ
}

fn main() {
    // Một giá trị integer được cấp phát trên _stack_
    let x = 5u32;

    // *Copy* `x` vào `y` - không có tài nguyên nào bị moved
    let y = x;

    // Cả hai giá trị đều có thể được sử dụng độc lập
    println!("x is {}, and y is {}", x, y);

    // `a` là một con trỏ đến một giá trị integer được cấp phát trên _heap_
    let a = Box::new(5i32);

    println!("a contains: {}", a);

    // *Move* `a` đến `b`
    let b = a;
    // Địa chỉ của con trỏ `a` (chứ không phải dữ liệu) được copy đến `b`
    // Cả hai bây giờ đều trỏ đến cùng một dữ liệu được cấp phát trên heap,
    // nhưng `b` giờ đây sở hữu dữ liệu đó.

    // Lỗi xảy ra! `a` không thể nào truy cập dữ liệu được nữa vì nó không còn
    // sỡ hữu dữ liệu được cấp phát trên heap đó nữa.
    //println!("a contains: {}", a);
    // TODO ^ Hãy thử bỏ chú thích dòng này

    // Hàm này lấy đi ownership của tài nguyên được cấp phát trên heap khỏi `b`
    destroy_box(b);

    // Tài nguyên trên heap đã được giải phóng tại thời điểm này, hành động sau sẽ dẫn đến việc
    // giải tham chiếu (dereference) một tài nguyên đã được giải phóng khỏi bộ nhớ, nhưng điều này bị cấm bởi compiler.
    // Lỗi xảy ra! Nguyên nhân giống như lỗi trước đó.
    //println!("b contains: {}", b);
    // TODO ^ Hãy thử bỏ chú thích dòng này
}
```

[references]: ../flow_control/match/destructuring/destructure_pointers.md

[^†] Lời người dịch:
Tài nguyên (resource) được đề cập trong ngữ cảnh của Rust có thể là các giá trị như một đối tượng heap-allocated (có nghĩa là giá trị được cấp phát động và quản lý bởi hệ điều hành), file handle, socket, hoặc bất cứ thứ gì khác mà ứng dụng của bạn cần phải cấp phát, sử dụng và giải phóng khi không cần thiết nữa để giải phóng tài nguyên và tránh lãng phí bộ nhớ hoặc tài nguyên hệ thống.
