# Closures 
//Cung cấp cho lập trình viên khả năng định nghĩa một hàm nặc danh, có thể được lưu trữ trong một biến và được
//truyền vào các hàm khác nhau như một tham số
//nó giúp cho code ngắn gọn, dễ đọc và dễ bảo trì hơn giảm thiểu lượng code lặp lại

fn main(){

    let outer_var = 42;
    
   //dạng thứ nhất định nghĩa như 1 hàm, khai báo kiểu dữ liệu biến và giá trị trả về  và sử dụng ||
    let closure_annotated = |i: i32| -> i32 { i + outer_var };
    //dạng thứ 2 không có {} và sử dụng ||
    let closure_inferred  = |i| i + outer_var  ;

    // các sử dụng của hai dạng đều giống như gọi hàm có tham số không có tham số
    println!("closure_annotated: {}", closure_annotated(5)); //ket qua la 47
    println!("closure_inferred: {}", closure_inferred(8));//ket qua la 50
    
    //trường hợp này không có tham số truyền vào
    let one = || 1;
    println!("closure returning one: {}", one());//ket qua la 1

    //chúng ta cũng có thể sử dụng các hàm có sẵn vào closures trong trường hợp này chúng ta sử dụng hàm sort_by
    //để sắp xếp các giá trị trong mảng
    let mut n = vec![3,5,1,80,23,10];
    n.sort_by(|a, b|b.cmp(a));

    println!("sap sep theo thu tu giam dan {:?}",n);
    //sap sep theo thu tu giam dan [80, 23, 10, 5, 3, 1]
    n.sort_by(|a, b|a.cmp(b));
    println!("sap sep theo thu tu tang dan {:?}",n);
    //sap sep theo thu tu tang dan [1, 3, 5, 10, 23, 80]
	
}
