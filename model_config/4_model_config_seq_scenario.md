# Sequence Scenario - Cấu hình Model

## Kịch bản 1: Xem danh sách Model

1. Sau khi đăng nhập, User truy cập tab "Models" trong trang Model Config
2. ModelConfigTab.tsx khởi tạo và gọi `ModelConfigService.getModels()` để lấy danh sách Model
3. ModelConfigService gửi request GET tới endpoint `/model-config`
4. Backend route nhận request và ModelConfigService được gọi hàm `get_models_by_user()`
5. ModelConfigService truy vấn cơ sở dữ liệu để lấy các Model của User
6. ModelConfigService convert Model objects thành ModelConfigResponse objects (masked API key)
7. model_config_routes.py trả về danh sách ModelConfigResponse JSON
8. ModelConfigService (frontend) nhận danh sách từ response và đóng gói với ServiceResult<ModelConfig[]>
9. ModelConfigTab.tsx cập nhật state `models` với dữ liệu nhận được
10. Component re-render và hiển thị danh sách Model phân loại theo Task Type cho User

---

## Kịch bản 2: Tạo Model mới

1. Tại giao diện danh sách Model, User click nút "Tạo Model mới"
2. ModelConfigTab.tsx gọi `handleCreate()`
3. ModelConfigTab.tsx gọi ModelConfigCreateDialog.tsx
4. ModelConfigCreateDialog.tsx gọi `ModelConfigService.getWrappers()` để lấy danh sách Wrapper
5. ModelConfigService gửi request GET tới endpoint `/model-config/wrapper`
6. model_config_routes.py nhận request và gọi `get_wrappers()`
7. ModelConfigService truy vấn cơ sở dữ liệu để lấy tất cả Wrapper
8. model_config_routes.py trả về danh sách ModelWrapperResponse JSON
9. ModelConfigService (frontend) nhận danh sách và đóng gói với ServiceResult<ModelWrapper[]>
10. ModelConfigCreateDialog.tsx hiển thị danh sách Wrapper và form cho User
11. User chọn một Wrapper, nhập tên Model, chọn Model Type, nhập API Key, Extra Config và click nút "Tạo"
12. ModelConfigCreateDialog.tsx validate form data
13. ModelConfigCreateDialog.tsx gọi `ModelConfigService.createModel()`
14. ModelConfigService gửi POST request tới `/model-config` endpoint
15. model_config_routes.py nhận request và gọi `create_model()`
16. ModelConfigService tạo ModelConfig model mới với các field: user_id, name, wrapper_id, model_type, api_key (encrypted), extra_config, lang
17. ModelConfigService thêm model vào database (db.add, db.commit)
18. ModelConfigService convert ModelConfig thành ModelConfigResponse (masked API key)
19. model_config_routes.py trả về ModelConfigResponse JSON
20. ModelConfigService (frontend) nhận response và đóng gói với ServiceResult<ModelConfig>
21. ModelConfigCreateDialog.tsx cập nhật danh sách Model bằng dữ liệu mới từ response
22. Component re-render và hiển thị danh sách Model được cập nhật
23. Toast success notification được hiển thị: "Tạo Model thành công"

---

## Kịch bản 3: Chỉnh sửa Model

