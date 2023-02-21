# Custom

Một số điều kiện `target_os` được cung cấp hoàn toàn bởi `rustc`, nhưng một số điều kiện tùy phải chuyển tới `rustc` bằng cách sử dụng cờ `--cfg`.

```rust,editable,ignore,mdbook-runnable
#[cfg(some_condition)]
fn conditional_function() {
    println!("condition met!");
}

fn main() {
    conditional_function();
}
```

Thử chạy đoạn code trên xem điều gì xảy ra khi không sử dụng cờ tùy chỉnh `cfg`.

Với cờ tùy chỉnh `cfg`:

```shell
$ rustc --cfg some_condition custom.rs && ./custom
condition met!
```