# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 16.1 khớp có v > 0 mỗi người
- Tổng: v=2 309 | v=1 158 | v=0 26

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 24 | 5 | 0 | 17% |
| 1 | left_eye | 21 | 8 | 0 | 28% |
| 2 | right_eye | 21 | 8 | 0 | 28% |
| 3 | left_ear | 9 | 20 | 0 | 69% |
| 4 | right_ear | 15 | 14 | 0 | 48% |
| 5 | left_shoulder | 24 | 5 | 0 | 17% |
| 6 | right_shoulder | 28 | 1 | 0 | 3% |
| 7 | left_elbow | 23 | 5 | 1 | 17% |
| 8 | right_elbow | 23 | 6 | 0 | 21% |
| 9 | left_wrist | 18 | 10 | 1 | 34% |
| 10 | right_wrist | 17 | 11 | 1 | 38% |
| 11 | left_hip | 16 | 13 | 0 | 45% |
| 12 | right_hip | 16 | 12 | 1 | 41% |
| 13 | left_knee | 16 | 9 | 4 | 31% |
| 14 | right_knee | 14 | 12 | 3 | 41% |
| 15 | left_ankle | 13 | 8 | 8 | 28% |
| 16 | right_ankle | 11 | 11 | 7 | 38% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
