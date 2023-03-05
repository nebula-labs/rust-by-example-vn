# `super` và `self`

Các từ khóa `super` và `self` có thể được sử dụng trong đường dẫn để tránh sự nhầm lẫn khi truy cập các mục và cũng để tránh việc phải cố định các đường dẫn không cần thiết.

```rust,editable
fn function() {
    println!("called `function()`");
}

mod cool {
    pub fn function() {
        println!("called `cool::function()`");
    }
}

mod my {
    fn function() {
        println!("called `my::function()`");
    }

    mod cool {
        pub fn function() {
            println!("called `my::cool::function()`");
        }
    }

    pub fn indirect_call() {
        // Hãy gọi tất cả các hàm có tên `function` từ phạm vi này!
        print!("called `my::indirect_call()`, that\n> ");

        // Từ khóa `self` chỉ đến phạm vi của module hiện tại - trong trường hợp này là `my`.
        // Khi sử dụng `self::function()` và gọi `function()` trực tiếp đều cho kết quả giống nhau,
        // vì chúng đều tham chiếu đến cùng một hàm.
        self::function();
        function();

        // Chúng ta cũng có thể sử dụng `self` để truy cập một module khác bên trong `my`:
        self::cool::function();

        // Từ khóa `super` chỉ đến phạm vi cha mà chứa `my` (nằm ngoài module `my`).
        super::function();

        // Cách này sẽ ánh xạ `cool::function` trong phạm vi của *crate*.
        // Trong trường hợp này, phạm vi của crate là phạm vi ngoài cùng
        {
            use crate::cool::function as root_function;
            root_function();
        }
    }
}

fn main() {
    my::indirect_call();
}
```
