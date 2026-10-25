# Automated Customer Feedback Sentiment Analysis System
### Hệ thống phân tích cảm xúc tự động từ đánh giá khách hàng

## 1. Giới thiệu dự án

Dự án xây dựng quy trình phân tích cảm xúc tự động từ các đánh giá sản phẩm của khách hàng trên nền tảng thương mại điện tử Tiki, sử dụng các kỹ thuật **Xử lý ngôn ngữ tự nhiên (Natural Language Processing – NLP)** và **Học máy (Machine Learning)**.

Hệ thống hướng đến việc phân loại nội dung đánh giá thành ba nhóm cảm xúc: **Tích cực (Positive), Trung tính (Neutral) và Tiêu cực (Negative)**. Thông qua việc khai phá dữ liệu văn bản, dự án tìm hiểu đặc điểm của các đánh giá, xây dựng mô hình phân loại và đánh giá hiệu quả của các phương pháp xử lý dữ liệu mất cân bằng.

Dự án được thực hiện theo quy trình từ tiền xử lý dữ liệu, khám phá và trực quan hóa dữ liệu đến trích xuất đặc trưng, huấn luyện mô hình và tối ưu hóa kết quả phân loại.

## 2. Mục tiêu dự án

- Xây dựng quy trình xử lý dữ liệu đánh giá khách hàng từ dữ liệu văn bản thô.
- Làm sạch và chuẩn hóa văn bản tiếng Việt phục vụ bài toán phân tích cảm xúc.
- Khám phá phân phối cảm xúc và các đặc điểm nổi bật của nội dung đánh giá.
- So sánh các phương pháp trích xuất đặc trưng văn bản.
- Xây dựng và đánh giá nhiều thuật toán phân loại cảm xúc.
- Phân tích các chỉ số Precision, Recall, F1-score và Confusion Matrix.
- Thử nghiệm các kỹ thuật tối ưu hóa mô hình, đặc biệt là xử lý mất cân bằng dữ liệu.

## 3. Dữ liệu sử dụng

Dữ liệu được cung cấp dưới dạng các tệp `.txt` chứa thông tin đánh giá sản phẩm. Các tệp được đọc, tổng hợp và chuyển đổi thành bảng dữ liệu để phục vụ quá trình phân tích.

Các thuộc tính chính bao gồm:

| Thuộc tính | Mô tả |
|---|---|
| `product_id` | Mã sản phẩm |
| `customer_name` | Tên khách hàng |
| `rating` | Điểm đánh giá |
| `review_title` | Tiêu đề đánh giá |
| `helpful_votes` | Số lượt đánh giá hữu ích |
| `verified_purchase` | Trạng thái xác nhận mua hàng |
| `review_date` | Ngày đánh giá |
| `review_text` | Nội dung đánh giá |

Từ thuộc tính `rating`, dữ liệu được gán nhãn thành ba nhóm cảm xúc theo quy tắc trong notebook:

- **Positive:** Điểm đánh giá từ 80 trở lên.
- **Neutral:** Điểm đánh giá từ 50 đến dưới 80.
- **Negative:** Điểm đánh giá dưới 50.

Dữ liệu có sự mất cân bằng giữa các lớp, trong đó lớp Positive chiếm tỷ trọng lớn hơn đáng kể so với Neutral và Negative. Đây là một trong những vấn đề trọng tâm được phân tích trong quá trình xây dựng mô hình.

## 4. Quy trình thực hiện

### 4.1. Tiền xử lý và khám phá dữ liệu

- Đọc và tổng hợp dữ liệu từ các tệp văn bản.
- Chuyển đổi dữ liệu thô thành DataFrame bằng Pandas.
- Kiểm tra cấu trúc, kích thước và đặc điểm của dữ liệu.
- Tính toán độ dài đánh giá dựa trên số ký tự và số từ.
- Gán nhãn cảm xúc dựa trên điểm đánh giá.

### 4.2. Tiền xử lý văn bản

Quy trình làm sạch dữ liệu văn bản bao gồm:

