# Sequence Scenario - Quản lý Model Config và Model Wrapper

## Kịch bản 1: Xem danh sách Model Config

1. Sau khi đăng nhập, User click nút "Mô hình" tại sidebar
2. ModelConfigPage.tsx khởi tạo và hiển thị giao diện với tab "Model Configurations"
3. ModelConfigTab.tsx khởi tạo và gọi `loadModels()` để lấy danh sách Model Config
4. ModelConfigTab.tsx gửi request GET tới endpoint `/model-config`
5. Backend route `get_models()` nhận request và gọi `get_models_by_user()`
6. ModelConfigService truy vấn cơ sở dữ liệu trả danh sách ModelConfig objects
7. model_config_routes.py trả về danh sách ModelConfigResponse JSON
8. ModelConfigTab.tsx nhận danh sách và cập nhật state `models` với dữ liệu nhận được
9. Component re-render và hiển thị danh sách Model Config cho User

---

## Kịch bản 2: Tạo Model Config mới

1. Sau khi đăng nhập, User click nút "Mô hình" tại sidebar
2. ModelConfigPage.tsx khởi tạo và hiển thị giao diện
3. User click vào tab "Model Configurations"
4. ModelConfigTab.tsx khởi tạo và gọi `loadModels()` để lấy danh sách Model Config
5. ModelConfigTab.tsx gửi request GET tới endpoint `/model-config`
6. Backend route `get_models()` nhận request và gọi `get_models_by_user()`
7. ModelConfigService truy vấn cơ sở dữ liệu trả danh sách ModelConfig objects
8. model_config_routes.py trả về danh sách ModelConfigResponse JSON
9. ModelConfigTab.tsx nhận danh sách và hiển thị danh sách Model Config theo task type
10. User chọn task type (ví dụ: GENERATION)
11. ModelConfigTab.tsx lọc và hiển thị danh sách Model Config cho task type đó
12. User click nút "Add Model"
13. ModelConfigTab.tsx mở ModelConfigCreateDialog và gọi `loadWrappers()`
14. ModelConfigCreateDialog gửi request GET tới endpoint `/model-config/wrapper?task_type=GENERATION`
15. Backend route `get_wrappers()` nhận request và gọi `get_wrappers()`
16. ModelConfigService truy vấn cơ sở dữ liệu trả danh sách ModelWrapper objects theo task type
17. model_config_routes.py trả về danh sách ModelWrapperResponse JSON
18. ModelConfigCreateDialog hiển thị bảng ModelWrapperTable với danh sách Wrapper
19. User chọn một Wrapper từ bảng (click Switch)
20. ModelConfigCreateDialog cập nhật selectedWrapperId
21. User nhập các thông tin: name, model_type, lang, extra_config
22. User click nút "Save"
23. ModelConfigCreateDialog validate form và gọi `handleSave()`
24. ModelConfigTab.tsx gửi POST request tới `/model-config` endpoint với dữ liệu
25. Backend route `create_model()` nhận request và gọi `create_model()`
26. ModelConfigService tạo ModelConfig mới và lưu vào database
27. ModelConfigService trả về ModelConfig object
28. model_config_routes.py trả về ModelConfigResponse JSON
29. ModelConfigTab.tsx cập nhật danh sách với model mới
30. Component re-render và hiển thị danh sách Model Config được cập nhật
31. Toast success notification được hiển thị: "Model created successfully"

---

## Kịch bản 3: Chỉnh sửa Model Config

