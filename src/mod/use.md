# Khai báo `use`

Khai báo `use` có thể được sử dụng để ánh xạ một đường dẫn đầy đủ với một cái tên mới, để truy cập dễ dàng hơn. Nó thường được sử dụng như sau:

```rust,editable,ignore
use crate::deeply::nested::{
    my_first_function,
    my_second_function,
    AndATraitType
};

fn main() {
    my_first_function();
}
```

Bạn có thể sử dụng từ khóa `as` để ánh xạ những thứ được dẫn xuất vào module hiện tại với một cái tên khác:

```rust,editable
// Ánh xạ đường dẫn `deeply::nested::function` đến `other_function`.
use deeply::nested::function as other_function;

fn function() {
    println!("called `function()`");
}

mod deeply {
    pub mod nested {
        pub fn function() {
            println!("called `deeply::nested::function()`");
        }
    }
}

fn main() {
    // Truy cập `deeply::nested::function` một cách dễ dàng.
    other_function();

    println!("Entering block");
    {
        // Cách này tương đương với `use deeply::nested::function as function`.
        // `function()` này sẽ che đi cái bên ngoài.
        use crate::deeply::nested::function;

        // các khai báo `use` có phạm vi cục bộ. Trong trường hợp này,
        // việc che đạy của `function()` chỉ áp dụng trong khối này.
        function();

        println!("Leaving block");
    }

    function();
}
```
