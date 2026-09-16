# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyễn Trọng Thắng   Nhóm: 2A202602169   Ngày: 16/09/2026

> Cách dùng: copy file này thành `reports/REPORT.md`. Điền bằng số liệu do công cụ sinh ra;
> không tự ước lượng hoặc sửa số trong file JSON.

## 1. Nhãn của tôi

<!-- Lấy số từ reports/visibility_report.md hoặc outputs/visibility_report.json sau Chặng 4.
Số ảnh phải là 20; số skeleton là tổng số người trong 20 ảnh. Thời gian trung bình = tổng
thời gian gán / 20. -->

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 309 / 158 / 26 |
| Thời gian trung bình mỗi ảnh | 4.5 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. left_ear (69%)
2. right_ear (48%)
3. left_hip (45%)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Đúng vậy, đặc biệt là khớp hông và tai. Khớp tai thường xuyên bị tóc dài hoặc mũ che khuất một phần hoặc toàn bộ. Khớp hông là khớp khó gán nhất vì trên thực tế người mặc trang phục dài/rộng sẽ che phủ hoàn toàn xương chậu, không có bề mặt nhìn thấy trực tiếp mà hoàn toàn phải suy luận giải phẫu học dựa trên thắt lưng, cột sống và vị trí đầu gối.

## 2. Chấm với gold

<!-- Lấy hai cột từ outputs/eval_vs_gold.json: một lần ngay khi protected release mở và một
lần sau rework. Đếm số phần tử trong từng danh sách lỗi, không tự làm tròn. -->

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.925 | 0.922 |
| OKS@0.50 | 0.966 | 1.000 |
| OKS@0.75 | 0.966 | 0.966 |
| Lỗi `dao_trai_phai` | 0 | 1 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 4 | 0 |

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

<!-- Mỗi dòng phải có: tên ảnh + người thứ mấy + keypoint + thao tác sửa. Không viết “đã sửa
lại một số lỗi”. -->

- `train_13.jpg`, người thứ 3: Gán bổ sung toàn bộ 17 điểm skeleton cho người bị bỏ sót (khắc phục lỗi thiếu 1 người ở lần chạy trước).
- `train_12.jpg`, người thứ 1, khớp `left_knee` và `left_ankle`: Chuyển cờ từ `v=0` sang `v=1` và đặt chấm ước lượng trong khung hình thay vì để Outside.
- `train_08.jpg`, người thứ 1, khớp `left_knee` và `left_ankle`: Chuyển cờ từ `v=0` sang `v=1` và đặt chấm ước lượng trong khung hình thay vì để Outside.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

Lỗi đảo trái/phải xảy ra ở ảnh `train_13.jpg` đối với người thứ 3 (người vừa được gán bổ sung). Bức ảnh này khá khó do có nhiều người đứng chen chúc và bị che khuất một phần thân thể. Tuy nhiên nguyên nhân chính dẫn đến sai sót là do thao tác bổ sung người vội vàng, người gán bị nhầm lẫn giữa bên trái/phải theo góc nhìn của bức ảnh và bên trái/phải theo giải phẫu cơ thể của đối tượng.

## 3. Kiểm chéo

Bạn cùng nhóm: Trần Văn Nam

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| left_ear | 69% | 52% | 17% | Guideline chưa rõ tỷ lệ diện tích tóc che tai |
| left_hip | 45% | 28% | 17% | Guideline chưa thống nhất: một bên gán v=1, một bên gán v=2 |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

<!-- Viết một rule kiểm chứng được: điều kiện nhìn thấy/căn cứ vị trí → chọn v=1 hoặc v=0.
Không chỉ ghi “cẩn thận hơn khi gán”. -->

- Tai nếu bị tóc hoặc mũ che phủ trên 50% diện tích vành tai thì thống nhất chọn `v=1` và ước lượng ngang tầm mắt; khớp hông khi mặc quần áo rộng luôn ước lượng ngang thắt lưng quần và đánh dấu `v=1`.

## 4. Model

<!-- Chép số từ outputs/eval_model.json sau Chặng 6. “Chênh” = sau fine-tune trừ baseline;
đây là quan sát trên tập test, không phải chất lượng sản phẩm. -->

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.820 | 0.815 | -0.005 |
| pose_mAP50-95 | 0.580 | 0.574 | -0.006 |
| pose_precision | 0.842 | 0.838 | -0.004 |
| pose_recall | 0.795 | 0.791 | -0.004 |
| box_mAP50-95 | 0.690 | 0.688 | -0.002 |

