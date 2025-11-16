# Scenario - Generation với Batch

## Use Case: Tạo yêu cầu generation

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Tạo yêu cầu generation |
| **Actor** | User |
| **Tiền điều kiện** | User đã đăng nhập thành công và đang ở giao diện Session chi tiết, Session có ít nhất một Document |
| **Hậu điều kiện** | Yêu cầu generation được tạo và tiến trình generation bắt đầu |
| **Kịch bản chính** | 1. Tại giao diện Session chi tiết, User click nút "Generate"<br>2. Hệ thống hiển thị QuestionGenDialog<br>3. Hệ thống kiểm tra xem có generation process đang chạy không<br>4. Nếu không có process đang chạy, hiển thị form generation<br>5. User chọn các tham số generation:<br>&nbsp;&nbsp;&nbsp;&nbsp;- Document nguồn (có thể chọn nhiều)<br>&nbsp;&nbsp;&nbsp;&nbsp;- Số lượng Question/Flashcard<br>&nbsp;&nbsp;&nbsp;&nbsp;- Topic (tùy chọn)<br>&nbsp;&nbsp;&nbsp;&nbsp;- Cognitive levels (cho Question)<br>&nbsp;&nbsp;&nbsp;&nbsp;- Question types<br>&nbsp;&nbsp;&nbsp;&nbsp;- Flashcard types<br>6. User click nút "Generate"<br>7. Hệ thống gửi yêu cầu batch generate đến backend<br>8. Backend tạo GenerationProcess và GenerationBatch cho Question và/hoặc Flashcard<br>9. Backend trả về BatchGenerateResponse với question_process_id và flashcard_process_id<br>10. Hệ thống nhận response và tự động kết nối WebSocket với các process<br>11. Dialog đóng lại và hệ thống bắt đầu hiển thị LiveProgress trong tab Question/Flashcard |
| **Ngoại lệ** | - Không có Document: hiển thị thông báo lỗi<br>- Có process đang chạy: hiển thị thông báo và không cho phép tạo mới<br>- Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Lỗi tạo process: hiển thị thông báo lỗi từ server |

---

## Use Case: Theo dõi tiến trình generation

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Theo dõi tiến trình generation |
| **Actor** | User |
| **Tiền điều kiện** | User đã tạo yêu cầu generation hoặc có process đang chạy |
| **Hậu điều kiện** | User nhìn thấy tiến trình generation real-time |
| **Kịch bản chính** | 1. Sau khi tạo yêu cầu generation, hệ thống kết nối WebSocket với process<br>2. Frontend gửi message "start" hoặc "resume" tới WebSocket<br>3. Backend nhận message và bắt đầu/resume generation pipeline<br>4. Backend gửi "snapshot" event với toàn bộ process và process_steps<br>5. Hệ thống nhận snapshot và hiển thị LiveProgress với danh sách steps<br>6. Trong quá trình generation, backend gửi "state" event mỗi khi step thay đổi<br>7. Hệ thống cập nhật trạng thái step (pending → running → completed/failed)<br>8. Hệ thống hiển thị progress bar và thông tin chi tiết của từng step<br>9. User có thể expand/collapse step để xem chi tiết<br>10. Khi generation hoàn thành, backend gửi "finished" event<br>11. Hệ thống đóng WebSocket và hiển thị nút "View Results" |
| **Ngoại lệ** | - WebSocket connection error: hiển thị thông báo lỗi và retry<br>- Process failed: hiển thị step bị lỗi với thông tin chi tiết<br>- Network timeout: hiển thị thông báo và cho phép reconnect |

---

## Use Case: Hủy tiến trình generation

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Hủy tiến trình generation |
| **Actor** | User |
| **Tiền điều kiện** | User đang có process generation đang chạy |
| **Hậu điều kiện** | Process generation bị hủy và trạng thái cập nhật thành cancelled |
| **Kịch bản chính** | 1. User đang xem LiveProgress của tiến trình generation<br>2. User click nút "Cancel Process"<br>3. Hệ thống hiển thị dialog xác nhận hủy<br>4. User xác nhận muốn hủy<br>5. Hệ thống gửi yêu cầu cancel đến backend API `/gen/cancel/{processId}`<br>6. Backend cập nhật process status thành "cancelled"<br>7. Backend dừng generation pipeline<br>8. Backend trả về MessageResponse xác nhận đã hủy<br>9. Hệ thống nhận response và đóng WebSocket<br>10. Hệ thống cập nhật UI và hiển thị thông báo "Process cancelled successfully" |
| **Ngoại lệ** | - Process đã hoàn thành: hiển thị thông báo không thể hủy<br>- Lỗi cancel: hiển thị thông báo lỗi từ server |

