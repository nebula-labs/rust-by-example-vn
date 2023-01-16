# Trả về giá trị từ loop

Một trong những cách sử dụng của `loop` đó là thử đi thử lại một thao tác 
cho đến khi nó thành công. Tuy nhiên, nếu thao tác đó trả về một giá trị, bạn có thể 
cần phải truyền nó cho phần còn lại của mã code: đặt giá trị sau lệnh `break` và nó 
sẽ được trả về bởi biểu thức `loop`.

```rust,editable
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    assert_eq!(result, 20);
}
```