1. Sau khi đăng nhập, User click nút "Mô hình" tại sidebar
2. ModelConfigPage.tsx khởi tạo và hiển thị giao diện
3. User click vào tab "Model Configurations"
4. ModelConfigTab.tsx khởi tạo và gọi `loadModels()` để lấy danh sách Model Config
5. ModelConfigTab.tsx gửi request GET tới endpoint `/model-config`
6. Backend route `get_models()` nhận request và gọi `get_models_by_user()`
7. ModelConfigService truy vấn cơ sở dữ liệu trả danh sách ModelConfig objects
8. model_config_routes.py trả về danh sách ModelConfigResponse JSON
9. ModelConfigTab.tsx nhận danh sách và hiển thị danh sách Model Config
10. User chọn task type và xem danh sách Model Config
11. ModelConfigTab.tsx lọc và hiển thị models theo task type
12. User click nút "Edit" trên Model Config muốn chỉnh sửa
13. ModelConfigTab.tsx mở ModelConfigUpdateDialog với dữ liệu hiện tại và gọi `loadWrappers()`
14. ModelConfigUpdateDialog gửi request GET tới endpoint `/model-config/wrapper?task_type=GENERATION`
15. Backend route `get_wrappers()` nhận request và gọi `get_wrappers()`
16. ModelConfigService truy vấn cơ sở dữ liệu trả danh sách ModelWrapper objects theo task type
17. model_config_routes.py trả về danh sách ModelWrapperResponse JSON
18. ModelConfigUpdateDialog hiển thị bảng ModelWrapperTable với Wrapper hiện tại được chọn
19. User có thể thay đổi Wrapper từ bảng
20. User cập nhật các thông tin: name, wrapper_id, model_type, lang, extra_config
21. User click nút "Save"
22. ModelConfigUpdateDialog validate form và gọi `handleSave()`
23. ModelConfigTab.tsx gửi PUT request tới `/model-config/{model_id}` endpoint
24. Backend route `update_model()` nhận request và gọi `update_model()`
25. ModelConfigService lấy ModelConfig hiện tại từ database, cập nhật các field và commit transaction
26. ModelConfigService trả về ModelConfig đã cập nhật
27. model_config_routes.py trả về ModelConfigResponse JSON
28. ModelConfigTab.tsx cập nhật item trong danh sách
29. Component re-render và hiển thị danh sách Model Config được cập nhật
30. Toast success notification được hiển thị: "Model updated successfully"

---

## Kịch bản 4: Xóa Model Config

1. Sau khi đăng nhập, User click nút "Mô hình" tại sidebar
2. ModelConfigPage.tsx khởi tạo và hiển thị giao diện
3. User click vào tab "Model Configurations"
4. ModelConfigTab.tsx khởi tạo và gọi `loadModels()` để lấy danh sách Model Config
5. ModelConfigTab.tsx gửi request GET tới endpoint `/model-config`
6. Backend route `get_models()` nhận request và gọi `get_models_by_user()`
7. ModelConfigService truy vấn cơ sở dữ liệu trả danh sách ModelConfig objects
8. model_config_routes.py trả về danh sách ModelConfigResponse JSON
9. ModelConfigTab.tsx nhận danh sách và hiển thị danh sách Model Config
10. User chọn task type và xem danh sách Model Config
11. ModelConfigTab.tsx lọc và hiển thị models theo task type
12. User click nút "Delete" trên Model Config muốn xóa
13. ModelConfigTab.tsx mở ModelConfigDeleteDialog
14. ModelConfigDeleteDialog hiển thị xác nhận xóa với tên Model Config
15. User click nút "Delete" để xác nhận
16. ModelConfigDeleteDialog gọi `handleConfirm()`
17. ModelConfigTab.tsx gửi DELETE request tới `/model-config/{model_id}` endpoint
18. Backend route `delete_model()` nhận request và gọi `delete_model()`
19. ModelConfigService xóa ModelConfig khỏi database
20. model_config_routes.py trả về MessageResponse JSON
21. ModelConfigTab.tsx xóa item khỏi danh sách
22. Component re-render, danh sách Model Config được cập nhật
23. Toast success notification được hiển thị: "Model deleted successfully"

---

## Kịch bản 5: Toggle trạng thái sử dụng Model Config

1. Sau khi đăng nhập, User click nút "Mô hình" tại sidebar
2. ModelConfigPage.tsx khởi tạo và hiển thị giao diện
3. User click vào tab "Model Configurations"
4. ModelConfigTab.tsx khởi tạo và gọi `loadModels()` để lấy danh sách Model Config
5. ModelConfigTab.tsx gửi request GET tới endpoint `/model-config`
6. Backend route `get_models()` nhận request và gọi `get_models_by_user()`
7. ModelConfigService truy vấn cơ sở dữ liệu trả danh sách ModelConfig objects
8. model_config_routes.py trả về danh sách ModelConfigResponse JSON
9. ModelConfigTab.tsx nhận danh sách và hiển thị danh sách Model Config
10. User chọn task type và xem danh sách Model Config
11. ModelConfigTab.tsx lọc và hiển thị models theo task type
12. User toggle Switch "Is Used" của một Model Config
13. ModelConfigTab.tsx gọi `handleToggleActive()`
14. ModelConfigTab.tsx gửi PUT request tới `/model-config/{model_id}` endpoint với `is_used`
15. Backend route `update_model()` nhận request và gọi `update_model()`
16. ModelConfigService cập nhật is_used của ModelConfig và tự động set is_used=false cho các ModelConfig khác cùng task type và user
17. ModelConfigService trả về ModelConfig đã cập nhật
18. model_config_routes.py trả về ModelConfigResponse JSON
19. ModelConfigTab.tsx cập nhật danh sách models
20. Component re-render và hiển thị trạng thái mới
21. Toast success notification được hiển thị: "Model status updated successfully"

