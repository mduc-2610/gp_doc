# Scenario - Quản lý Document

## Use Case: Upload Document

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Upload Document |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang ở giao diện Session chi tiết |
| **Hậu điều kiện** | - Document mới được upload và xử lý<br>- Document được thêm vào danh sách |
| **Kịch bản chính** | 1. Tại giao diện Session chi tiết, User xem danh sách Document trong sidebar<br>2. User click icon plus để thêm Document mới<br>3. Hệ thống mở dialog upload với 2 tab: "Files" và "URLs"<br>4. **Nếu upload File:**<br>&nbsp;&nbsp;&nbsp;&nbsp;a. User chọn tab "Files" và chọn một hoặc nhiều file<br>&nbsp;&nbsp;&nbsp;&nbsp;b. Hệ thống validate kích thước file và định dạng<br>&nbsp;&nbsp;&nbsp;&nbsp;c. File được thêm vào danh sách upload<br>&nbsp;&nbsp;&nbsp;&nbsp;**Nếu upload URL:**<br>&nbsp;&nbsp;&nbsp;&nbsp;a. User chọn tab "URLs" và nhập URL<br>&nbsp;&nbsp;&nbsp;&nbsp;b. User click "Add URL"<br>&nbsp;&nbsp;&nbsp;&nbsp;c. Hệ thống validate URL hợp lệ và số lượng<br>&nbsp;&nbsp;&nbsp;&nbsp;d. URL được thêm vào danh sách upload<br>5. User click "Upload All"<br>6. Hệ thống gửi yêu cầu upload từng item đến backend<br>7. **Backend xử lý cho mỗi Document:**<br>&nbsp;&nbsp;&nbsp;&nbsp;a. Lấy Parse Model Config từ ModelConfigService theo task type<br>&nbsp;&nbsp;&nbsp;&nbsp;b. Tạo Parser phù hợp từ ParserFactory (Doc/Audio/Youtube/Web/Image)<br>&nbsp;&nbsp;&nbsp;&nbsp;c. Lưu file (nếu là upload) và tạo Document record trong database<br>&nbsp;&nbsp;&nbsp;&nbsp;d. Parse Document bằng Parser để extract text<br>&nbsp;&nbsp;&nbsp;&nbsp;e. Lấy Embedding Model Config từ ModelConfigService<br>&nbsp;&nbsp;&nbsp;&nbsp;f. Tạo Embedding Model từ EmbeddingFactory (FastEmbed/OpenAI)<br>&nbsp;&nbsp;&nbsp;&nbsp;g. Tạo Text Splitter từ TextSplitterFactory (Recursive/Semantic)<br>&nbsp;&nbsp;&nbsp;&nbsp;h. Split text thành chunks bằng Splitter<br>&nbsp;&nbsp;&nbsp;&nbsp;i. Generate embedding cho từng chunk và lưu vào DocumentChunk<br>8. Backend trả về thông tin Document đã xử lý<br>9. Hệ thống cập nhật danh sách Document và hiển thị thông báo "Upload thành công" |
| **Ngoại lệ** | - File vượt quá kích thước cho phép: hiển thị lỗi validation<br>- Định dạng file không được hỗ trợ: hiển thị lỗi validation<br>- URL không hợp lệ: hiển thị lỗi validation<br>- Lỗi parse: cập nhật processing_status = FAILED và hiển thị lỗi<br>- Lỗi embedding: hiển thị thông báo lỗi<br>- Lỗi upload: hiển thị thông báo lỗi từ server |

---

## Use Case: Xem danh sách Document

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem danh sách Document |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang ở giao diện Session chi tiết |
| **Hậu điều kiện** | - Danh sách Document được hiển thị trên giao diện |
| **Kịch bản chính** | 1. Tại giao diện Session chi tiết, User xem danh sách Document trong sidebar<br>2. Hệ thống gửi yêu cầu lấy danh sách Document đến backend<br>3. Backend trả về danh sách Document<br>4. Hệ thống hiển thị danh sách Document<br>5. User có thể tìm kiếm Document bằng tên hoặc click vào Document để xem chi tiết |
| **Ngoại lệ** | - Không có Document: hiển thị thông báo trống<br>- Lỗi load Document: hiển thị thông báo lỗi |

