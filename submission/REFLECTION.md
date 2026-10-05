# Reflection — Lab 19

**Tên:** Nguyễn Văn Tài
**Cohort:** A20-K4
**Path đã chạy:** lite

---

## Câu hỏi (≤ 200 chữ)

> Trên golden set 50 queries, mode nào thắng ở loại query nào (`exact` /
> `paraphrase` / `mixed`), và tại sao? Khi nào bạn **không** dùng hybrid
> (i.e. khi nào pure BM25 hoặc pure vector là lựa chọn đúng)?

Precision@10 trung bình: hybrid 78.6% > BM25 77.8% > vector 73.2%.

- **exact** (BM25 96.7%, vector 88.7%, hybrid 96.7%): query chứa đúng thuật
  ngữ trong corpus nên BM25 đã đủ mạnh; hybrid chỉ ngang bằng.
- **paraphrase** (BM25 33.3%, vector 24.0%, hybrid 32.0%): cả hai đều yếu.
  `bge-small-en` được train cho tiếng Anh nên không nắm được câu tiếng Việt
  diễn đạt lại. Vấn đề nằm ở model, không ở cách fusion; cần đổi sang `bge-m3`.
- **mixed** (97.0% / 98.5% / **100%**): hybrid thắng rõ vì RRF gộp được
  tín hiệu từ khoá và ngữ nghĩa, và đây là kiểu query người dùng thật hay gõ nhất.

**Khi nào không dùng hybrid:** tra mã lỗi, SKU, tên hàm, ID, tức các trường
hợp cần khớp chính xác: dùng thuần BM25 (vector có thể trả về kết quả "gần
nghĩa" nhưng sai). Còn khi corpus đa ngôn ngữ hoặc query toàn diễn đạt lại
và đã có embedding tốt, thì thuần vector là đủ: BM25 chỉ thêm nhiễu và tăng
latency (hybrid phải chạy 2 retriever với depth 50).

---

## Điều ngạc nhiên nhất khi làm lab này

Hybrid chỉ hơn BM25 0.8 điểm trung bình, và trên paraphrase thì vector
(24%) còn thua BM25. Chọn embedding model quan trọng hơn chọn thuật toán fusion.

---

## Bonus challenge

- [ ] Đã làm bonus (xem `bonus/`)
- [ ] Pair work với: _<tên đồng đội nếu có>_