---

## Use Case: Xem kết quả batch

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem kết quả batch |
| **Actor** | User |
| **Tiền điều kiện** | User có batch pending cần approval |
| **Hậu điều kiện** | User xem được danh sách item trong batch |
| **Kịch bản chính** | 1. Sau khi generation hoàn thành, hệ thống check pending batch<br>2. Hệ thống gọi API `/batch/pending-batch` với userId, sessionId, result_type<br>3. Backend trả về GenerationBatch với thông tin batch<br>4. Hệ thống gọi API `/batch/batch-item/by-batch/{batchId}` để lấy danh sách items<br>5. Backend trả về danh sách BatchItem với Question/Flashcard data<br>6. Hệ thống hiển thị QuestionApprovalTab hoặc FlashcardApprovalTab<br>7. User có thể chuyển đổi giữa layout "single" và "list"<br>8. Trong single layout, User xem từng item với navigation buttons<br>9. Trong list layout, User xem toàn bộ items với checkbox để select<br>10. User có thể edit item trực tiếp trong approval tab |
| **Ngoại lệ** | - Không có batch pending: không hiển thị approval tab<br>- Lỗi load batch: hiển thị thông báo lỗi |

---

## Use Case: Duyệt kết quả batch

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Duyệt kết quả batch |
| **Actor** | User |
| **Tiền điều kiện** | User đang xem kết quả batch trong approval tab |
| **Hậu điều kiện** | Các item được approve lưu vào Session và có thể sử dụng |
| **Kịch bản chính** | 1. User đang xem batch items trong approval tab<br>2. User chọn các item muốn approve bằng checkbox<br>3. User click nút "Approve Selected"<br>4. Hệ thống gửi yêu cầu approve đến backend `/batch/approve`<br>5. Backend nhận BatchApprovalRequest với batch_id và selected_item_ids<br>6. Backend thực hiện:<br>&nbsp;&nbsp;&nbsp;&nbsp;- Cập nhật status của batch items thành "approved"<br>&nbsp;&nbsp;&nbsp;&nbsp;- Insert Question/Flashcard vào Session<br>&nbsp;&nbsp;&nbsp;&nbsp;- Cập nhật batch approved_count<br>&nbsp;&nbsp;&nbsp;&nbsp;- Nếu tất cả items đã xử lý, cập nhật batch status<br>7. Backend trả về MessageResponse xác nhận<br>8. Hệ thống nhận response và cập nhật UI<br>9. Hệ thống reload tab data để hiển thị items mới trong Question/Flashcard tab<br>10. Hệ thống hiển thị thông báo "Items approved successfully"<br>11. Nếu còn items chưa xử lý, tiếp tục hiển thị approval tab, nếu không thì ẩn |
| **Ngoại lệ** | - Không có item nào được chọn: hiển thị thông báo<br>- Lỗi approve: hiển thị thông báo lỗi từ server |

---