- Chuyển văn bản về chữ thường.
- Loại bỏ URL, HTML và các ký tự không cần thiết.
- Tách văn bản thành các token dựa trên khoảng trắng.
- Loại bỏ stopwords tiếng Việt.
- Giữ lại các từ phủ định và từ quan trọng đối với việc biểu đạt cảm xúc.
- Bổ sung danh sách stopwords theo lĩnh vực thương mại điện tử để giảm các từ ít giá trị phân biệt.

Kết quả của bước này là dữ liệu văn bản đã được làm sạch, sẵn sàng cho quá trình trích xuất đặc trưng.

### 4.3. Trực quan hóa dữ liệu

Sử dụng các thư viện trực quan hóa để khám phá đặc điểm của dữ liệu:

- Biểu đồ phân phối các lớp cảm xúc.
- Thống kê độ dài nội dung đánh giá.
- WordCloud thể hiện các từ xuất hiện phổ biến.
- So sánh đặc điểm văn bản giữa các nhóm cảm xúc.

Các kết quả trực quan hóa giúp nhận diện sự mất cân bằng dữ liệu và hỗ trợ lựa chọn phương pháp xây dựng mô hình.

### 4.4. Trích xuất đặc trưng văn bản

Sử dụng các phương pháp biểu diễn văn bản thành dữ liệu số, bao gồm:

- **CountVectorizer:** Biểu diễn văn bản dựa trên tần suất xuất hiện của từ.
- **TF-IDF (Term Frequency–Inverse Document Frequency):** Đánh giá mức độ quan trọng tương đối của từ trong văn bản và tập dữ liệu.

Các vector đặc trưng được sử dụng làm đầu vào cho những thuật toán học máy.

### 4.5. Xây dựng mô hình học máy

Notebook triển khai và thử nghiệm các thuật toán phân loại:

- **Naive Bayes:** Thử nghiệm các biến thể Multinomial Naive Bayes và Complement Naive Bayes.
- **Logistic Regression:** Xây dựng mô hình phân loại cơ sở và thử nghiệm điều chỉnh tham số.
- **Support Vector Machine (SVM):** Sử dụng mô hình SVM tuyến tính cho bài toán phân loại văn bản.
- **Random Forest:** Khảo sát mô hình tổ hợp dựa trên cây quyết định.

Ngoài ra, dự án sử dụng Cross-Validation để đánh giá độ ổn định của mô hình và thực hiện các thử nghiệm lựa chọn siêu tham số.

### 4.6. Đánh giá và tối ưu hóa mô hình

Các mô hình được đánh giá thông qua nhiều chỉ số nhằm phân tích cả hiệu quả tổng thể và khả năng nhận diện từng lớp cảm xúc.

Dự án tập trung vào:

- So sánh kết quả phân loại giữa các thuật toán.
- Phân tích sự khác biệt giữa Precision và Recall.
- Sử dụng Confusion Matrix để xác định các trường hợp phân loại nhầm.
- Điều chỉnh siêu tham số bằng Grid Search hoặc Randomized Search.
- Thử nghiệm `class_weight='balanced'` để cải thiện khả năng nhận diện các lớp thiểu số.
- Phân tích các hướng cải thiện mô hình trong bối cảnh dữ liệu mất cân bằng.

## 5. Các chỉ số đánh giá

| Chỉ số | Ý nghĩa |
|---|---|
| Accuracy | Tỷ lệ dự đoán đúng trên tổng số mẫu |
| Precision | Mức độ chính xác của các dự đoán thuộc một lớp |
| Recall | Khả năng nhận diện các mẫu thực sự thuộc một lớp |
| F1-score | Trung bình điều hòa giữa Precision và Recall |
| Macro F1-score | Trung bình F1-score của các lớp với trọng số ngang nhau |
| Confusion Matrix | Thể hiện số lượng dự đoán đúng và các trường hợp nhầm lẫn |

Do dữ liệu mất cân bằng, Accuracy không phải là chỉ số duy nhất để lựa chọn mô hình. Macro F1-score, Recall của lớp Negative và Confusion Matrix giúp đánh giá rõ hơn khả năng nhận diện các đánh giá tiêu cực.

## 6. Công nghệ và thư viện