1. Tại giao diện danh sách Model, User click nút "Chỉnh sửa" (icon edit) trên một Model
2. ModelConfigTab.tsx gọi `handleEdit(model)`
3. ModelConfigTab.tsx gọi ModelConfigUpdateDialog.tsx với dữ liệu model=selected
4. ModelConfigUpdateDialog.tsx gọi `ModelConfigService.getWrappers()` để lấy danh sách Wrapper
5. ModelConfigService gửi request GET tới endpoint `/model-config/wrapper`
6. model_config_routes.py nhận request và gọi `get_wrappers()`
7. ModelConfigService truy vấn cơ sở dữ liệu để lấy tất cả Wrapper
8. model_config_routes.py trả về danh sách ModelWrapperResponse JSON
9. ModelConfigService (frontend) nhận danh sách và đóng gói với ServiceResult<ModelWrapper[]>
10. ModelConfigUpdateDialog.tsx populate form với thông tin Model hiện tại và hiển thị danh sách Wrapper
11. User có thể chọn lại Wrapper, cập nhật tên Model, Model Type, API Key, Extra Config và click nút "Lưu"
12. ModelConfigUpdateDialog.tsx validate form data
13. ModelConfigUpdateDialog.tsx gọi `ModelConfigService.updateModel()`
14. ModelConfigService gửi PUT request tới `/model-config/{modelId}` endpoint
15. model_config_routes.py nhận request và gọi `update_model()`
16. ModelConfigService tìm Model theo modelId
17. ModelConfigService cập nhật các field có trong updateData
18. ModelConfigService update database (db.commit)
19. ModelConfigService convert ModelConfig thành ModelConfigResponse (masked API key)
20. model_config_routes.py trả về ModelConfigResponse JSON
21. ModelConfigService (frontend) nhận response và đóng gói với ServiceResult<ModelConfig>
22. ModelConfigUpdateDialog.tsx cập nhật danh sách Model bằng dữ liệu mới từ response
23. Component re-render và hiển thị danh sách Model được cập nhật
24. Toast success notification được hiển thị: "Cập nhật Model thành công"

---

## Kịch bản 4: Xóa Model

1. Tại giao diện danh sách Model, User click icon xóa trên một Model
2. ModelConfigTab.tsx gọi `handleDelete(modelId)`
3. ModelConfigTab.tsx set state `modelToDelete` và mở ModelConfigDeleteDialog.tsx
4. ModelConfigDeleteDialog.tsx hiển thị thông báo xác nhận xóa
5. User xác nhận muốn xóa
6. ModelConfigDeleteDialog.tsx gọi `ModelConfigService.deleteModel()`
7. ModelConfigService gửi DELETE request tới `/model-config/{modelId}` endpoint
8. model_config_routes.py nhận request và gọi `delete_model()`
9. ModelConfigService tìm Model theo modelId
10. ModelConfigService xóa các related ModelUsage records
11. ModelConfigService xóa Model (db.delete, db.commit)
12. model_config_routes.py trả về MessageResponse JSON
13. ModelConfigService (frontend) nhận response
14. ModelConfigTab.tsx update state, loại bỏ Model khỏi danh sách
15. Toast success notification được hiển thị: "Xóa Model thành công"
16. Dialog được đóng

---

## Kịch bản 5: Xem danh sách Model Wrapper

1. Admin truy cập trang Model Config
2. Admin click tab "Model Wrappers"
3. ModelWrapperTab.tsx khởi tạo và gọi `ModelConfigService.getWrappers()` để lấy danh sách Wrapper
4. ModelConfigService gửi request GET tới endpoint `/model-config/wrapper`
5. model_config_routes.py nhận request và gọi `get_wrappers()`
6. ModelConfigService truy vấn cơ sở dữ liệu để lấy tất cả Model Wrapper
7. ModelConfigService convert Wrapper objects thành ModelWrapperResponse objects
8. model_config_routes.py trả về danh sách ModelWrapperResponse JSON
9. ModelConfigService (frontend) nhận danh sách từ response và đóng gói với ServiceResult<ModelWrapper[]>
10. ModelWrapperTab.tsx cập nhật state `wrappers` với dữ liệu nhận được
11. Component re-render và hiển thị danh sách Model Wrapper cho Admin

---

## Kịch bản 6: Tạo Model Wrapper mới

