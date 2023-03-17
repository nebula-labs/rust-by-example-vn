# Diverging functions
// Trong Rust, diverging funtion thường được sử dụng trong trường hợp xử lý lỗi. 
// Khi một lỗi không thể được xử lý và chương trình không thể tiếp tục thực hiện, 
//thì các diverging function được sử dụng để kích hoạt lỗi và dừng chương trình

// Hàm foo() không có giá trị trả về nhưng thay vào đó, nó kích hoạt một sử dụng panic!() để dừng chương trình. 
fn foo() -> ! {
    panic!("Day la ham khong tra ve gia tri");
}

// Hàm some_fn() không thể được khởi tạo, bởi vì tập hợp tất cả các giá trị có thể có mà loại này có thể có trống
fn some_fn() {
    ()
}
// Hàm foo_loop() sử dụng 1 vòng lập vô hạn để chặn một luồng chạy. Vì không bao giờ có giá trị hợp lệ được trả về, hàm được đánh dấu là ->!
fn foo_loop() -> ! {
    loop {}
}
//hàm tính tổng các số lẻ từ 1 đến số nhập vào trong trường hợp này là từ 1 đến 9
fn sum_odd_numbers(up_to: u32) -> u32 {
    let mut acc = 0;
    for i in 1..up_to {
   
        let addition: u32 = match i%2 == 1 {
           
            true => i,
      
            false => continue,
        };
        acc += addition;
    }
    acc
}
fn main() {
    //goi ham some_fn() tra ve ()
   println!("Ham some_fn {:?}",some_fn());
    //goi ham tinh tong ca so le tu 1 den 10 ket qua la 25
    println!("Tong cac so tu 1 den 9 (excluding): {}", sum_odd_numbers(10));
    //goi ham foo() in ra panic
    foo();
}
