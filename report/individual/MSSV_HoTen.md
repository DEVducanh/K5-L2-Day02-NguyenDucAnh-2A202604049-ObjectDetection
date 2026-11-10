# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên: Nguyễn Đức Anh
- MSSV: 2A202604049
- Email:nguyenanh191025@gmail.com
- Lớp: 2B
- Link repo bài làm: https://github.com/DEVducanh/K5-L2-Day02-NguyenDucAnh-2A202604049-ObjectDetection
- Link Colab đã chạy:https://colab.research.google.com/github/DEVducanh/K5-L2-Day02-NguyenDucAnh-2A202604049-ObjectDetection/blob/main/notebooks/day2_annotation_to_yolo.ipynb#scrollTo=QYnjiNbXIwe6

## 2. Schema và mini guideline

Tham khảo: GUIDELINE_MINI.md
Ba trường hợp mơ hồ đã gặp:
1. Phân biệt car và van
   - Tình huống: Một số xe nhìn giống xe con nhưng thân xe dài hơn.
   - Quyết định: Ưu tiên theo định nghĩa trong schema, nếu là xe chở người cỡ nhỏ thì gán car, nếu có thân van đặc trưng thì gán van.
   - Cần làm rõ: Tiêu chí phân biệt các trường hợp ranh giới giữa car và van.

2. Xe bị che khuất một phần
   - Tình huống: Xe nằm sau bus hoặc xe tải.
   - Quyết định: Vẫn gán nhãn nếu còn nhìn thấy đủ để nhận diện loại phương tiện.
   - Cần làm rõ: Mức độ che khuất tối thiểu để vẫn được gán nhãn.

3. Xe bị cắt bởi mép ảnh
   - Tình huống: Chỉ nhìn thấy một phần đầu hoặc đuôi xe.
   - Quyết định: Vẫn gán nhãn và box theo phần nhìn thấy được.
   - Cần làm rõ: Trường hợp chỉ còn rất ít pixel của vật thể có cần gán nhãn hay không.

## 3. Annotation và self-QC
- Số file nhãn train:
  26 file.

- Kết quả checker và lỗi đã xử lý:
  - Checker không báo lỗi format YOLO.
  - Đã sửa một số box chưa bám sát vật thể.
  - Đã rà lại các ảnh có xe nhỏ hoặc xe ở mép ảnh.

- Danh sách ít nhất 10 ảnh đã rà bằng mắt:
  - drive_001
  - drive_002
  - drive_003
  - drive_004
  - drive_005
  - drive_006
  - drive_007
  - drive_008
  - drive_009
  - drive_010

- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:
  - Bỏ sót xe nhỏ ở xa → thêm bbox.
  - Bỏ sót xe ở mép ảnh → thêm bbox.
  - Một số box lấy quá nhiều nền → chỉnh lại cho khít hơn.
  - Nhầm car và van ở một số ảnh → xem lại schema và sửa class.

- Commit khóa nhãn ban đầu:

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 433 | Chưa có |
| FP | 179 | Chưa có |
| FN | 220 | Chưa có |
| Precision | 0.7075 | Chưa có |
| Recall | 0.6631 | Chưa có |
| F1 | 0.6846 | Chưa có |
| mean_iou_of_matches | 0.8073 | Chưa có |
| passed | False | Chưa có |

Dẫn tới: outputs/annotation_evaluation.json

Rework:
- Chưa thực hiện vòng rework chính thức và chưa chạy lại đánh giá.
- Qua rà soát thủ công phát hiện một số lỗi thường gặp:
  - Bỏ sót xe nhỏ ở xa.
  - Bỏ sót xe bị cắt bởi mép ảnh.
  - Nhầm lẫn giữa car và van trong một số trường hợp khó.
  - Một số bbox chưa bám sát vật thể.

Quyết định:
- Giữ các bbox có IoU tốt và bám sát vật thể.
- Bổ sung các object bị bỏ sót khi phát hiện trong quá trình review.
- Điều chỉnh class theo schema khi có bằng chứng rõ ràng.

Lý do:
- Mean IoU = 0.8073 cho thấy các bbox đã match có chất lượng tốt.
- F1 thấp chủ yếu do còn nhiều FN và FP.
## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch:
  Xem log huấn luyện trong thư mục `assets/yolo26_runs/`.

- Các tham số thay đổi so với notebook nếu có và lý do:
  Không thay đổi tham số mặc định của notebook (nếu có thay đổi cần ghi rõ trong báo cáo).

- Đường dẫn best checkpoint trong phiên chạy:
  `/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt`

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.3739 |
| Recall | 0.4503 |
| mAP50 | 0.4636 |
| mAP50–95 | 0.2495 |
Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
- Recall thấp nhất nhiều khả năng thuộc các lớp có đối tượng nhỏ hoặc dễ nhầm lẫn như car (class 0).
Dấu hiệu hỗ trợ:
    Có nhiều trường hợp bỏ sót xe nhỏ ở xa.
    Có xe bị cắt bởi mép ảnh nhưng không được khoanh.
    Có xe bị che khuất một phần phía sau xe bus hoặc xe tải.
    Trong nhiều ảnh review, số lượng object trong file YOLO lớn hơn số bbox đã khoanh.

Ví dụ minh họa:
    Ảnh giao thông ban đêm có 12 object nhưng chỉ khoanh được khoảng 9 object.
    Ảnh đường cao tốc có 19 object nhưng thiếu khoảng 2 xe nhỏ ở hàng xe phía xa.
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
- Cần kiểm tra độ khít của bounding box.

Giải thích:
    mAP50 chỉ yêu cầu IoU ≥ 0.50 nên box tương đối đúng vị trí vẫn được tính đúng.
    mAP50-95 đánh giá ở nhiều ngưỡng IoU từ 0.50 đến 0.95 nên yêu cầu box phải khít hơn nhiều.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
- Các lỗi dễ liên quan guideline nhất là:
    car ↔ van
    car ↔ truck
    van ↔ truck
    bus ↔ truck lớn

Dữ kiện hỗ trợ:
    Đã nhiều lần cần phân biệt các lớp:
    car
    truck
    bus
    van
Trong quá trình annotation thường xuất hiện xe nhìn tương đối giống nhau khi:
    bị che khuất,
    góc nhìn xa,
    ảnh mờ,
    vật thể chỉ xuất hiện một phần.
Nếu guideline không mô tả rõ đặc điểm từng lớp thì annotator có thể gán khác nhau cho cùng một đối tượng.
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
|Đọc nhãn YOLO lần đầu | Chưa hiểu định dạng x_center, y_center, width, height | Học cách đổi từ YOLO sang tọa độ pixel| Hiểu cách đọc bbox|
|Annotation ảnh giao thông|Bỏ sót xe nhỏ ở xa|Zoom ảnh và đếm từng làn xe|Giảm một phần FN|
|Annotation ảnh giao thông|Bỏ sót xe ở mép ảnh|Bỏ sót xe ở mép ảnh|Phát hiện thêm object|

## 7. Tự đánh giá theo rubric

- Hiểu định dạng annotation YOLO.
- Đọc và diễn giải file label YOLO.
- Xác định class_id, class_name và taxonomy_name.
- Thực hiện annotation cho tập ảnh giao thông.
- Chạy QC và đọc các chỉ số:
    TP
    FP
    FN
    Precision
    Recall
    F1
    IoU
- Thực hiện review các ảnh lỗi.
- Viết nhận xét nguyên nhân làm giảm Recall và F1.

## 8. Công cụ AI và tài liệu tham khảo

- Microsoft Copilot :  Giải thích định dạng YOLO, bbox, IoU, Precision, Recall, F1

- Tài liệu tham khảo :
    Tài liệu YOLO label format trong notebook và repo bài học.
    Hướng dẫn annotation của khóa học.
    RUBRIC.md của bài thực hành.
    Kết quả QC (TP, FP, FN, Precision, Recall, F1, IoU) được sinh từ notebook.
