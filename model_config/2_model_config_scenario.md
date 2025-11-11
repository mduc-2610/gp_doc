# Scenario - Cấu hình Model

## Use Case: Xem danh sách Model

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem danh sách Model |
| **Actor** | User |
| **Tiền điều kiện** | User đã đăng nhập thành công |
| **Hậu điều kiện** | Danh sách các Model của User được hiển thị trên giao diện, phân loại theo loại tác vụ |
| **Kịch bản chính** | 1. Tại giao diện chính, User click vào tab "Model Config" hoặc tương tự<br>2. Giao diện mặc định hiển thị tab "Models"<br>3. Hệ thống load và hiển thị danh sách Model phân loại theo Task Type (Generation, Embedding, Parse Doc, Parse Audio, Parse Image) |
| **Ngoại lệ** | - User không có Model nào: hiển thị thông báo "Không có Model nào"<br>- Lỗi load Model: hiển thị thông báo lỗi từ server |

---

## Use Case: Tạo Model mới

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Tạo Model mới |
| **Actor** | User |
| **Tiền điều kiện** | User đã đăng nhập thành công và đang ở giao diện danh sách Model |
| **Hậu điều kiện** | Model mới được tạo và thêm vào danh sách |
| **Kịch bản chính** | 1. Tại giao diện danh sách Model, User click nút "Tạo Model mới"<br>2. Hệ thống mở dialog tạo Model<br>3. Hệ thống load và hiển thị danh sách Model Wrapper<br>4. User nhập các thông tin và click nút "Tạo":<br>&nbsp;&nbsp;&nbsp;&nbsp;- Tên Model<br>&nbsp;&nbsp;&nbsp;&nbsp;- Model Wrapper (Chọn)<br>&nbsp;&nbsp;&nbsp;&nbsp;- Model Type<br>&nbsp;&nbsp;&nbsp;&nbsp;- API Key<br>&nbsp;&nbsp;&nbsp;&nbsp;- Extra Config<br>5. Hệ thống validate dữ liệu<br>6. Hệ thống gửi yêu cầu tạo Model đến backend<br>7. Backend tạo Model và trả về thông tin Model mới<br>8. Hệ thống đóng dialog, cập nhật danh sách và hiển thị thông báo "Tạo Model thành công" |
| **Ngoại lệ** | - Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Lỗi tạo Model: hiển thị thông báo lỗi từ server |

---

## Use Case: Chỉnh sửa Model

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Chỉnh sửa Model |
| **Actor** | User |
| **Tiền điều kiện** | User đã đăng nhập thành công và đang ở giao diện danh sách Model |
| **Hậu điều kiện** | Thông tin Model được cập nhật trong hệ thống |
| **Kịch bản chính** | 1. Tại giao diện danh sách Model, User click icon chỉnh sửa trên một Model<br>2. Hệ thống mở dialog chỉnh sửa với thông tin hiện tại<br>3. Hệ thống load và hiển thị danh sách Model Wrapper<br>4. User cập nhật các thông tin và click nút "Lưu":<br>&nbsp;&nbsp;&nbsp;&nbsp;- Tên<br>&nbsp;&nbsp;&nbsp;&nbsp;- Model Wrapper (Chọn)<br>&nbsp;&nbsp;&nbsp;&nbsp;- Model Type<br>&nbsp;&nbsp;&nbsp;&nbsp;- API Key<br>&nbsp;&nbsp;&nbsp;&nbsp;- Extra Config<br>5. Hệ thống validate dữ liệu<br>6. Hệ thống gửi yêu cầu cập nhật đến backend<br>7. Backend cập nhật Model trong cơ sở dữ liệu<br>8. Hệ thống đóng dialog, cập nhật danh sách và hiển thị thông báo "Cập nhật Model thành công" |
| **Ngoại lệ** | - Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Model không tồn tại: hiển thị thông báo lỗi<br>- Lỗi cập nhật: hiển thị thông báo lỗi từ server |

---

## Use Case: Xóa Model

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xóa Model |
| **Actor** | User |
| **Tiền điều kiện** | User đã đăng nhập thành công và đang ở giao diện danh sách Model |
| **Hậu điều kiện** | Model được xóa khỏi hệ thống và danh sách được cập nhật |
| **Kịch bản chính** | 1. Tại giao diện danh sách Model, User click icon xóa trên một Model<br>2. Hệ thống hiển thị dialog xác nhận xóa<br>3. User xác nhận muốn xóa<br>4. Hệ thống gửi yêu cầu xóa đến backend<br>5. Backend xóa Model khỏi cơ sở dữ liệu<br>6. Hệ thống loại bỏ item khỏi danh sách và hiển thị thông báo "Xóa Model thành công" |
| **Ngoại lệ** | - Model không tồn tại: hiển thị thông báo lỗi<br>- Lỗi xóa: hiển thị thông báo lỗi từ server |

---

