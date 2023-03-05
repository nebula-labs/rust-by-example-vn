# Phạm vi

Mặc định, các mục trong một module có phạm vi là riêng tư, nhưng điều này có thể được ghi đè bằng từ khóa `pub`. Chỉ những mục công khai của một module có thể được truy cập từ bên ngoài phạm vi của nó.

```rust,editable
// Một module tên là `my_mod`
mod my_mod {
    // Các mục trong module có phạm vi mặc định là riêng tư.
    fn private_function() {
        println!("called `my_mod::private_function()`");
    }

    // Sử dụng từ khóa `pub` để ghi đè phạm vi mặc định.
    pub fn function() {
        println!("called `my_mod::function()`");
    }

    // Các mục có thể truy cập lẫn nhau trong cùng một module,
    // thậm chí phạm vi của nó là riêng tư.
    pub fn indirect_access() {
        print!("called `my_mod::indirect_access()`, that\n> ");
        private_function();
    }

    // Các module có thể lồng nhau.
    pub mod nested {
        pub fn function() {
            println!("called `my_mod::nested::function()`");
        }

        #[allow(dead_code)]
        fn private_function() {
            println!("called `my_mod::nested::private_function()`");
        }

        // Các hàm được khai báo bằng cách sử dụng cú pháp `pub(in path)` chỉ hiện diện
        // trong đường dẫn được chỉ định. `path` phải là một module cha hoặc tổ tiên (ancestor).
        pub(in crate::my_mod) fn public_function_in_my_mod() {
            print!("called `my_mod::nested::public_function_in_my_mod()`, that\n> ");
            public_function_in_nested();
        }

        // Các hàm được khai báo bằng cách sử dụng cú pháp `pub(self)` chỉ hiện diện
        // trong module hiện tại, tương tự như khi ta để phạm vi của chúng là riêng tư.
        pub(self) fn public_function_in_nested() {
            println!("called `my_mod::nested::public_function_in_nested()`");
        }

        // Các hàm được khai báo bằng cách sử dụng cú pháp `pub(super)` chỉ hiện diện
        // trong module cha.
        pub(super) fn public_function_in_super_mod() {
            println!("called `my_mod::nested::public_function_in_super_mod()`");
        }
    }

    pub fn call_public_function_in_my_mod() {
        print!("called `my_mod::call_public_function_in_my_mod()`, that\n> ");
        nested::public_function_in_my_mod();
        print!("> ");
        nested::public_function_in_super_mod();
    }

    // pub(crate) khiến cho các hàm có thể hiện diện chỉ trong crate hiện tại.
    pub(crate) fn public_function_in_crate() {
        println!("called `my_mod::public_function_in_crate()`");
    }

    // Các module lồng nhau sẽ theo cùng một quy tắc về phạm vi.
    mod private_nested {
        #[allow(dead_code)]
        pub fn function() {
            println!("called `my_mod::private_nested::function()`");
        }

        // Các mục cha có phạm vi riêng tư sẽ giới hạn phạm vi của một mục con,
        // ngay cả khi nó được khai báo là công khai trong một phạm vi lớn hơn.
        #[allow(dead_code)]
        pub(crate) fn restricted_function() {
            println!("called `my_mod::private_nested::restricted_function()`");
        }
    }
}

fn function() {
    println!("called `function()`");
}

fn main() {
    // Các module cho phép phân biệt giữa các mục có cùng tên.
    function();
    my_mod::function();

    // Các mục công khai, bao gồm các mục trong các module lồng nhau, có thể được
    // truy cập từ bên ngoài module cha.

    my_mod::indirect_access();
    my_mod::nested::function();
    my_mod::call_public_function_in_my_mod();

    // Các mục khai báo pub(crate) có thể được truy cập từ bất cứ đầu trong cùng một crate.
    my_mod::public_function_in_crate();

    // Các mục khai báo pub(in path) chỉ có thể được truy cập trong module được chỉ định.
    // Lỗi! hàm `public_function_in_my_mod` là riêng tư.
    //my_mod::nested::public_function_in_my_mod();
    // TODO ^ Hãy thử bỏ ghi chú trên dòng này

    // Các mục riêng tư của module không thể được truy cập trực tiếp, ngay cả khi
    // nó được lồng bên trong một module công khai:

    // Lỗi! `private_function` là riêng tư
    //my_mod::private_function();
    // TODO ^ Hãy thử bỏ ghi chú trên dòng này

    // Lỗi! `private_function` là riêng tư
    //my_mod::nested::private_function();
    // TODO ^ Hãy thử bỏ ghi chú trên dòng này

    // Error! `private_nested` là một module riêng tư
    //my_mod::private_nested::function();
    // TODO ^ Hãy thử bỏ ghi chú trên dòng này

    // Error! `private_nested` là một module riêng tư
    //my_mod::private_nested::restricted_function();
    // TODO ^ Hãy thử bỏ ghi chú trên dòng này
}
```
