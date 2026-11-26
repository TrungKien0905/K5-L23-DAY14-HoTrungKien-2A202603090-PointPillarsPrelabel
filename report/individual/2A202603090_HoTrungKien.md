# Báo cáo Day 14 — Hồ Trung Kiên — 2A202603090

- Mã học viên dùng với `--student` là `2A202603090`.
- `seed_tag` trong `frames_A.txt` là `393590736f`.

## 1. Dự đoán trước sweep (bước 5b)

Ghi dự đoán cá nhân dựa trên ảnh BEV trong `d14_2A202603090/png_A/`, rồi commit và push mục này trước khi chạy `scripts/pp_sweep.py`. Giữ nguyên dự đoán sau khi có kết quả.

- **Tham số tôi dự đoán sẽ giảm công sửa nhiều nhất** — Tăng `score_thr` so với mức gốc `0.1` để lọc các hộp có độ tin cậy thấp.
- **Lý do** — Khi xem ảnh BEV của tập A, tôi thấy mô hình tạo nhiều hộp thừa, trong đó có hộp gán nhầm cho chính xe thu dữ liệu. Một vài hộp quanh xe thật cũng bị sai hướng. Tôi dự đoán tăng ngưỡng tin cậy sẽ lọc được một phần hộp thừa và giảm số hộp phải xóa khi sửa nhãn.

## 2. Tập A: sửa tay và tune

| Cấu hình | Xóa | Thêm | Sửa | Chi phí |
|---|---:|---:|---:|---:|
| Gốc | 56 | 1 | 3 | 19,3 |
| `best_config.json` | 19 | 8 | 2 | 14,7 |

Chi phí sửa được tính trong bán kính 50 m theo công thức `0,3 × xóa + 1,0 × thêm + 0,5 × sửa`. [Kết quả 24 cấu hình đã thử](../../d14_2A202603090/sweep.csv).

| Frame | Chi phí gốc | Chi phí với `best_config.json` |
|---|---:|---:|
| `s03f25` | 7,2 | 8,4 |
| `s04f39` | 4,1 | 3,1 |
| `s05f00` | 8,0 | 3,2 |

**Tham số thay đổi.** Cấu hình đã chọn tăng `score_thr` từ `0.1` lên `0.4`, giữ `min_points=0` và `range_m=50` như cấu hình gốc. [Cấu hình đã lưu](../../d14_2A202603090/best_config.json).

**Kết quả leave-one-out.** Mỗi lượt chọn cấu hình trên hai frame và chấm trên frame còn lại. Các dòng dưới đây được tính lại từ `sweep.csv` theo cách chọn của `scripts/pp_sweep.py`.

```text
s03f25: cấu hình chọn {'score_thr': '0.4', 'min_points': '0', 'range_m': '50'} → 8.4 (tốt nhất có thể 5.1)
s04f39: cấu hình chọn {'score_thr': '0.4', 'min_points': '0', 'range_m': '40'} → 5.1 (tốt nhất có thể 2.9)
s05f00: cấu hình chọn {'score_thr': '0.3', 'min_points': '10', 'range_m': '50'} → 5.6 (tốt nhất có thể 3.2)
```

Lượt chấm nào cũng chọn một tổ hợp khác nhau và chi phí đều cao hơn mức thấp nhất có thể trên frame được giữ lại. Trên cả tập A, cấu hình đã chọn đạt chi phí thấp nhất trong lưới đã thử (14,7), giảm 4,6 điểm chi phí so với cấu hình gốc. Riêng `s03f25`, chi phí tăng từ 7,2 lên 8,4.

## 3. Trước/sau (bước 8)

![so sánh](../../d14_<ID>/compare/<frame>.jpg)

- `scripts/pp_compare.py` báo giảm (hoặc tăng) ...% công sửa trên tập A.
- Dự đoán ở mục 1 đúng hay sai? Giải thích bằng số box xóa/thêm/sửa: ...

## 4. Tập B

Số đo từ `scripts/pp_prelabel.py --frames d14_<ID>/frames_B.txt --config d14_<ID>/best_config.json --gt d14_<ID>/ref_B.json`:

| Xóa | Thêm | Sửa | Chi phí |
|---:|---:|---:|---:|
| | | | |

- Loại box tốn công sửa nhất ở tập B: ...

## 5. Giới hạn

- Nêu ít nhất 2 giới hạn (ví dụ: chỉ 3 frame tune; model KITTI không có Truck/Bus; chi phí là công thức, chưa phải thời gian sửa thật): ...
