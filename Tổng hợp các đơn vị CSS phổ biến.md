# Tổng hợp các đơn vị CSS phổ biến

## Giới thiệu

Tài liệu này tổng hợp các đơn vị đo lường phổ biến trong CSS, giúp bạn hiểu rõ hơn về cách chúng hoạt động và khi nào nên sử dụng chúng.

## Các đơn vị CSS

### 1. Đơn vị tương đối

#### a. rem (root em)

* **Mô tả**:
    * Đơn vị tương đối dựa trên kích thước phông chữ của phần tử gốc (`<html>`).
    * Giúp tạo ra các thiết kế có thể mở rộng và dễ quản lý.
* **Ví dụ**:
    * Nếu `html { font-size: 16px; }`, thì `1rem` = 16px, `2rem` = 32px.

#### b. em

* **Mô tả**:
    * Đơn vị tương đối dựa trên kích thước phông chữ của phần tử cha gần nhất.
    * Có thể gây khó khăn trong việc quản lý nếu có nhiều phần tử lồng nhau.
* **Ví dụ**:
    * Nếu phần tử cha có `font-size: 20px;`, thì `1em` trong phần tử con = 20px.

#### c. % (phần trăm)

* **Mô tả**:
    * Đơn vị tương đối, dựa trên kích thước của phần tử cha.
    * Rất linh hoạt và được sử dụng rộng rãi trong thiết kế đáp ứng.

#### d. vw (Viewport Width) và vh (Viewport Height)

* **Mô tả**:
    * Đơn vị tương đối dựa trên chiều rộng và chiều cao của viewport.
    * 1vw bằng 1% chiều rộng của viewport, và 1vh bằng 1% chiều cao của viewport.

#### e. vmin và vmax

* **Mô tả**:
    * vmin tương đương 1% của kích thước nhỏ hơn giữa chiều rộng và chiều cao của viewport.
    * vmax tương đương 1% của kích thước lớn hơn giữa chiều rộng và chiều cao của viewport.

#### f. dvh (Dynamic Viewport Height)

* **Mô tả**:
    * Đơn vị này phản ánh chiều cao động của viewport, thay đổi khi các giao diện người dùng (như thanh địa chỉ) xuất hiện hoặc biến mất.

#### g. svh (Small Viewport Height)

* **Mô tả**:
    * Đại diện cho chiều cao nhỏ nhất của viewport, thường là khi các giao diện người dùng hiển thị đầy đủ.

#### h. lvh (Large Viewport Height)

* **Mô tả**:
    * Đại diện cho chiều cao lớn nhất của viewport, thường là khi các giao diện người dùng bị ẩn đi.

### 2. Đơn vị tuyệt đối

#### a. px (pixel)

* **Mô tả**:
    * Đơn vị tuyệt đối, đại diện cho một điểm ảnh trên màn hình.
    * Thường được sử dụng cho các kích thước cố định.

#### b. Các đơn vị tuyệt đối khác

* `pt` (point), `pc` (pica), `cm` (centimet), `mm` (milimet), `in` (inch).
* Ít được sử dụng trong thiết kế web, thường được sử dụng trong in ấn.

## Lưu ý

* Việc lựa chọn đơn vị phù hợp phụ thuộc vào yêu cầu cụ thể của thiết kế.
* `rem` thường được ưa chuộng hơn `em` trong các thiết kế phức tạp.
* Các đơn vị viewport (`vw`, `vh`, `vmin`, `vmax`, `dvh`, `svh`, `lvh`) rất hữu ích cho việc tạo các thiết kế đáp ứng.
* Các đơn vị tuyệt đối thường được sử dụng cho các trường hợp kích thước phần tử được cố định.
