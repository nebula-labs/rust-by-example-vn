# Methods
Phương thức được chú thích tương tự như hàm:
struct Owner(i32);

impl Owner {
    // Chú thích về lifetime được thêm vào tương tự như trong một hàm độc lập.
    fn add_one<'a>(&'a mut self) { self.0 += 1; }
    fn print<'a>(&'a self) {
        println!("`print`: {}", self.0);
    }
}

fn main() {
    let mut owner = Owner(18);

    owner.add_one();
    owner.print();
}
