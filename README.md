# NLP_MAC-SQL
Tiểu luận học phần NLP: Nghiên cứu kiến trúc đa tác tử MAC-SQL cho bài toán Text-to-SQL
# MAC-SQL Algorithm 1 Prototype Implementation

Mã nguồn mô phỏng và kiểm chứng thuật toán điều phối đa tác tử (Algorithm 1) theo kiến trúc của framework MAC-SQL (Wang et al., 2025).

## Cấu trúc thực nghiệm
1. **Selector Agent:** Kiểm tra kích thước token lược đồ với ngưỡng an toàn `tokens > 0.8 * L` để tự động cắt tỉa bảng gây nhiễu, bảo toàn 100% Primary Key và Foreign Key.
2. **Decomposer Agent:** Mô hình hóa chuỗi tư duy (Chain-of-Thought) phân rã bài toán đa bước với trần khống chế `L ∈ [1, 5]`.
3. **Refiner Agent:** Thiết lập vòng lặp phản hồi khép kín (Closed Feedback Loop) với môi trường SQLite Sandbox, ngân sách tự sửa lỗi tối đa `maxTryTimes = 3`.

## Môi trường thực thi
- Python 3.10+
- SQLite3
- tiktoken
