# Báo cáo Lab Ngày 08: Học chủ động cho bộ phát hiện xe

Họ và tên: Nguyen Minh Trung

Công cụ gán nhãn đã dùng: CVAT

## 1. Dữ liệu và cách chia tập

Camera đứng một chỗ, một chiếc xe nằm trong hình vài giây. Ảnh học và ảnh kiểm tra phải cách nhau theo thời gian. Nếu trộn ngẫu nhiên, cùng một xe có thể vừa được AI học vừa được dùng để chấm. Điểm sẽ đẹp hơn sự thật.

## 2. Mô hình khởi đầu lạnh (cold start)

Ở vòng 0, điểm khớp khung AP50 là 0.771. Xe nhỏ chỉ được tìm thấy khoảng 0.182, xe vừa 0.547, xe lớn 0.561. Nghĩa là xe ở xa bị bỏ sót nhiều hơn xe ở gần. Trên ảnh `compare_round0.jpg`, mô hình khởi đầu lạnh có thể khoanh lệch hoặc bỏ sót các xe quá tối ở mép trái. Tuy nhiên, cần lưu ý nhãn dùng để chấm cũng do máy vẽ (pre-label), chưa có người xem từng khung, nên có thể nhãn chấm sai chứ không phải AI của bạn sai.

## 3. Chiến lược chọn mẫu

Công thức chọn ảnh tính điểm dựa trên độ bất định (U), số lượng khung hình (A), và khoảng cách thời gian (D). Mỗi ảnh có một điểm. Một nửa điểm là AI không chắc. Ba phần mười là AI vẽ nhiều khung còn lưỡng lự. Hai phần mười là ảnh có khác thời gian với ảnh khác. Hai ảnh trong cùng lô phải cách nhau ít nhất 2 giây (được quy định bởi MIN_GAP_S), vì camera đứng yên, ảnh sát nhau gần như giống hệt.
Nếu chỉ được sửa 5 ảnh, tôi chọn frame_0182.jpg, frame_0369.jpg, frame_0380.jpg, frame_0326.jpg và frame_0331.jpg. Năm ảnh này đứng đầu danh sách điểm. Điểm cao chỉ nghĩa là AI đang phân vân. Nó chưa chứng minh sửa ảnh đó xong thì AI sẽ nhận xe tốt hơn.

## 4. Các vòng học chủ động (active learning)

Trong vòng 1, tôi đã xem 12 ảnh. Mô hình đề xuất 169 box, tôi đã giữ nguyên 169 (accepted), chỉnh sửa 0 (edited), xoá 0 (deleted), và thêm mới 0 (added).

Kết quả AP50 ở vòng 1 là 0.815, tăng nhẹ 0.044 so với vòng 0 (0.771). Dù không sửa đổi nhiều khung nhãn do mô hình khởi đầu lạnh đã làm tương đối tốt, việc được huấn luyện trực tiếp trên 12 ảnh cùng bối cảnh giúp mô hình học thêm được phân bố ánh sáng và góc máy. Trên ảnh `compare_round1.jpg`, mô hình tự tin hơn trong việc bắt các xe ở xa so với vòng 0.

## 5. Kết luận và giới hạn

Dựa trên kết quả vòng 1, tôi quyết định dừng vì điểm số đã tăng và thời gian làm thủ công tốn kém, trong khi tập dữ liệu có hạn.
Hai chỗ còn yếu của mô hình hiện tại là: các xe quá nhỏ ở xa bị hòa vào bóng tối chỉ còn 2 chấm đèn vẫn dễ bị nhầm hoặc bỏ sót, và xe bị mép khung hình cắt mất một phần lớn vẫn khó bắt chuẩn.
Giới hạn của đánh giá: tập kiểm thử chỉ có 20 ảnh là quá ít để phản ánh chính xác hiệu suất tổng thể, các xe quá nhỏ không được tính vào kết quả, và nhãn tham chiếu do máy tạo ra vẫn chưa được con người rà soát thủ công nên điểm số có thể sai lệch so với độ chuẩn xác thực tế.