---

## Kịch bản 6: Preload Model Config

1. Sau khi đăng nhập, User click nút "Mô hình" tại sidebar
2. ModelConfigPage.tsx khởi tạo và hiển thị giao diện
3. User click vào tab "Model Configurations"
4. ModelConfigTab.tsx khởi tạo và gọi `loadModels()` để lấy danh sách Model Config
5. ModelConfigTab.tsx gửi request GET tới endpoint `/model-config`
6. Backend route `get_models()` nhận request và gọi `get_models_by_user()`
7. ModelConfigService truy vấn cơ sở dữ liệu trả danh sách ModelConfig objects
8. model_config_routes.py trả về danh sách ModelConfigResponse JSON
9. ModelConfigTab.tsx nhận danh sách và hiển thị danh sách Model Config (có thể rỗng)
10. User chọn task type
11. ModelConfigTab.tsx lọc và hiển thị models theo task type
12. User click nút "Preload Models"
13. ModelConfigTab.tsx hiển thị confirm dialog
14. User xác nhận preload
15. ModelConfigTab.tsx gửi POST request tới `/model-config/preload/{user_id}` endpoint
16. Backend route `preload_models()` nhận request và gọi `preload_models_for_user()`
17. ModelConfigService tạo các ModelConfig mặc định từ template theo task type và lưu các ModelConfig vào database
18. ModelConfigService trả về danh sách ModelConfig objects
19. model_config_routes.py trả về danh sách ModelConfigResponse JSON
20. ModelConfigTab.tsx cập nhật danh sách với items mới
21. Component re-render và hiển thị danh sách Model Config được cập nhật
22. Toast success notification được hiển thị: "Models preloaded successfully"

---

## Kịch bản 7: Xem danh sách Model Wrapper

1. Sau khi đăng nhập, User click nút "Mô hình" tại sidebar
2. ModelConfigPage.tsx khởi tạo và hiển thị giao diện
3. User click vào tab "Model Wrappers"
4. ModelWrapperTab.tsx khởi tạo và gọi `loadWrappers()` để lấy danh sách Model Wrapper
5. ModelWrapperTab.tsx gửi request GET tới endpoint `/model-config/wrapper`
6. Backend route `get_wrappers()` nhận request và gọi `get_wrappers()`
7. ModelConfigService truy vấn cơ sở dữ liệu trả danh sách ModelWrapper objects
8. model_config_routes.py trả về danh sách ModelWrapperResponse JSON
9. ModelWrapperTab.tsx nhận danh sách và cập nhật state `wrappers` với dữ liệu nhận được
10. Component re-render và hiển thị danh sách Model Wrapper cho User

---

## Kịch bản 8: Tạo Model Wrapper mới (Admin only)

1. Sau khi đăng nhập, Admin click nút "Mô hình" tại sidebar
2. ModelConfigPage.tsx khởi tạo và hiển thị giao diện
3. Admin click vào tab "Model Wrappers"
4. ModelWrapperTab.tsx khởi tạo và gọi `loadWrappers()` để lấy danh sách Model Wrapper
5. ModelWrapperTab.tsx gửi request GET tới endpoint `/model-config/wrapper`
6. Backend route `get_wrappers()` nhận request và gọi `get_wrappers()`
7. ModelConfigService truy vấn cơ sở dữ liệu trả danh sách ModelWrapper objects
8. model_config_routes.py trả về danh sách ModelWrapperResponse JSON
9. ModelWrapperTab.tsx nhận danh sách và hiển thị danh sách Wrapper trong ModelWrapperTable
10. Admin click nút "Add Wrapper"
11. ModelWrapperTab.tsx mở ModelWrapperCreateDialog
12. ModelWrapperCreateDialog hiển thị form trống
13. Admin nhập các thông tin: name, task_type, instruction
14. Admin click nút "Save"
15. ModelWrapperCreateDialog validate form và gọi `handleSave()`
16. ModelWrapperTab.tsx gửi POST request tới `/model-config/wrapper` endpoint
17. Backend route `create_wrapper()` nhận request và gọi `create_wrapper()`
18. ModelConfigService tạo ModelWrapper mới và lưu vào database
19. ModelConfigService trả về ModelWrapper object
20. model_config_routes.py trả về ModelWrapperResponse JSON
21. ModelWrapperTab.tsx cập nhật danh sách với wrapper mới
22. Component re-render và hiển thị danh sách Model Wrapper được cập nhật
23. Toast success notification được hiển thị: "Wrapper created successfully"

