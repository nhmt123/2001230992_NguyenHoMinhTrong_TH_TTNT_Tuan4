# BÁO CÁO THỰC HÀNH: ỨNG DỤNG THUẬT TOÁN TÌM KIẾM TRONG TRÍ TUỆ NHÂN TẠO

### Thông tin chung

* **Môn học:** Trí Tuệ Nhân Tạo (AI)
* **Nội dung:** Bài toán Tô Màu Đồ Thị và Bài toán Người Bán Hàng (TSP).
* **Sinh viên thực hiện:** [Tên của bạn] - [MSSV]
* **Mã nguồn:** [Link đến file .ipynb hoặc .py trên GitHub]

---

## 1. Bài Toán Tô Màu Đồ Thị

Bài toán Tô màu Đồ thị là một ví dụ điển hình của **Bài toán Thỏa mãn Ràng buộc (CSP)**. Mục tiêu là gán màu cho các đỉnh sao cho **hai đỉnh kề nhau không được cùng màu**, đồng thời cố gắng sử dụng số lượng màu tối thiểu.

### 1.1. Cách Thức Hoạt Động (Tham Lam kết hợp Heuristic)

Chúng tôi sử dụng thuật toán **Tham Lam (Greedy Coloring)** áp dụng Heuristic **"Đỉnh có Bậc Lớn nhất"**.

1.  **Sắp xếp ưu tiên (Degree Heuristic):** Các đỉnh được sắp xếp theo thứ tự **bậc giảm dần**. Đỉnh có bậc cao nhất sẽ được tô màu trước để xử lý các ràng buộc nghiêm ngặt sớm nhất.
2.  **Quá trình Tô màu:** Duyệt qua các đỉnh đã sắp xếp. Mỗi đỉnh được gán màu khả dụng đầu tiên từ danh sách màu của nó.
3.  **Lan truyền Ràng buộc (Constraint Propagation):** Ngay sau khi một đỉnh được tô màu, màu đó sẽ bị loại bỏ khỏi danh sách các màu khả dụng của tất cả các đỉnh kề chưa được tô màu.

### 1.2. Phân Tích Hàm Quan Trọng trong Code

| Khối Lệnh/Biến | Chức Năng | Mô Tả Hoạt Động |
| :--- | :--- | :--- |
| `G`, `nodes` | **Dữ liệu đầu vào** | Ma trận kề của đồ thị (7 đỉnh A-G) và tên các đỉnh. |
| `degree` | Tính Bậc | Tính tổng các cạnh kề của mỗi đỉnh. |
| `colorDict` | Miền giá trị | Khởi tạo danh sách các màu khả dụng ban đầu (domain) cho mỗi đỉnh. |
| `sortedNode` | **Sắp xếp Ưu tiên** | Thực hiện sắp xếp các đỉnh theo bậc giảm dần. Đây là việc áp dụng **Heuristic** để tìm kiếm hiệu quả hơn. |
| `theSolution` | **Lõi Tô Màu** | Duyệt qua `sortedNode`, gán màu đầu tiên (`setTheColor[0]`) và sau đó loại bỏ màu này khỏi `colorDict` của các đỉnh kề. Đây là cơ chế **Lan truyền Ràng buộc**. |
| `G_nx`, `plt` | Trực quan hóa | Các lệnh NetworkX và Matplotlib để vẽ đồ thị và hiển thị kết quả tô màu. |

### 1.3. Kết Quả Chạy

Thuật toán đã tìm được một lời giải hợp lệ sử dụng 4 màu:
