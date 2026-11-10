# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Ô tô chở người cỡ nhỏ đến trung bình, thường 4-7 chỗ. | Sedan, hatchback, SUV cá nhân. Nhỏ hơn bus, không có thùng hàng như truck, không có khoang liền khối lớn như van.|
| `truck` | Xe tải dùng để chở hàng. Có khoang lái và khu vực chở hàng riêng biệt hoặc thùng hàng phía sau. | Xe tải nhẹ, xe tải giao hàng, xe ben. Ưu tiên đặc điểm chở hàng thay vì số chỗ ngồi. |
| `bus` | Xe khách hoặc xe buýt được thiết kế để chở nhiều hành khách. |Xe buýt công cộng, xe khách đường dài. Thân xe dài, cao, nhiều cửa sổ và sức chứa lớn.|
| `van` | Xe có thân liền khối, khoang lái và khoang chở hàng/hành khách nối liền nhau. | xe giao hàng dạng kín. Không có thùng hàng hở như truck. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Bbox ôm sát phần nhìn thấy của xe, chạm gần mép ngoài của xe ở cả bốn phía, không ăn quá nhiều nền và không cắt vào thân xe.
- Vật thể bị che khuất: Vẫn gán nhãn nếu còn đủ thông tin để xác định chắc chắn là car, truck, bus hoặc van. Vẽ bbox theo phần nhìn thấy được của vật thể.
- Vật thể bị cắt bởi rìa ảnh: Vẫn gán nhãn nếu phần còn lại đủ để nhận diện lớp. Bbox chỉ bao phần xuất hiện trong ảnh.
- Vật thể rất nhỏ/mơ hồ: Không gán nhãn khi không thể xác định đáng tin cậy lớp của vật thể hoặc không phân biệt được với nền/xe khác.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_001.jpg` | Xe bị cột che khoảng 40% | Car | Vẫn nhận diện được ô tô con, theo guideline đối tượng bị che khuất nhưng đủ đặc trưng vẫn được gán nhãn. |
| `drive_003.jpg` | Xe buýt màu trắng bên phải bị cắt một phần bởi rìa ảnh | Bus | Dù bị cắt khung, vẫn thấy rõ thân xe lớn và nhiều cửa sổ, phù hợp lớp bus.|
| `drive_016.jpg` |Xe bị che khuất mạnh bởi bụi cây, chỉ còn thấy một phần nóc và kính xe | Car |Mặc dù đối tượng bị che khuất một phần lớn, vẫn còn đủ đặc điểm của ô tô con. Theo guideline, vật thể bị che khuất nhưng còn nhận diện được lớp thì vẫn gán nhãn. |