---

## Kịch bản 9: Chỉnh sửa Model Wrapper (Admin only)

1. Sau khi đăng nhập, Admin click nút "Mô hình" tại sidebar
2. ModelConfigPage.tsx khởi tạo và hiển thị giao diện
3. Admin click vào tab "Model Wrappers"
4. ModelWrapperTab.tsx khởi tạo và gọi `loadWrappers()` để lấy danh sách Model Wrapper
5. ModelWrapperTab.tsx gửi request GET tới endpoint `/model-config/wrapper`
6. Backend route `get_wrappers()` nhận request và gọi `get_wrappers()`
7. ModelConfigService truy vấn cơ sở dữ liệu trả danh sách ModelWrapper objects
8. model_config_routes.py trả về danh sách ModelWrapperResponse JSON
9. ModelWrapperTab.tsx nhận danh sách và hiển thị danh sách Wrapper
10. Admin click nút "Edit" trên Wrapper muốn chỉnh sửa
11. ModelWrapperTab.tsx mở ModelWrapperUpdateDialog với dữ liệu hiện tại
12. ModelWrapperUpdateDialog hiển thị form với thông tin Wrapper
13. Admin cập nhật các thông tin: name, task_type, instruction
14. Admin click nút "Save"
15. ModelWrapperUpdateDialog validate form và gọi `handleSave()`
16. ModelWrapperTab.tsx gửi PUT request tới `/model-config/wrapper/{wrapper_id}` endpoint
17. Backend route `update_wrapper()` nhận request và gọi `update_wrapper()`
18. ModelConfigService lấy ModelWrapper hiện tại từ database, cập nhật các field và commit transaction
19. ModelConfigService trả về ModelWrapper đã cập nhật
20. model_config_routes.py trả về ModelWrapperResponse JSON
21. ModelWrapperTab.tsx cập nhật item trong danh sách
22. Component re-render và hiển thị danh sách Model Wrapper được cập nhật
23. Toast success notification được hiển thị: "Wrapper updated successfully"

---

## Kịch bản 10: Xóa Model Wrapper (Admin only)

1. Sau khi đăng nhập, Admin click nút "Mô hình" tại sidebar
2. ModelConfigPage.tsx khởi tạo và hiển thị giao diện
3. Admin click vào tab "Model Wrappers"
4. ModelWrapperTab.tsx khởi tạo và gọi `loadWrappers()` để lấy danh sách Model Wrapper
5. ModelWrapperTab.tsx gửi request GET tới endpoint `/model-config/wrapper`
6. Backend route `get_wrappers()` nhận request và gọi `get_wrappers()`
7. ModelConfigService truy vấn cơ sở dữ liệu trả danh sách ModelWrapper objects
8. model_config_routes.py trả về danh sách ModelWrapperResponse JSON
9. ModelWrapperTab.tsx nhận danh sách và hiển thị danh sách Wrapper
10. Admin click nút "Delete" trên Wrapper muốn xóa
11. ModelWrapperTab.tsx mở ModelWrapperDeleteDialog
12. ModelWrapperDeleteDialog hiển thị xác nhận xóa với tên Wrapper
13. Admin click nút "Delete" để xác nhận
14. ModelWrapperDeleteDialog gọi `handleConfirm()`
15. ModelWrapperTab.tsx gửi DELETE request tới `/model-config/wrapper/{wrapper_id}` endpoint
16. Backend route `delete_wrapper()` nhận request và gọi `delete_wrapper()`
17. ModelConfigService xóa ModelWrapper và các ModelConfig liên quan (cascade) khỏi database
18. model_config_routes.py trả về MessageResponse JSON
19. ModelWrapperTab.tsx xóa item khỏi danh sách
20. Component re-render, danh sách Model Wrapper được cập nhật
21. Toast success notification được hiển thị: "Wrapper deleted successfully"
