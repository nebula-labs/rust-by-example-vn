# map for Result
Trong ví dụ trước đó, việc panic trong hàm multiply sẽ không tạo ra code đáng tin cậy. Thông thường, chúng ta muốn trả về lỗi cho người gọi để họ có thể quyết định cách phản hồi đúng với các lỗi.

Trước tiên, chúng ta cần biết loại lỗi mà chúng ta đang xử lý. Để xác định loại Err, chúng ta nhìn vào hàm parse(), được triển khai với trait FromStr cho kiểu i32. Do đó, loại Err được chỉ định là ParseIntError.

Trong ví dụ dưới đây, câu lệnh match trực tiếp dẫn đến mã nguồn tổng thể phức tạp hơn.

use std::num::ParseIntError;

// Với kiểu trả về được viết lại, chúng ta sử dụng khớp mẫu mà không cần unwrap().
fn multiply(first_number_str: &str, second_number_str: &str) -> Result<i32, ParseIntError> {
    match first_number_str.parse::<i32>() {
        Ok(first_number)  => {
            match second_number_str.parse::<i32>() {
                Ok(second_number)  => {
                    Ok(first_number * second_number)
                },
                Err(e) => Err(e),
            }
        },
        Err(e) => Err(e),
    }
}

fn print(result: Result<i32, ParseIntError>) {
    match result {
        Ok(n)  => println!("n is {}", n),
        Err(e) => println!("Error: {}", e),
    }
}

fn main() {
    // Kết quả trả về vẫn là một câu trả lời hợp lý.
    let twenty = multiply("10", "2");
    print(twenty);

    // Sau đó, chương trình sẽ trả về thông báo lỗi hữu ích hơn.
    let tt = multiply("t", "2");
    print(tt);
}

May mắn thay, các hàm map, and_then và nhiều hàm kết hợp khác cũng được triển khai cho Result. Result chứa một danh sách đầy đủ các hàm này.

use std::num::ParseIntError;

//Giống với Option, chúng ta có thể sử dụng các hàm kết hợp như map().
//Hàm này hoàn toàn giống với hàm trên và đọc như sau:
//Nhân hai giá trị nếu cả hai giá trị đều có thể được phân tích từ chuỗi, nếu không, truyền lỗi cho người gọi.
fn multiply(first_number_str: &str, second_number_str: &str) -> Result<i32, ParseIntError> {
    first_number_str.parse::<i32>().and_then(|first_number| {
        second_number_str.parse::<i32>().map(|second_number| first_number * second_number)
    })
}

fn print(result: Result<i32, ParseIntError>) {
    match result {
        Ok(n)  => println!("n is {}", n),
        Err(e) => println!("Error: {}", e),
    }
}

fn main() {
    // Kết quả trả về vẫn là một câu trả lời hợp lý.
    let twenty = multiply("10", "2");
    print(twenty);

 // Sau đó, chương trình sẽ trả về thông báo lỗi hữu ích hơn.
    let tt = multiply("t", "2");
    print(tt);
}
