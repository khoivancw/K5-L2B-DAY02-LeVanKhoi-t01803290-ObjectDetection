# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` | Phương tiện cá nhân chở người từ 4-7 chỗ hoặc xe bán tải cỡ nhỏ. | Sedan, SUV, Crossover, Hatchback, Taxi; **Bao gồm cả xe bán tải (Pickup truck)** vì khung gầm xe con. Không gán mô tô, xe máy, xe đạp. |
| `truck` | Xe chuyên dụng chở hàng hóa tải trọng lớn, có khoang lái tách rời hoặc cabin chuyên biệt. | Xe tải thùng, xe ben, xe bồn, xe đầu kéo container (kể cả có kéo rơ-moóc hay chạy không). Không gán xe bán tải chở hàng nhỏ. |
| `bus` | Phương tiện chở khách công cộng cỡ lớn từ 29 chỗ trở lên. | Xe buýt nội đô, xe khách liên tỉnh, xe buýt hai tầng, xe buýt khớp nối dài. Không gán xe minivan chở người 16 chỗ. |
| `van` | Phương tiện dạng thân hộp kín (box-like), từ 9-16 chỗ chở người hoặc khoang kín chở hàng cỡ trung. | Minivan gia đình, xe chuyển phát nhanh (như Ford Transit, Solati, Hiace, Starex). Thân xe dạng khối hộp liền mạch, mũi ngắn. |

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Bounding box phải ôm khít viền pixel ngoài cùng của toàn bộ thân xe nhìn thấy được (tight bounding box). Tuyệt đối không để hở khoảng trống mặt đường, bóng đổ (shadow), bầu trời hoặc gộp nhiều xe vào một hộp.
- Vật thể bị che khuất: Chỉ vẽ box bao quanh **phần thân xe thực sự nhìn thấy được (visible body)**. Không vẽ ước lượng hay dự đoán phần bị xe khác hoặc chướng ngại vật che mất.
- Vật thể bị cắt bởi rìa ảnh: Vẽ box chạm sát vào đúng cạnh rìa của ảnh (x=0, y=0, x=W hoặc y=H). Tuyệt đối không vẽ hộp vượt ra ngoài khung hình ảnh hoặc đoán phần ngoài biên.
- Vật thể rất nhỏ/mơ hồ: Nếu xe ở hậu cảnh xa nhưng mắt người vẫn nhận diện được rõ hình khối và xác định chắc chắn thuộc 1 trong 4 lớp -> vẫn gán box. Nếu chỉ là chấm sáng mờ, vệt nhòe chuyển động dưới 10 pixel không thể xác định loại xe -> bỏ qua để tránh tạo False Positive.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_008.jpg` | Xe bán tải (Pickup truck) có thùng hở phía sau chở ít đồ. | Gán lớp `car` (ID 0) | Theo schema, xe bán tải thuộc nhóm phương tiện cá nhân cỡ nhỏ, khung gầm nhẹ, không tính vào `truck` thương mại. |
| `drive_022.jpg` | Xe khách 16 chỗ dáng dài màu bạc đi cùng chiều (dáng giống Minivan nhưng chở khách). | Gán lớp `van` (ID 3) | Kích thước và số chỗ (dưới 16 chỗ, thân hộp liền mạch) thuộc định nghĩa `van`, chưa đạt ngưỡng xe buýt/xe khách lớn của `bus`. |
| `drive_033.jpg` | Xe con ở làn trong cùng bị cột đèn/biển báo giao thông che cắt ngang thân xe. | Vẽ 1 box ôm trọn thân xe nhìn thấy, bao gồm cả cột che ở giữa | Không tách làm 2 box rời rạc vì đây là một vật thể nguyên vẹn; box ôm phần nhìn thấy của xe và không đoán đuôi xe ngoài lề đường. |