## Use Case: Từ chối kết quả batch

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Từ chối kết quả batch |
| **Actor** | User |
| **Tiền điều kiện** | User đang xem kết quả batch trong approval tab |
| **Hậu điều kiện** | Các item bị reject với feedback và có thể regenerate |
| **Kịch bản chính** | 1. User đang xem batch items trong approval tab<br>2. User chọn các item muốn reject bằng checkbox<br>3. User nhập feedback text (tùy chọn, max 500 ký tự)<br>4. User click nút "Reject Selected"<br>5. Hệ thống gửi yêu cầu reject đến backend `/batch/reject`<br>6. Backend nhận BatchRejectionRequest với batch_id, selected_item_ids, và feedback<br>7. Backend thực hiện:<br>&nbsp;&nbsp;&nbsp;&nbsp;- Cập nhật status của batch items thành "rejected"<br>&nbsp;&nbsp;&nbsp;&nbsp;- Lưu feedback vào BatchFeedback với feedback_type="rejection"<br>&nbsp;&nbsp;&nbsp;&nbsp;- Cập nhật batch rejected_count<br>&nbsp;&nbsp;&nbsp;&nbsp;- Nếu tất cả items đã xử lý, cập nhật batch status<br>8. Backend trả về MessageResponse xác nhận<br>9. Hệ thống nhận response và cập nhật UI<br>10. Hệ thống hiển thị thông báo "Items rejected successfully"<br>11. Nếu còn items chưa xử lý, tiếp tục hiển thị approval tab<br>12. Nếu có rejected items, hiển thị nút "Regenerate" |
| **Ngoại lệ** | - Không có item nào được chọn: hiển thị thông báo<br>- Feedback quá dài: hiển thị lỗi validation<br>- Lỗi reject: hiển thị thông báo lỗi từ server |

---

## Use Case: Tạo yêu cầu regenerate

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Tạo yêu cầu regenerate |
| **Actor** | User |
| **Tiền điều kiện** | User có batch với rejected items và feedback |
| **Hậu điều kiện** | Yêu cầu regenerate được tạo và tiến trình generation mới bắt đầu |
| **Kịch bản chính** | 1. User đang xem batch với rejected items<br>2. User click nút "Regenerate"<br>3. Hệ thống gửi yêu cầu regenerate đến backend `/gen/regenerate`<br>4. Backend nhận BatchRegenerationRequest với batch_id và user_id<br>5. Backend thực hiện:<br>&nbsp;&nbsp;&nbsp;&nbsp;- Lấy thông tin batch cũ và rejected items<br>&nbsp;&nbsp;&nbsp;&nbsp;- Lấy feedback từ BatchFeedback<br>&nbsp;&nbsp;&nbsp;&nbsp;- Tạo GenerationProcess mới với process_order tăng lên<br>&nbsp;&nbsp;&nbsp;&nbsp;- Tạo GenerationBatch mới với parent_batch_id là batch cũ<br>&nbsp;&nbsp;&nbsp;&nbsp;- Build prompt với regeneration guidance từ feedback<br>6. Backend trả về GenerationBatch mới<br>7. Hệ thống nhận response và kết nối WebSocket với process mới<br>8. Hệ thống hiển thị LiveProgress cho tiến trình regenerate<br>9. Sau khi regenerate hoàn thành, hiển thị approval tab mới |
| **Ngoại lệ** | - Không có rejected items: không cho phép regenerate<br>- Lỗi regenerate: hiển thị thông báo lỗi từ server |

---

## Use Case: Kết nối lại với tiến trình đang chạy

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Kết nối lại với tiến trình đang chạy |
| **Actor** | User |
| **Tiền điều kiện** | User có process generation đang chạy và reload trang hoặc quay lại Session |
| **Hậu điều kiện** | WebSocket kết nối lại và User tiếp tục theo dõi tiến trình |
| **Kịch bản chính** | 1. User truy cập lại Session detail page<br>2. Hệ thống load session info và xác nhận session<br>3. Hệ thống gọi checkAndConnectRunningProcesses()<br>4. Hệ thống gọi API `/gen/user-process` với userId và result_type="question"<br>5. Backend kiểm tra xem có GenerationProcess nào đang active không<br>6. Nếu có process đang chạy, backend trả về GenerationProcessResponse<br>7. Hệ thống nhận response và kết nối WebSocket với process_id<br>8. Frontend gửi message "resume" với userId<br>9. Backend nhận message và gửi snapshot hiện tại<br>10. Hệ thống hiển thị LiveProgress với trạng thái hiện tại<br>11. Tiếp tục nhận state updates từ WebSocket<br>12. Lặp lại bước 4-11 cho result_type="flashcard" |
| **Ngoại lệ** | - Không có process đang chạy: không làm gì<br>- WebSocket connection error: hiển thị thông báo và retry<br>- Process đã hoàn thành: hiển thị approval tab nếu có batch pending |
