# Mini guideline - nhóm: 2A202602169  |  người gán: Nguyễn Trọng Thắng  |  ngày: 16/09/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của nhóm bạn (phải điền)

| Tình huống | Luật nhóm bạn chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Ước lượng vị trí giải phẫu ngang thắt lưng quần, dóng từ cột sống xuống khớp háng, chọn `v=1` | Quần áo rộng/dài che hoàn toàn xương chậu, không có bề mặt nhìn thấy trực tiếp |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Thấy vành tai thì chọn `v=2`; nếu bị che >50% thì chấm ước lượng ngang mắt và chọn `v=1` | Giúp model nhận diện đúng cấu trúc vùng đầu dù có vật che phụ kiện |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Điểm trên thân gán bình thường; các khớp chân ngoài mép ảnh chọn `v=0` và không chấm | Khớp ngoài rìa ảnh không có thông tin thị giác, tránh đoán bừa toạ độ |
| Cổ tay nằm sau tay lái / sau thân mình | Dóng theo hướng cẳng tay từ khuỷu tay đến vị trí nắm, đặt chấm ước lượng và chọn `v=1` | Giữ chuỗi động học liên tục (kinematic chain) từ khuỷu tay đến bàn tay |
| Hai người chồng lên nhau | Gán dứt điểm người đứng trước; người đứng sau bị che khớp nào thì chấm ước lượng khớp đó với `v=1` | Tránh kéo nhầm khớp của người sau sang người trước (lỗi `nham_nguoi`) |
| Người nhỏ đến mức nào thì không gán nữa | Gán toàn bộ 20 ảnh core. Nếu hộp bao cao < 30px hoặc nhòe mờ không thấy rõ chi thì dừng | Đảm bảo khoảng cách giữa các khớp lớn hơn bán kính dung sai OKS |

With mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_12.jpg`, người thứ `1`, khớp `left_knee, left_ankle`

- Mơ hồ ở chỗ nào: Chân trái phía dưới bị vật cản phía trước che khuất tầm nhìn, phân vân giữa Outside (`v=0`) hay Occluded (`v=1`).
- Bạn quyết thế nào: Ước lượng vị trí khớp gối và cổ chân theo chiều dọc của cẳng chân và đánh dấu `v=1`.
- Vì sao: Thân người không bị mép ảnh cắt ngang, vị trí giải phẫu của chân vẫn nằm trọn trong giới hạn khung ảnh bên dưới vật cản.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu chọn `v=0`, model sẽ học sai rằng chân bị cụt hoặc mất tín hiệu dự đoán khớp khi có vật cản che.

### Ca 2 - ảnh `train_08.jpg`, người thứ `1`, khớp `left_knee, left_ankle`

- Mơ hồ ở chỗ nào: Khớp gối và cổ chân trái bị khuất sau vật thể che chắn phía trước.
- Bạn quyết thế nào: Dóng từ hông trái xuống theo hướng giải phẫu học, đặt chấm ước lượng với `v=1`.
- Vì sao: Tuân thủ quy tắc bắt buộc: khớp bị che nhưng còn trong khung ảnh bắt buộc phải có chấm và cờ `v=1`.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu đánh `v=0`, khớp bị loại khỏi tính toán hoặc model dự đoán thiếu khớp (giảm recall).

### Ca 3 - ảnh `train_01.jpg`, người thứ `1`, khớp `left_wrist`

- Mơ hồ ở chỗ nào: Cổ tay trái bị che khuất sau lưng / hông của người bên cạnh, không thấy bàn tay.
- Bạn quyết thế nào: Dóng theo trục cẳng tay xuất phát từ khuỷu tay trái để ước lượng vị trí cổ tay, gán `v=1`.
- Vì sao: Góc gập của khuỷu tay xác định rõ ràng hướng đi của cẳng tay, cho phép nội suy vị trí cổ tay hợp lý.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Nếu đánh `v=2` model học vị trí bị nhiễu do không có bề mặt nhìn thấy; nếu xóa khớp làm đứt đoạn khung xương.

## 4. Sau khi so visibility report với bạn cùng nhóm

- Khớp lệch `%v=1` nhiều nhất: `left_ear` (bạn `69%` / họ `52%`) và `left_hip` (bạn `45%` / họ `28%`)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: Guideline ban đầu chưa định lượng rõ tỷ lệ tóc/mũ che tai bao nhiêu thì chuyển `v=1`, và cách xử lý khớp hông khi mặc quần áo rộng.
- Luật mới bổ sung vào mục 2 sau khi thống nhất: Tai bị che trên 50% diện tích bắt buộc chọn `v=1`; khớp hông của người mặc quần áo rộng luôn ước lượng ngang thắt lưng và gán `v=1`.
