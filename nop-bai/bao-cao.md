# Báo Cáo Lab Day 21 - CI/CD cho AI Systems

<!--
HƯỚNG DẪN - đọc rồi XÓA TOÀN BỘ các khối chú thích này sau khi điền xong:

  - Giới hạn: KHÔNG QUÁ 1 TRANG A4, tương đương khoảng 450 - 550 từ nội dung.
  - Chỉ điền vào các chỗ ___ và các ô trong bảng. Không thêm mục mới.
  - Viết bằng câu hoàn chỉnh, không gạch đầu dòng cụt lủn.
  - Kiểm tra độ dài sau khi đã xóa hết chú thích:
        wc -w nop-bai/bao-cao.md
    và xem trước bản in bằng cách mở file trên GitHub rồi Ctrl+P / Cmd+P.
-->

| | |
|---|---|
| Họ và tên | Nguyễn Đình LÂm Phúc |
| MSSV | 2A202602986 |
| Lớp / Khóa | K4 |
| Repo GitHub | https://github.com/NguyenDinhLamPhuc/K4-L3-DAY21-NguyenDinhLamPhuc-2A202602986-CI-CD-for-AI-Systems |
| Ngày nộp | 7/10/2026 |

---

## 1. Bộ Siêu Tham Số Đã Chọn và Lý Do

<!-- Khoảng 120 - 150 từ. Điền kết quả thật từ MLflow UI ở Bước 1, tối thiểu 3 lần chạy. -->

| Lần chạy | n_estimators | learning_rate | max_depth | f1_score | accuracy |
|---|---|---|---|---|---|
| 1 | 100 | 0.1 | 3 | 0.7109 | 0.8780 |
| 2 | 50 | 0.05 | 2 | 0.6051 | 0.0.8460 |
| 3 | 200 | 0.1 | 5 | 0.7149 | 0.8740 |

**Bộ siêu tham số đã chọn:** `n_estimators=200`, `learning_rate=0.1`, `max_depth=5`.

**Lý do:** Bộ n_estimators=200, learning_rate=0.1, max_depth=5 được chọn vì đạt f1_score=0.7149, cao nhất trong ba lần chạy, cho thấy mô hình nhận diện lớp thu nhập cao tốt hơn các bộ còn lại. Mặc dù lần chạy 1 có accuracy cao nhất (0.8780), accuracy không phản ánh đầy đủ khả năng dự đoán lớp thiểu số; bộ được chọn có accuracy thấp hơn một chút (0.8740) nhưng có F1 cao hơn (0.7149 so với 0.7109). Điều này cho thấy accuracy cao chưa chắc đồng nghĩa với mô hình tốt hơn. So sánh lần 2 và lần 3 cũng cho thấy khi tăng số cây từ 50 lên 200, giữ learning_rate=0.1 và tăng độ sâu, F1 cải thiện đáng kể. Nhìn chung, learning rate thấp thường cần nhiều cây hơn để đạt hiệu quả tương đương.

<!--
Trả lời trong phần Lý do:
  - Vì sao bộ này tốt hơn các bộ còn lại (dựa trên f1_score, không phải accuracy)?
  - Lần chạy có accuracy cao nhất có trùng với lần có f1_score cao nhất không?
    Nếu không, điều đó nói lên điều gì?
  - Bạn quan sát thấy đánh đổi nào giữa n_estimators và learning_rate?
-->

---

## 2. Vì Sao Ngưỡng Chất Lượng Đặt Trên F1 Chứ Không Phải Accuracy

<!-- Khoảng 120 - 150 từ. -->

Tập dữ liệu có sự mất cân bằng rõ rệt: chỉ khoảng 24,8% mẫu thuộc lớp thu nhập cao hơn 50.000 USD, trong khi khoảng 75,2% thuộc lớp thu nhập thấp. Vì vậy, một mô hình luôn dự đoán “thu nhập thấp” vẫn đạt accuracy khoảng 0,752, nhưng hoàn toàn không phát hiện được người có thu nhập cao và có F1 bằng 0. Accuracy chỉ phản ánh tỷ lệ dự đoán đúng tổng thể, nên dễ tạo cảm giác mô hình hoạt động tốt. Ngược lại, F1 của lớp dương kết hợp precision và recall, qua đó đánh giá khả năng dự đoán đúng cũng như phát hiện đầy đủ lớp thu nhập cao. Lab sử dụng F1 cho lớp dương bằng cách giữ mặc định average="binary", không dùng weighted hoặc macro, vì các cách này có thể bị lớp đa số chi phối và làm chỉ số cao giả tạo.

<!--
Cần nêu được:
  - Phân bố lớp của tập dữ liệu (tỷ lệ lớp thu nhập > 50K) và hệ quả của nó.
  - Accuracy của một mô hình luôn trả lời "thu nhập thấp" là bao nhiêu, vì sao con số
    đó gây hiểu nhầm.
  - F1 của lớp dương đo điều gì mà accuracy không đo được.
  - Vì sao KHÔNG dùng average="weighted" hay average="macro" khi gọi f1_score.
-->

---

## 3. Khó Khăn Gặp Phải và Cách Giải Quyết

<!-- Nêu 2 - 3 khó khăn thật, mỗi ô một câu ngắn. -->

| Khó khăn | Nguyên nhân | Cách giải quyết |
|---|---|---|
| ___ | ___ | ___ |
| ___ | ___ | ___ |
| ___ | ___ | ___ |

---

## 4. So Sánh Bước 2 và Bước 3 (bắt buộc, 2 - 3 câu)

<!-- Lấy số liệu từ bảng ở mục 3.6 của tasks/buoc-3.md. -->

| | f1_score | accuracy |
|---|---|---|
| Bước 2 (chỉ `train_batch1`) | ___ | ___ |
| Bước 3 (thêm `train_batch2`) | ___ | ___ |

**Nhận xét:** ___

<!--
Một câu trả lời trung thực kiểu "f1 giảm 0,01 vì dữ liệu mới cùng phân phối, không mang
thêm thông tin mới" được đánh giá cao hơn kết luận sai rằng thêm dữ liệu luôn tốt hơn.
-->

---

## 5. Phần Bonus Đã Thực Hiện (nếu có)

<!-- Xóa cả mục 5 nếu không làm bonus. Mỗi bonus tối đa 1 dòng. -->

- [ ] Bonus 1 - Tracking MLflow từ xa với DagsHub: ___
- [ ] Bonus 2 - Điều chỉnh ngưỡng quyết định: ___
- [ ] Bonus 3 - Báo cáo precision / recall tự động: ___
- [ ] Bonus 4 - Hoàn trả về phiên bản trước: ___
- [ ] Bonus 5 - Cảnh báo lệch lạc dữ liệu: ___
