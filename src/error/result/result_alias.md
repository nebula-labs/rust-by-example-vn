# aliases for Result
Tương tự như Option. Một kết quả trả về (Result) của một function thường sẽ có hai trường hợp:

 Nếu thành công thì trả về kết quả
 Nếu lỗi (Err) và trả về thông tin lỗi.
Result mô tả lỗi gì đang xảy

sau đây là ví dụ
//tạo enum mô tả 3 vị trí cúa nhân viên CEO, CTO thì có quyền truy cao nhất
enum Possition {
    CEO,
    CTO,
    IT,
}
//Trạng thái của nhân viên đó khi truy cập vào hệ thống
enum Status {
    Allow,
    Deny,
}
// mô tả thông tin của nhân viên để có thể vào hệ thống
struct Employee {
    possition: Possition,
    status: Status,
}
//hàm này định nghĩa các quyền truy cập, nếu status là deny thì chặn không cho vô hệ thống
//nếu là nhân viên IT thì không được vào hệ thống, còn các vị trí còn lại thì được vô
fn try_access(employee: &Employee) -> Result<(), String> {
    match employee.status {
        Status::Deny => return Err("Access deny".to_string()),
        _ => (),
    }
    match employee.possition {
        Possition::CEO => Ok(()),
        Possition::CTO => Ok(()),
        Possition::IT => Err("Invalid possition".to_string()),
    }
}
//hàm này in thông tin nhân viên được truy cập thành công hay thất bại
fn print_access(employee: Employee) -> Result<(), String> {
    if try_access(&employee).is_ok(){
        println!("Success access");
    }
    else {
        println!("Deny access");
    }
    Ok(())
}
//nhân viên 1 được truy cập vào
//nhân viên 2 không được truy cập vào
fn main() {
    let employee1 = Employee{possition: Possition::CEO, status: Status::Allow};
    let employee2 = Employee {possition: Possition::IT, status: Status::Deny};

    print_access(employee1);
    print_access(employee2);
}
