# Closures 
Closures là các hàm có thể nắm giữ môi trường bao quanh. Ví dụ, closure nắm giữ biến x:

|val| val + x

Cú pháp và tính năng của closures khiến chúng rất thuận tiện cho việc sử dụng trực tiếp. Gọi một closure giống như gọi một hàm. Tuy nhiên, cả kiểu đầu vào và trả về đều có thể được suy ra và tên biến đầu vào phải được chỉ định.

Các đặc điểm khác của closures bao gồm:

	*sử dụng || thay vì () để bao quanh các biến đầu vào.
	*phân cách cú pháp thân tùy chọn ({}) cho một biểu thức đơn (bắt buộc nếu không sử dụng).
	*khả năng bắt giữ các biến môi trường bên ngoài.

fn main() {
	let outer_var = 42;

	// Một hàm thông thường không thể truy cập được các biến trong môi trường bao quanh
	// fn function(i: i32) -> i32 { i + outer_var }
	// TODO: bỏ chú thích ở dòng trên và xem lỗi của trình biên dịch. Trình biên dịch
	// đề xuất chúng ta nên định nghĩa một closure thay thế.

	// Closures là vô danh, ở đây chúng tôi đang liên kết chúng với các tham chiếu
	// Chú thích giống như chú thích hàm, nhưng không bắt buộc
	// như là các `{}` bao quanh thân hàm. Những hàm vô danh này
	// được gán cho các biến có tên phù hợp.
	let closure_annotated = |i: i32| -> i32 { i + outer_var };
	let closure_inferred  = |i     |          i + outer_var  ;

	// Gọi các closures.
	println!("closure_annotated: {}", closure_annotated(1));
	println!("closure_inferred: {}", closure_inferred(1));
	// Khi loại của closure đã được suy ra, nó không thể được suy ra lại với loại khác.
	// println!("cannot reuse closure_inferred with another type: {}", closure_inferred(42i64));
	// TODO: bỏ chú thích ở dòng trên và xem lỗi của trình biên dịch.

	// Một closure không có tham số và trả về một `i32`.
	// Loại trả về được suy ra.
	let one = || 1;
	println!("closure returning one: {}", one());
}
