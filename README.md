# NLP_MAC-SQL
Tiểu luận học phần Xử lý Ngôn ngữ Tự nhiên (NLP): Nghiên cứu kiến trúc cộng tác đa tác tử MAC-SQL cho bài toán Text-to-SQL.

# Thực nghiệm: Tái lập quy mô nhỏ (Small-scale Replication) kiến trúc MAC-SQL

Mã nguồn này thực hiện việc đánh giá (ablation study) vai trò của tác tử **Refiner** trong kiến trúc MAC-SQL (Wang et al., 2025) trên một mô hình ngôn ngữ nhỏ chạy cục bộ.

**Lưu ý quan trọng:** Do giới hạn tài nguyên của Colab Free, pipeline thực nghiệm này là một phiên bản rút gọn (chỉ tập trung vào Refiner), không phải là bản tái hiện đầy đủ 3 tác tử của bài báo gốc.

## 1. Cấu trúc thực nghiệm
Mã nguồn bao gồm các thành phần sau:
- **Backbone Model:** Sử dụng `Qwen/Qwen2.5-Coder-3B-Instruct` (chế độ fp16, chạy local trên GPU T4).
- **Tác tử Refiner:** Thiết lập vòng lặp phản hồi khép kín (Closed Feedback Loop) với môi trường SQLite Sandbox. Refiner chỉ được kích hoạt khi câu lệnh SQL sinh ra gặp lỗi runtime (ngoại lệ), với ngân sách tự sửa lỗi tối đa `maxTryTimes = 3`.
- **Baseline:** Lưu trữ kết quả sinh SQL ở lượt đầu tiên (không qua Refiner) để làm mốc so sánh hiệu năng.
- **Sandbox:** Môi trường SQLite được mở ở chế độ `read-only` (chỉ đọc) để chấm điểm Execution Accuracy (EX) tự động.

## 2. Dữ liệu thực nghiệm
- **Dataset:** Sử dụng tập `dev.json` của bộ dữ liệu chuẩn **Spider**.
- **Quy mô:** Trích xuất ngẫu nhiên **200 mẫu** (sử dụng `seed=42`) để đảm bảo tính tái lập (reproducibility).

## 3. Kết quả chính
Dựa trên kết quả đo đạc thực tế từ mã nguồn này:
- **Baseline EX (Không Refiner):** 63.5%
- **Pipeline EX (Có Refiner):** 65.0%
- **Kiểm định McNemar:** p-value = 0.25 (Refiner sửa đúng 3 câu, không làm hỏng câu nào).
*(Chi tiết số liệu được lưu trong file `bang_tong_hop.csv` và `checkpoint_main.jsonl` khi chạy code).*

## 4. Môi trường thực thi
- Python 3.10+ (đã test trên Google Colab)
- SQLite3
- Thư viện: `transformers`, `accelerate`, `bitsandbytes`, `scipy`, `pandas`, `tqdm`, `gdown`.

## 5. Hướng dẫn chạy
1. Mở file `MAC_SQL_Experiment.ipynb` trên Google Colab.
2. Bật GPU T4: `Runtime` -> `Change runtime type` -> `T4 GPU`.
3. Chạy tuần tự các ô code (Cell) từ 1 đến 13. Hệ thống sẽ tự động tải dữ liệu, mô hình và lưu kết quả (checkpoint) vào Google Drive của bạn.

## 6. Tài liệu tham khảo
Wang, B., Ren, C., Yang, J., et al. (2025). *MAC-SQL: A Multi-Agent Collaborative Framework for Text-to-SQL*. arXiv:2312.11242v6.
