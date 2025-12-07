# BÁO CÁO THỰC HÀNH TRÍ TUỆ NHÂN TẠO

### Thông tin chung
* **Nội dung:** Bài toán Tô Màu Đồ Thị và Bài toán Người Bán Hàng (TSP).
* **Sinh viên thực hiện:** Nguyễn Hồ Minh Trọng - 2001230992
* **Mã nguồn colab:** https://colab.research.google.com/drive/1KUaKZOrgzlh_Jhh8xtasJ3T_9HHIsuCf?usp=sharing

---

## 1. Bài Toán Tô Màu Đồ Thị

Bài toán Tô màu Đồ thị là một ví dụ điển hình của **Bài toán Thỏa mãn Ràng buộc (CSP)**. Mục tiêu là gán màu cho các đỉnh sao cho **hai đỉnh kề nhau không được cùng màu**, đồng thời cố gắng sử dụng số lượng màu tối thiểu.

### 1.1. Cách Thức Hoạt Động (Tham Lam kết hợp Heuristic)

Sử dụng thuật toán **Tham Lam (Greedy Coloring)** áp dụng Heuristic **"Đỉnh có Bậc Lớn nhất"**.

1.  **Sắp xếp ưu tiên (Degree Heuristic):** Các đỉnh được sắp xếp theo thứ tự **bậc giảm dần**. Đỉnh có bậc cao nhất sẽ được tô màu trước để xử lý các ràng buộc.
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
* Đỉnh A = Red
* Đỉnh B = Blue
* Đỉnh C = Yellow
* Đỉnh D = Green
* Đỉnh E = Blue
* Đỉnh F = Red
* Đỉnh G = Yellow

Tổng số màu đã sử dụng: 4

## 2. Bài Toán Người Bán Hàng (Traveling Salesperson Problem - TSP)

Bài toán TSP thuộc lớp **Tối ưu hóa Tổ hợp**. Mục tiêu là tìm chu trình ngắn nhất đi qua tất cả các thành phố. So sánh hai thuật toán: Tham Lam (Heuristic) và Nhánh và Cận (Tối ưu).

### 2.1. Phân Tích Hàm Quan Trọng

#### A. Thuật Toán Tham Lam (`tsp_greedy`)

* **Mục tiêu:** Tìm kiếm **Tối ưu Cục bộ** bằng cách luôn chọn cạnh rẻ nhất tiếp theo.
* **Hàm quan trọng:** Vòng lặp tìm `min_cost` và `next_node`. Nó chỉ xét chi phí của cạnh hiện tại mà không quan tâm đến chi phí để hoàn thành chu trình.

#### B. Thuật Toán Nhánh và Cận (`tsp_branch_and_bound`, `solve_tsp_bnb`)

* **Mục tiêu:** Tìm kiếm **Tối ưu Toàn cục** bằng cách loại bỏ các nhánh không tiềm năng.
* **Hàm `solve_tsp_bnb`:** Khởi tạo. Nó chạy `tsp_greedy` để lấy chi phí của đường đi Tham Lam làm **Cận Trên (Upper Bound)** ban đầu (`min_cost_bnb`).
* **Khối tính `lower_bound`:** **Hàm Heuristic:** Ước tính chi phí tối thiểu còn lại của chu trình.
* **Khối cắt nhánh (`Pruning`):** `if current_cost + lower_bound >= min_cost_bnb`: Đây là lõi của thuật toán. Nếu chi phí hiện tại cộng với chi phí ước tính tối thiểu lớn hơn hoặc bằng chi phí tốt nhất đã biết, nhánh tìm kiếm đó sẽ bị dừng (cắt nhánh) vì không thể tìm được kết quả tốt hơn.

### 2.2. Kết Quả Chạy và Phân Tích So Sánh

Sử dụng một ma trận chi phí đã được điều chỉnh để tạo ra sự khác biệt về kết quả:

| Phương Pháp | Chu Trình | Tổng Chi Phí | Đánh giá |
| :--- | :--- | :--- | :--- |
| **Tham Lam** | A $\to$ C $\to$ D $\to$ B $\to$ E $\to$ A | **16** | **Tối ưu Cục bộ.** |
| **Nhánh & Cận** | A $\to$ B $\to$ E $\to$ C $\to$ D $\to$ A | **14** | **Tối ưu Toàn cục.** |

### 2.3. Kết Luận Về Sự Khác Biệt

Sự chênh lệch chi phí từ 16 xuống 14 cho thấy tính vượt trội của Branch and Bound:

1.  **Tham Lam (Chi phí 16):** Đã bị "bẫy" bởi lựa chọn rẻ nhất tức thời ($A \to C$, chi phí 1). Lựa chọn này dẫn đến chi phí tổng cao hơn.
2.  **Nhánh và Cận (Chi phí 14):** B&B đã chấp nhận khám phá nhánh có chi phí ban đầu cao hơn ($A \to B$, chi phí 3) vì cơ chế Cận Dưới cho thấy nhánh này có tiềm năng dẫn đến chi phí tổng thấp nhất, và cuối cùng tìm ra chi phí tối thiểu tuyệt đối là **14**.

---

**Kết luận chung:** Bài toán Tô màu đồ thị được giải bằng phương pháp **Tham Lam** dựa trên thỏa mãn ràng buộc. Bài toán Người bán hàng được giải bằng phương pháp **Nhánh và Cận** dựa trên giảm thiểu chi phí.