1. Tại giao diện danh sách Model Wrapper, Admin click nút "Tạo Model Wrapper mới"
2. ModelWrapperTab.tsx gọi `handleCreate()`
3. ModelWrapperTab.tsx gọi ModelWrapperCreateDialog.tsx
4. ModelWrapperCreateDialog.tsx hiển thị giao diện form cho Admin
5. Admin nhập tên Wrapper, chọn Task Type, nhập Instruction và click nút "Tạo"
6. ModelWrapperCreateDialog.tsx validate form data
7. ModelWrapperCreateDialog.tsx gọi `ModelConfigService.createWrapper()`
8. ModelConfigService gửi POST request tới `/model-config/wrapper` endpoint
9. model_config_routes.py nhận request và gọi `create_wrapper()`
10. ModelConfigService tạo ModelWrapper model mới với các field: name, task_type, instruction
11. ModelConfigService kiểm tra unique constraint (name + task_type)
12. ModelConfigService thêm wrapper vào database (db.add, db.commit)
13. ModelConfigService convert ModelWrapper thành ModelWrapperResponse
14. model_config_routes.py trả về ModelWrapperResponse JSON
15. ModelConfigService (frontend) nhận response và đóng gói với ServiceResult<ModelWrapper>
16. ModelWrapperCreateDialog.tsx cập nhật danh sách Wrapper bằng dữ liệu mới từ response
17. Component re-render và hiển thị danh sách Wrapper được cập nhật
18. Toast success notification được hiển thị: "Tạo Model Wrapper thành công"

---

## Kịch bản 7: Chỉnh sửa Model Wrapper

1. Tại giao diện danh sách Model Wrapper, Admin click nút "Chỉnh sửa" (icon edit) trên một Wrapper
2. ModelWrapperTab.tsx gọi `handleEdit(wrapper)`
3. ModelWrapperTab.tsx gọi ModelWrapperUpdateDialog.tsx với dữ liệu wrapper=selected
4. ModelWrapperUpdateDialog.tsx populate form với thông tin Wrapper hiện tại
5. ModelWrapperUpdateDialog.tsx hiển thị thông tin: Tên, Task Type, Instruction
6. Admin cập nhật các thông tin và click nút "Lưu"
7. ModelWrapperUpdateDialog.tsx validate form data
8. ModelWrapperUpdateDialog.tsx gọi `ModelConfigService.updateWrapper()`
9. ModelConfigService gửi PUT request tới `/model-config/wrapper/{wrapperId}` endpoint
10. model_config_routes.py nhận request và gọi `update_wrapper()`
11. ModelConfigService tìm Wrapper theo wrapperId
12. ModelConfigService cập nhật các field có trong updateData
13. ModelConfigService update database (db.commit)
14. ModelConfigService convert ModelWrapper thành ModelWrapperResponse
15. model_config_routes.py trả về ModelWrapperResponse JSON
16. ModelConfigService (frontend) nhận response và đóng gói với ServiceResult<ModelWrapper>
17. ModelWrapperUpdateDialog.tsx cập nhật danh sách Wrapper bằng dữ liệu mới từ response
18. Component re-render và hiển thị danh sách Wrapper được cập nhật
19. Toast success notification được hiển thị: "Cập nhật Model Wrapper thành công"

---

## Kịch bản 8: Xóa Model Wrapper

1. Tại giao diện danh sách Model Wrapper, Admin click icon xóa trên một Wrapper
2. ModelWrapperTab.tsx gọi `handleDelete(wrapperId)`
3. ModelWrapperTab.tsx set state `wrapperToDelete` và mở ModelWrapperDeleteDialog.tsx
4. ModelWrapperDeleteDialog.tsx hiển thị thông báo xác nhận xóa
5. Admin xác nhận muốn xóa
6. ModelWrapperDeleteDialog.tsx gọi `ModelConfigService.deleteWrapper()`
7. ModelConfigService gửi DELETE request tới `/model-config/wrapper/{wrapperId}` endpoint
8. model_config_routes.py nhận request và gọi `delete_wrapper()`
9. ModelConfigService tìm Wrapper theo wrapperId
10. ModelConfigService kiểm tra xem Wrapper có đang được sử dụng (có related Models)
11. Nếu có Model sử dụng, throw exception
12. Nếu không có, xóa Wrapper (db.delete, db.commit)
13. model_config_routes.py trả về MessageResponse JSON
14. ModelConfigService (frontend) nhận response
15. ModelWrapperTab.tsx update state, loại bỏ Wrapper khỏi danh sách
16. Toast success notification được hiển thị: "Xóa Model Wrapper thành công"
17. Dialog được đóng