## Use Case: Xem danh sách Model Wrapper

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xem danh sách Model Wrapper |
| **Actor** | User, Admin |
| **Tiền điều kiện** | User hoặc Admin đã đăng nhập thành công |
| **Hậu điều kiện** | Danh sách các Model Wrapper được hiển thị trên giao diện |
| **Kịch bản chính** | 1. Tại giao diện Model Config, User hoặc Admin click tab "Model Wrappers"<br>2. Hệ thống load và hiển thị danh sách tất cả các Model Wrapper<br>3. Nếu là Admin, hiển thị thêm các nút tạo, chỉnh sửa, xóa Wrapper |
| **Ngoại lệ** | - Không có Model Wrapper: hiển thị thông báo "Không có Model Wrapper nào"<br>- Lỗi load: hiển thị thông báo lỗi từ server |

---

## Use Case: Tạo Model Wrapper mới

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Tạo Model Wrapper mới |
| **Actor** | Admin |
| **Tiền điều kiện** | Admin đã đăng nhập thành công và đang ở giao diện danh sách Model Wrapper |
| **Hậu điều kiện** | Model Wrapper mới được tạo và thêm vào danh sách |
| **Kịch bản chính** | 1. Tại giao diện danh sách Model Wrapper, Admin click nút "Tạo Model Wrapper mới"<br>2. Hệ thống mở dialog tạo Wrapper<br>3. Admin nhập các thông tin và click nút "Tạo":<br>&nbsp;&nbsp;&nbsp;&nbsp;- Tên<br>&nbsp;&nbsp;&nbsp;&nbsp;- Task Type<br>&nbsp;&nbsp;&nbsp;&nbsp;- Instruction<br>4. Hệ thống validate dữ liệu<br>5. Hệ thống gửi yêu cầu tạo Wrapper đến backend<br>6. Backend tạo Wrapper và trả về thông tin mới<br>7. Hệ thống đóng dialog, cập nhật danh sách và hiển thị thông báo "Tạo Model Wrapper thành công" |
| **Ngoại lệ** | - Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Wrapper name trùng: hiển thị lỗi từ server<br>- Lỗi tạo: hiển thị thông báo lỗi từ server |

---

## Use Case: Chỉnh sửa Model Wrapper

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Chỉnh sửa Model Wrapper |
| **Actor** | Admin |
| **Tiền điều kiện** | Admin đã đăng nhập thành công và đang ở giao diện danh sách Model Wrapper |
| **Hậu điều kiện** | Thông tin Model Wrapper được cập nhật trong hệ thống |
| **Kịch bản chính** | 1. Tại giao diện danh sách Model Wrapper, Admin click icon chỉnh sửa trên một Wrapper<br>2. Hệ thống mở dialog chỉnh sửa với thông tin hiện tại<br>3. Admin cập nhật các thông tin và click nút "Lưu":<br>&nbsp;&nbsp;&nbsp;&nbsp;- Tên<br>&nbsp;&nbsp;&nbsp;&nbsp;- Task Type<br>&nbsp;&nbsp;&nbsp;&nbsp;- Instruction<br>4. Hệ thống validate dữ liệu<br>5. Hệ thống gửi yêu cầu cập nhật đến backend<br>6. Backend cập nhật Wrapper trong cơ sở dữ liệu<br>7. Hệ thống đóng dialog, cập nhật danh sách và hiển thị thông báo "Cập nhật Model Wrapper thành công" |
| **Ngoại lệ** | - Dữ liệu không hợp lệ: hiển thị lỗi validation<br>- Wrapper không tồn tại: hiển thị thông báo lỗi<br>- Lỗi cập nhật: hiển thị thông báo lỗi từ server |

---

## Use Case: Xóa Model Wrapper

| Thành phần | Chi tiết |
|-----------|----------|
| **Use Case** | Xóa Model Wrapper |
| **Actor** | Admin |
| **Tiền điều kiện** | Admin đã đăng nhập thành công và đang ở giao diện danh sách Model Wrapper |
| **Hậu điều kiện** | Model Wrapper được xóa khỏi hệ thống và danh sách được cập nhật |
| **Kịch bản chính** | 1. Tại giao diện danh sách Model Wrapper, Admin click icon xóa trên một Wrapper<br>2. Hệ thống hiển thị dialog xác nhận xóa<br>3. Admin xác nhận muốn xóa<br>4. Hệ thống gửi yêu cầu xóa đến backend<br>5. Backend xóa Wrapper khỏi cơ sở dữ liệu<br>6. Hệ thống loại bỏ item khỏi danh sách và hiển thị thông báo "Xóa Model Wrapper thành công" |
| **Ngoại lệ** | - Wrapper không tồn tại: hiển thị thông báo lỗi<br>- Wrapper đang được sử dụng: hiển thị thông báo lỗi<br>- Lỗi xóa: hiển thị thông báo lỗi từ server |
