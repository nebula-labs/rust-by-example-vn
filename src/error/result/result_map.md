# map for Result
Result map là một phương thức của kiểu Result trong Rust, cho phép bạn chuyển đổi giá trị thành công
của Result bằng cách áp dụng một hàm cho giá trị thành công đó. Nó trả về một Result mới có giá trị
thành công được biến đổi bằng hàm đã cho, hoặc giá trị lỗi của Result gốc nếu có lỗi xảy ra trong 
quá trình biến đổi

// Trong ví dụ này, chúng ta định nghĩa một hàm divide để chia một số cho một số khác. Nếu số bị chia
bằng 0 thì hàm trả về lỗi. Nếu không thì trả về kết quả

fn divide(x: f32, y: f32) -> Result<f32, &'static str> {
    if y == 0.0 {
        Err("Khong the chia voi so 0")
    }
    else {
        Ok(x/y)
    }
}
fn main() {
	//chia với số 0 kết quả trả về lỗi
    let r1 = divide(10.0, 0.0).map(|x|x*2.0);
    match r1 {
        Ok(vl) => println!("Ket qua phep chia {}", vl),
        Err(er) => println!("Loi phep chia {}", er),
    }
	//trả về kết quả là 6
    let r2 = divide(10.0, 2.0).map(|x|x*3.0);
    match r2 {
        Ok(vl) => println!("Ket qua phep chia {}", vl),
        Err(er) => println!("Loi phep chia {}", er),
    }
}