---

## Use Case: Xem thông tin Document

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem thông tin Document |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang ở giao diện Session chi tiết<br>- Session có ít nhất một Document |
| **Hậu điều kiện** | - Thông tin chi tiết Document được hiển thị |
| **Kịch bản chính** | 1. Tại giao diện Session chi tiết, User xem danh sách Document trong sidebar<br>2. User click vào một Document trong danh sách<br>3. Hệ thống cập nhật lựa chọn Document và hiển thị thông tin chi tiết:<br>&nbsp;&nbsp;&nbsp;&nbsp;- Tên Document<br>&nbsp;&nbsp;&nbsp;&nbsp;- Loại file<br>&nbsp;&nbsp;&nbsp;&nbsp;- Kích thước<br>&nbsp;&nbsp;&nbsp;&nbsp;- Nguồn<br>&nbsp;&nbsp;&nbsp;&nbsp;- Trạng thái xử lý<br>&nbsp;&nbsp;&nbsp;&nbsp;- Ngày tạo<br>4. User có thể click nút để xem file gốc hoặc nội dung đã xử lý |
| **Ngoại lệ** | - Document không tồn tại: hiển thị thông báo lỗi<br>- Lỗi load thông tin: hiển thị thông báo lỗi |

---

## Use Case: Cập nhật Document

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Cập nhật Document |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang ở giao diện Session chi tiết<br>- User có quyền chỉnh sửa Session |
| **Hậu điều kiện** | - Thông tin Document được cập nhật trong hệ thống |
| **Kịch bản chính** | 1. Tại giao diện Session chi tiết, User xem danh sách Document trong sidebar<br>2. User click icon menu trên một Document cần cập nhật và chọn "Đổi tên"<br>3. Hệ thống mở dialog cho phép cập nhật thông tin Document<br>4. User nhập tên mới cho Document và click nút "Lưu"<br>5. Hệ thống validate dữ liệu<br>6. Hệ thống gửi yêu cầu cập nhật đến backend<br>7. Backend cập nhật Document trong cơ sở dữ liệu và trả về thông tin Document mới<br>8. Hệ thống cập nhật dữ liệu trong danh sách và hiển thị thông báo "Cập nhật Document thành công" |
| **Ngoại lệ** | - Tên Document không hợp lệ: hiển thị lỗi validation<br>- Document không tồn tại: hiển thị thông báo lỗi<br>- Không có quyền chỉnh sửa: không hiển thị tùy chọn cập nhật<br>- Lỗi cập nhật: hiển thị thông báo lỗi từ server |

---

## Use Case: Xóa Document

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xóa Document |
| **Actor** | User |
| **Tiền điều kiện** | - User đã đăng nhập thành công<br>- User đang ở giao diện Session chi tiết<br>- User có quyền chỉnh sửa Session |
| **Hậu điều kiện** | - Document được xóa khỏi hệ thống<br>- Danh sách Document được cập nhật |
| **Kịch bản chính** | 1. Tại giao diện Session chi tiết, User xem danh sách Document trong sidebar<br>2. User click icon menu trên một Document cần xóa và chọn "Xóa"<br>3. Hệ thống hiển thị dialog xác nhận xóa Document<br>4. User xác nhận muốn xóa<br>5. Hệ thống gửi yêu cầu xóa đến backend<br>6. Backend xóa Document khỏi cơ sở dữ liệu và trả về trạng thái thành công<br>7. Hệ thống loại bỏ Document khỏi danh sách và hiển thị thông báo "Xóa Document thành công" |
| **Ngoại lệ** | - Document không tồn tại: hiển thị thông báo lỗi<br>- Không có quyền xóa: không hiển thị tùy chọn xóa<br>- Lỗi xóa: hiển thị thông báo lỗi từ server |