- **Ngôn ngữ lập trình:** Python
- **Môi trường thực thi:** Google Colab, Jupyter Notebook
- **Xử lý dữ liệu:** Pandas, NumPy
- **Trực quan hóa:** Matplotlib, Seaborn
- **Xử lý ngôn ngữ tự nhiên:** Regular Expressions, CountVectorizer, TF-IDF
- **Học máy:** Scikit-learn
- **Xử lý dữ liệu ma trận thưa:** SciPy

## 7. Hướng dẫn chạy dự án

### Chạy trên Google Colab

1. Mở notebook `Automated_Customer_Feedback_Sentiment_Analysis_Systemipynb.ipynb` trên Google Colab.
2. Chuẩn bị tệp dữ liệu `tiki_dataset.zip`.
3. Chuẩn bị file stopwords tiếng Việt `vietnamese-stopwords-dash.txt`.
4. Tải các tệp cần thiết lên Colab theo yêu cầu của notebook.
5. Chạy lần lượt các cell theo thứ tự từ đầu đến cuối.
6. Theo dõi kết quả tiền xử lý, biểu đồ trực quan hóa, báo cáo phân loại và các thử nghiệm tối ưu hóa mô hình.

### Chạy trên máy tính cá nhân

1. Tải repository về máy tính.
2. Cài đặt Python và các thư viện cần thiết.
3. Chuẩn bị dữ liệu và điều chỉnh đường dẫn tệp nếu cần.
4. Mở notebook bằng Jupyter Notebook hoặc JupyterLab.
5. Chạy lần lượt các cell để tái hiện quy trình thực nghiệm.

Cài đặt các thư viện chính bằng lệnh:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn scipy jupyter
```

## 8. Kết quả và ý nghĩa

Dự án tập trung vào việc khảo sát các thuật toán phân loại cảm xúc và phân tích những yếu tố ảnh hưởng đến hiệu quả dự đoán.

Các nội dung phân tích chính bao gồm:

- Ảnh hưởng của phương pháp tiền xử lý văn bản đến dữ liệu đầu vào.
- So sánh phương pháp biểu diễn văn bản bằng CountVectorizer và TF-IDF.
- Đánh giá sự khác biệt giữa các thuật toán phân loại.
- Phân tích hạn chế của mô hình khi dữ liệu mất cân bằng.
- Khảo sát khả năng cải thiện kết quả bằng điều chỉnh siêu tham số và trọng số lớp.

Kết quả thực nghiệm cụ thể cần được đối chiếu với các báo cáo trong notebook trước khi xác định mô hình có hiệu quả tốt nhất.

## 9. Kiến thức và kỹ năng đạt được

Thông qua dự án, tôi có cơ hội thực hành:

- Thu thập, tổ chức và tiền xử lý dữ liệu văn bản.
- Khám phá và trực quan hóa dữ liệu.
- Phân tích cảm xúc bằng các kỹ thuật NLP.
- Trích xuất đặc trưng văn bản bằng CountVectorizer và TF-IDF.
- Xây dựng và so sánh các mô hình học máy.
- Đánh giá mô hình bằng nhiều chỉ số phân loại.
- Thực hiện Cross-Validation và tối ưu hóa siêu tham số.
- Phân tích vấn đề mất cân bằng dữ liệu và đề xuất hướng cải thiện mô hình.

## 10. Hướng phát triển

- Thử nghiệm các phương pháp tách từ tiếng Việt chuyên biệt.
- Khảo sát thêm các kỹ thuật xử lý mất cân bằng dữ liệu.
- Tối ưu siêu tham số theo quy trình thực nghiệm có hệ thống.
- Cải thiện khả năng nhận diện đánh giá tiêu cực.
- Thử nghiệm các mô hình ngôn ngữ tiền huấn luyện dành cho tiếng Việt.
- Phát triển giao diện cho phép người dùng nhập đánh giá và nhận kết quả phân tích cảm xúc tự động.

## Tác giả

**Vũ Khánh Vân**

GitHub: [Vukhanhvan09](https://github.com/Vukhanhvan09)

---

*Dự án phục vụ mục đích học tập và thực hành về Khai phá dữ liệu, Xử lý ngôn ngữ tự nhiên và Học máy.*
