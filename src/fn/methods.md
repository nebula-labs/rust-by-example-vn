# Associated functions & Methods

Một số function được kết nối với một loại (type) cụ thể. Có hai dạng:
các hàm liên quan và phương thức. Hàm liên quan là hàm thường được định
nghĩa trên một loại, trong khi các phương thức là các hàm liên quan được
được gọi trên một thể hiện (instance) cụ thể của một loại.

```rust,editable
struct Point {
    x: f64,
    y: f64,
}

// Triển khai tất cả các fucntion & method liên quan đến `Point`
impl Point {
    // Đây là một "associated function" bởi vì function này được kết nối với
    // một loại cụ thể là Point
    //
    // Associated functions không cần phải được gọi với một instance.
    // Các function này có thể được sử dụng dạng như một constructor.
    fn origin() -> Point {
        Point { x: 0.0, y: 0.0 }
    }

    // Một associated function khác, nhận vào hai đối số:
    fn new(x: f64, y: f64) -> Point {
        Point { x: x, y: y }
    }
}

struct Rectangle {
    p1: Point,
    p2: Point,
}

impl Rectangle {
    // Đây là một method
    // `&self` trỏ tới `self: &Self`, trong đó `Self` là loại 
    // của đối tượng gọi method. Trong trường hợp này `Self` = `Rectangle`
    fn area(&self) -> f64 {
        // `self` cung cấp truy cập vào các fields của struct thông qua toán tử "."
        let Point { x: x1, y: y1 } = self.p1;
        let Point { x: x2, y: y2 } = self.p2;

        // `abs` là một phương thức `f64` trả về giá trị tuyệt đối của đối tượng gọi
        ((x1 - x2) * (y1 - y2)).abs()
    }

    fn perimeter(&self) -> f64 {
        let Point { x: x1, y: y1 } = self.p1;
        let Point { x: x2, y: y2 } = self.p2;

        2.0 * ((x1 - x2).abs() + (y1 - y2).abs())
    }

    // Phương thức này yêu cầu đối tượng gọi tới phải là mutable
    // `&mut self` trỏ tới `self: &mut Self`
    fn translate(&mut self, x: f64, y: f64) {
        self.p1.x += x;
        self.p2.x += x;

        self.p1.y += y;
        self.p2.y += y;
    }
}

// `Pair` sở hữu các tài nguyên: hai số int trên vùng nhớ heap 
struct Pair(Box<i32>, Box<i32>);

impl Pair {
    // Phương thức này "sử dụng" các tài nguyên của đối tượng gọi tới
    // `self` trỏ tới `self: Self`
    fn destroy(self) {
        // Destructure `self`
        let Pair(first, second) = self;

        println!("Destroying Pair({}, {})", first, second);

        // `first` và `second` ra khỏi scope và được giải phóng
    }
}

fn main() {
    let rectangle = Rectangle {
        // Associated functions được gọi bằng cách sử dụng ":"
        p1: Point::origin(),
        p2: Point::new(3.0, 4.0),
    };

    // Method được gọi bằng cách sử dụng toán tử "."
    // Lưu ý rằng biến số `&self` được ngầm định truyền vào
    // `rectangle.perimeter()` === `Rectangle::perimeter(&rectangle)`
    println!("Rectangle perimeter: {}", rectangle.perimeter());
    println!("Rectangle area: {}", rectangle.area());

    let mut square = Rectangle {
        p1: Point::origin(),
        p2: Point::new(1.0, 1.0),
    };

    // Error! `rectangle` đang là immutable, nhưng phương thức này yêu cầu một đối tượng mutable
    // rectangle.translate(1.0, 0.0);
    // TODO ^ Bỏ comment dòng này để kiểm tra

    // Okay! Đối tượng mutable có thể gọi tới phương thức mutable
    square.translate(1.0, 1.0);

    let pair = Pair(Box::new(1), Box::new(2));

    pair.destroy();

    // Error! Previous `destroy` call "consumed" `pair`
    //pair.destroy();
    // TODO ^ Bỏ comment dòng này để kiểm tra
}
```