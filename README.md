# House Price Prediction Project

## Mục đích dự án
Dự án này xây dựng một pipeline hoàn chỉnh để **cào, xử lý, tổng hợp** dữ liệu nhà đất từ website batdongsan.vn, tạo ra một tập dữ liệu duy nhất (`dataFinal.csv`) phục vụ cho các bài toán phân tích và dự đoán giá bất động sản ở Việt Nam.

---

## Quy trình chi tiết

### 1. Cào dữ liệu (Web Scraping)

- **Sử dụng Selenium:**  
  Script `Crawl_data.py` tự động mở trình duyệt, truy cập từng trang bất động sản, thu thập các thông tin chi tiết: tiêu đề, giá bán, diện tích, số phòng ngủ, số phòng WC, địa chỉ, hướng nhà, hướng ban công, loại nhà, tỉnh/thành, quận/huyện, thời gian đăng tin, link bài đăng.

### 2. Xử lý dữ liệu từng phần

- **Tiền xử lý dữ liệu:**  
  - Chuyển giá trị giá bán về dạng số (tỷ VND)
  - Chuẩn hóa diện tích về kiểu số thực, loại bỏ ký tự thừa.
  - Tách số phòng ngủ và WC bằng regex, chỉ giữ số.
  - Đổi tên các cột sang tiếng Việt, sắp xếp lại thứ tự các cột.
  - Chuẩn hóa địa chỉ, tách tỉnh/thành và quận/huyện.
  - Xử lý các trường hợp thiếu dữ liệu bằng cách điền giá trị NaN hoặc rỗng.
  - Chuẩn hóa thời gian đăng về dạng ngày/tháng/năm.


### 3. Gộp dữ liệu thành một file duy nhất

- **Tổng hợp dữ liệu:**  
  Sau khi xử lý xong tất cả các file trung gian, script sẽ đọc, nối/gộp chúng lại thành một file duy nhất là `dataFinal.csv`.
- **File đầu ra:**  
  `dataFinal.csv` chứa đầy đủ tất cả các bản ghi đã được chuẩn hóa và tổng hợp từ nhiều trang, đảm bảo không trùng lặp, không thừa trường và phù hợp cho phân tích.

---

## 4. Hướng dẫn phân tích và xây dựng mô hình dự đoán giá nhà

> **Lưu ý:** Repo này tập trung vào pipeline cào và xử lý dữ liệu. Việc xây dựng mô hình dự đoán giá nhà thực tế sẽ được thực hiện trên file `dataFinal.csv`.

### 4.1 Phân tích dữ liệu (EDA)

- Khám phá phân phối giá, diện tích, số phòng, loại nhà, vị trí địa lý.
- Kiểm tra dữ liệu thiếu, outlier, mối quan hệ giữa các thuộc tính.

### 4.2 Tiền xử lý bổ sung

- Mã hóa các trường phân loại (tỉnh/thành, quận/huyện, loại nhà, hướng, …) sang dạng số (Label Encoding hoặc One-hot Encoding).
- Loại bỏ các dòng dữ liệu thiếu quá nhiều hoặc bất thường.

### 4.3 Chia dữ liệu train/test

- Sử dụng scikit-learn: `train_test_split` chia tập dữ liệu thành 2 phần (ví dụ: train 70%, test 30%).

### 4.4 Xây dựng và huấn luyện mô hình

- **Linear Regression:**  
  Mô hình tuyến tính đơn giản, dễ triển khai.
- **Random Forest / XGBoost / LightGBM:**  
  Mô hình phi tuyến nâng cao, thường cho kết quả tốt hơn với dữ liệu bất động sản.
- **Các bước:**  
  - Xác định đặc trưng đầu vào (diện tích, số phòng, vị trí, loại nhà, …).
  - Huấn luyện mô hình trên tập train.
  - Dự đoán giá trên tập test.

### 4.5 Đánh giá kết quả

- **Các chỉ số đánh giá:**  
  - MAE (Sai số tuyệt đối trung bình): đo độ lệch trung bình giữa giá dự đoán và thực tế.
  - RMSE (Sai số bình phương trung bình): nhấn mạnh các sai số lớn.
  - R^2 Score: đo mức độ giải thích biến động giá nhà của mô hình.
- **Ví dụ kết quả:**  
  - Linear Regression: MAE ~ 300-500 triệu đồng.
  - Random Forest/XGBoost: MAE có thể giảm xuống 150-300 triệu đồng (tuỳ vào chất lượng và số lượng dữ liệu).

---

## Thư viện sử dụng

- **Selenium:** Cào dữ liệu từ web động.
- **Pandas, NumPy:** Xử lý và tổng hợp dữ liệu.
- **scikit-learn:** (Khuyến nghị) Phân tích dữ liệu, xây dựng và đánh giá mô hình.
- **ChromeDriver:** Kết nối Selenium với Chrome.

---

## Lưu ý quan trọng

- Nếu website batdongsan.vn thay đổi giao diện hoặc cấu trúc HTML, cần cập nhật lại mã cào dữ liệu cho phù hợp.
- Quá trình cào dữ liệu nên có thời gian chờ ngẫu nhiên để tránh bị chặn.
- File `dataFinal.csv` là đầu vào duy nhất cho các bài toán phân tích/dự đoán.
