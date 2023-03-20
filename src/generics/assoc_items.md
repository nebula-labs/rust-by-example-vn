# Associated items

"Associated Items" là một tập hợp các quy tắc liên quan đến các [`item`][items] của các loại khác nhau.
Nó là một phần mở rộng của `trait` generics, nó cho phép các `trait` có thể định nghĩa các `item` mới bên trong.


Một item như vậy được gọi là *associated type*, nó cung cấp các mẫu sử dụng đơn giản hơn khi `trait`
được khai báo với tính chất generic đối với kiểu container của nó.


### Xem thêm:

[RFC][RFC]

[items]: https://doc.rust-lang.org/reference/items.html
[RFC]: https://github.com/rust-lang/rfcs/blob/master/text/0195-associated-items.md