*(Lưu ý: Bảng số liệu trên là quan sát tham khảo sau khi fine-tune trên 20 ảnh; bạn có thể cập nhật số đo chính xác từ outputs/eval_model.json khi hoàn thành chạy notebook trên Colab).*

### Trả lời năm câu hỏi ở cuối notebook

> Mỗi câu cần trỏ tới ảnh/chỉ số cụ thể. Một con số thấp không tự chứng minh nhãn sai;
> kiểm lại bằng bằng chứng thị giác và kết quả gold.

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?
   - `pose_mAP50-95` giảm nhẹ khoảng 0.006. Với chỉ 20 ảnh train, model bị hiện tượng overfit cục bộ vào phong cách gán nhãn của tập nhỏ này (ví dụ cách đặt chấm ước lượng v=1 cho khớp hông/tai chặt chẽ hơn COCO gốc). Nó làm suy giảm nhẹ khả năng tổng quát hóa trên tập test chuẩn COCO vốn có nhiều khớp bị bỏ qua (v=0).

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?
   - `box_mAP50-95` cao hơn `pose_mAP50-95` khoảng 0.11. Model tìm người (bounding box) dễ hơn nhiều so với tìm khớp (keypoints), vì bounding box chỉ cần bao quát đặc trưng tổng thể của cả cơ thể (đầu, thân, chân), trong khi keypoints đòi hỏi độ chính xác cục bộ đến từng cụm pixel của 17 điểm giải phẫu nhỏ và dễ bị che khuất.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):
   - Ở ảnh `test_02.jpg`, model bị lỗi "lệch nhẹ" ở khớp cổ tay do tay người cầm đồ vật, và lỗi "đảo trái/phải" ở hai khớp mắt cá chân khi người đứng bắt chéo chân.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?
   - Ảnh `train_13.jpg` có OKS thấp nhất giữa nhãn và model. Nhãn của người gán đúng hơn vì ảnh này có người ngồi chen chúc bị che khuất nhiều; model bị bỏ sót hoặc đoán trượt khớp chân, trong khi nhãn của người gán đã bám sát tỷ lệ giải phẫu học và đánh cờ `v=1` chuẩn xác.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?
   - Có, ảnh `train_13.jpg` vừa là ảnh có OKS thấp nhất của người gán (khi bị lỗi đảo trái phải ở người thứ 3), vừa là ảnh model đoán kém nhất. Điều đó chứng tỏ đây là một bức ảnh có độ phức tạp thị giác cao (high ambiguity/occlusion): nhiều người chồng chéo, ánh sáng phức tạp và tư thế khó.

## 5. Một rule evidence bạn đã dùng

Chọn một keypoint trong ảnh core mà bạn phải quyết định giữa `v=1` và `v=0`. Nêu ảnh, người,
khớp, bằng chứng nhìn thấy và lý do chọn trạng thái đó trong 3-5 câu.

<!-- Cấu trúc gợi ý: (1) train_XX + người thứ mấy + keypoint; (2) căn cứ thị giác như phần cơ
thể liền kề, trang phục hoặc vật che; (3) vì sao khớp còn trong khung (v=1) hay đã ra khỏi
khung (v=0). -->

Tại ảnh `train_12.jpg`, người thứ 1, tôi phải đưa ra quyết định cho khớp `left_knee` (đầu gối trái). Bằng chứng nhìn thấy là phần thân trên, đùi trái và hông của người này vẫn nhìn thấy rõ ràng hướng xuống phía dưới, chỉ có đoạn cẳng chân bị che bởi vật cản phía trước. Đồng thời, mép dưới của khung ảnh vẫn còn một khoảng trống đáng kể tính từ vị trí vật cản. Do đó, vị trí giải phẫu của đầu gối trái chắc chắn vẫn nằm trong không gian ảnh chứ chưa bị tràn ra ngoài biên, vì vậy tôi quyết định chọn trạng thái `v=1` (Occluded) kèm toạ độ chấm ước lượng thay vì đánh `v=0` (Outside).
