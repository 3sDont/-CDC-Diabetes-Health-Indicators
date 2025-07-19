# Project: Phân tích và Xây dựng Mô hình Dự đoán Bệnh Tiểu đường

Dự án này phân tích bộ dữ liệu "CDC Diabetes Health Indicators" từ khảo sát BRFSS 2015 để tìm ra các yếu tố ảnh hưởng chính và xây dựng mô hình dự đoán khả năng mắc bệnh tiểu đường.

## Cấu trúc Thư mục

```
.
├── data/
│   └── diabetes_012_health_indicators_BRFSS2015.csv   # Dữ liệu gốc
├── report/
│   └── Bao_cao_Nhom12_project2.html                   # Báo cáo kết quả phân tích
├── .gitignore
├── README.md                                          # File giới thiệu dự án
└── analysis.Rmd                                       # File mã nguồn R Markdown
```

## Dữ liệu

-   **Nguồn:** CDC's Behavioral Risk Factor Surveillance System (BRFSS) 2015.
-   **Kích thước:** 253,680 quan sát và 22 thuộc tính.
-   **Biến mục tiêu:** `Diabetes_012` (0 = Không tiểu đường, 1 = Tiền tiểu đường, 2 = Tiểu đường). Trong phân tích này, nhóm 1 và 2 được gộp thành "Diabetes".

## Quy trình Phân tích

1.  **Tiền xử lý:** Dữ liệu được nhập và làm sạch. Không có giá trị thiếu.
2.  **Phân tích Khám phá (EDA):**
    -   Trực quan hóa sự mất cân bằng của biến mục tiêu (số người không mắc bệnh nhiều hơn đáng kể).
    -   Phân tích phân phối của các yếu tố như Giới tính, Tuổi, và BMI cho cả hai nhóm (mắc bệnh và không mắc bệnh).
    -   Phát hiện các yếu tố có khả năng liên quan cao như Huyết áp cao, Cholesterol cao, BMI, và Tuổi.
3.  **Kiểm định Giả thuyết:**
    -   Sử dụng kiểm định hoán vị (Permutation Test) để so sánh chỉ số BMI trung bình giữa hai nhóm.
    -   Sử dụng kiểm định Chi-squared để đánh giá mối liên hệ giữa tình trạng tiểu đường và các yếu tố như Cholesterol cao, Huyết áp cao.
4.  **Xây dựng Mô hình:**
    -   **Lựa chọn Đặc trưng:** Sử dụng Chi-squared test để chọn ra 6 đặc trưng quan trọng nhất.
    -   **Xử lý Mất cân bằng:** Áp dụng kỹ thuật SMOTE (Synthetic Minority Over-sampling Technique) trên tập huấn luyện để cân bằng số lượng mẫu giữa các lớp.
    -   **Mô hình:**
        -   **Logistic Regression:** Mô hình cơ sở để phân loại.
        -   **Random Forest:** Mô hình mạnh hơn để cải thiện hiệu suất.
    -   **Đánh giá:** Các mô hình được đánh giá dựa trên Độ chính xác (Accuracy), Độ nhạy (Sensitivity), Độ đặc hiệu (Specificity), và Ma trận nhầm lẫn (Confusion Matrix).

## Kết quả

-   **Logistic Regression:** Đạt độ chính xác **70.4%**. Tuy nhiên, độ chính xác dự đoán dương tính (Precision) khá thấp (31.3%), cho thấy mô hình có xu hướng dự đoán sai nhiều trường hợp "dương tính".
-   **Random Forest:** Cải thiện độ chính xác lên **78.6%**. Mô hình này có độ đặc hiệu (Specificity) cao (84%), tức là dự đoán tốt các trường hợp "không mắc bệnh", nhưng độ nhạy (Sensitivity) chỉ đạt 50%.

**Kết luận:** Cả hai mô hình đều cho thấy các yếu tố như huyết áp, cholesterol, và BMI là những chỉ báo quan trọng. Tuy nhiên, để ứng dụng trong thực tế, cần cải thiện thêm khả năng phát hiện chính xác các trường hợp mắc bệnh (tăng Sensitivity và Precision).

## Yêu cầu

Để chạy lại phân tích này, bạn cần cài đặt các thư viện R sau:

```R
install.packages(c("tidyverse", "janitor", "caret", "corrplot", "VIM", "smotefamily", "randomForest", "cowplot"))
```

## Cách chạy

1.  Clone repository này về máy của bạn.
2.  Mở file `analysis.Rmd` bằng RStudio.
3.  Đảm bảo file dữ liệu `diabetes_012_health_indicators_BRFSS2015.csv` nằm trong thư mục `data/`.
4.  Nhấn nút "Knit" trong RStudio để chạy toàn bộ mã và tạo lại file báo cáo HTML.
