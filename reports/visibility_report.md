# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 28 skeleton, trung bình 15.93 khớp có v > 0 mỗi người
- Tổng: v=2 295 | v=1 151 | v=0 30

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 23 | 5 | 0 | 18% |
| 1 | left_eye | 20 | 8 | 0 | 29% |
| 2 | right_eye | 20 | 8 | 0 | 29% |
| 3 | left_ear | 8 | 20 | 0 | 71% |
| 4 | right_ear | 14 | 14 | 0 | 50% |
| 5 | left_shoulder | 24 | 4 | 0 | 14% |
| 6 | right_shoulder | 27 | 1 | 0 | 4% |
| 7 | left_elbow | 23 | 4 | 1 | 14% |
| 8 | right_elbow | 22 | 6 | 0 | 21% |
| 9 | left_wrist | 18 | 9 | 1 | 32% |
| 10 | right_wrist | 16 | 11 | 1 | 39% |
| 11 | left_hip | 15 | 13 | 0 | 46% |
| 12 | right_hip | 15 | 12 | 1 | 43% |
| 13 | left_knee | 15 | 7 | 6 | 25% |
| 14 | right_knee | 13 | 12 | 3 | 43% |
| 15 | left_ankle | 12 | 6 | 10 | 21% |
| 16 | right_ankle | 10 | 11 | 7 | 39% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
