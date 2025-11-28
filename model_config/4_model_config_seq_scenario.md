# Sequence Scenario - Quản lý ModelConfig và ModelWrapper

## 1. Xem danh sách ModelConfig

1. User click tab "Cấu hình mô hình"
2. ModelConfigTab.tsx gọi hàm loadModelsFromUser()
3. ModelConfigTab.tsx gọi ModelConfigService
4. ModelConfigService gọi hàm getModelsByUser()
5. ModelConfigService gửi yêu cầu GET "/model-config/by-user/{user_id}" đến model_config_routes.py
6. model_config_routes.py gọi ModelConfigService.get_models_by_user()
7. ModelConfigService xử lý yêu cầu và gọi chính nó
8. ModelConfigService gọi ModelConfig
9. ModelConfig khởi tạo đối tượng ModelConfig()
10. ModelConfig trả về dữ liệu cho ModelConfigService
11. ModelConfigService trả về danh sách ModelConfig cho model_config_routes.py
12. model_config_routes.py trả về kết quả cho ModelConfigService
13. ModelConfigService trả về danh sách ModelConfig cho ModelConfigTab.tsx
14. ModelConfigTab.tsx hiển thị danh sách ModelConfig cho User

---

## 2. Tạo ModelConfig mới

1. User click "Tạo mô hình"
2. ModelConfigTab.tsx gọi ModelConfigCreateDialog.tsx
3. ModelConfigCreateDialog.tsx gọi hàm loadWrappers()
4. ModelConfigCreateDialog.tsx gọi ModelConfigService
5. ModelConfigService gọi hàm getWrappers()
6. ModelConfigService gửi yêu cầu GET "/model-config/wrapper" đến model_config_routes.py
7. model_config_routes.py gọi ModelConfigService.get_wrappers()
8. ModelConfigService xử lý yêu cầu và gọi chính nó
9. ModelConfigService gọi ModelWrapper
10. ModelWrapper khởi tạo đối tượng ModelWrapper()
11. ModelWrapper trả về dữ liệu cho ModelConfigService
12. ModelConfigService trả về danh sách ModelWrapper cho model_config_routes.py
13. model_config_routes.py trả về kết quả cho ModelConfigService
14. ModelConfigService hiển thị danh sách ModelWrapper cho User
15. User nhập thông tin và click "Lưu thay đổi"
16. ModelConfigCreateDialog.tsx gọi hàm validate()
17. ModelConfigCreateDialog.tsx gọi hàm handleCreateModel()
18. ModelConfigCreateDialog.tsx gọi ModelConfigService
19. ModelConfigService gọi hàm createModel()
20. ModelConfigService gửi yêu cầu POST "/model-config" đến model_config_routes.py
21. model_config_routes.py gọi ModelConfigService.create_model()
22. ModelConfigService xử lý yêu cầu và gọi chính nó
23. ModelConfigService gọi ModelConfig
24. ModelConfig khởi tạo đối tượng ModelConfig()
25. ModelConfig trả về dữ liệu cho ModelConfigService
26. ModelConfigService trả về ModelConfig mới cho model_config_routes.py
27. model_config_routes.py trả về kết quả cho ModelConfigService
28. ModelConfigService trả về ModelConfig mới cho ModelConfigTab.tsx
29. ModelConfigTab.tsx hiển thị ModelConfig mới cho User

---

## 3. Chỉnh sửa ModelConfig

1. User click icon update trên một ModelConfig
2. ModelConfigTab.tsx gọi ModelConfigUpdateDialog.tsx
3. ModelConfigUpdateDialog.tsx gọi hàm loadWrappers()
4. ModelConfigUpdateDialog.tsx gọi ModelConfigService
5. ModelConfigService gọi hàm getWrappers()
6. ModelConfigService gửi yêu cầu GET "/model-config/wrapper" đến model_config_routes.py
7. model_config_routes.py gọi ModelConfigService.get_wrappers()
8. ModelConfigService xử lý yêu cầu và gọi chính nó
9. ModelConfigService gọi ModelWrapper
10. ModelWrapper khởi tạo đối tượng ModelWrapper()
11. ModelWrapper trả về dữ liệu cho ModelConfigService
12. ModelConfigService trả về danh sách ModelWrapper cho model_config_routes.py
13. model_config_routes.py trả về kết quả cho ModelConfigService
14. ModelConfigService hiển thị danh sách ModelWrapper cho User
15. User nhập thông tin và click "Lưu thay đổi"
16. ModelConfigUpdateDialog.tsx gọi hàm validate()
17. ModelConfigUpdateDialog.tsx gọi hàm handleUpdateModel()
18. ModelConfigUpdateDialog.tsx gọi ModelConfigService
19. ModelConfigService gọi hàm updateModel()
20. ModelConfigService gửi yêu cầu PUT "/model-config/{model_id}" đến model_config_routes.py
21. model_config_routes.py gọi ModelConfigService.update_model()
22. ModelConfigService xử lý yêu cầu và gọi chính nó
23. ModelConfigService gọi ModelConfig
24. ModelConfig khởi tạo đối tượng ModelConfig()
25. ModelConfig trả về dữ liệu cho ModelConfigService
26. ModelConfigService trả về ModelConfig đã cập nhật cho model_config_routes.py
27. model_config_routes.py trả về kết quả cho ModelConfigService
28. ModelConfigService trả về ModelConfig đã cập nhật cho ModelConfigTab.tsx
29. ModelConfigTab.tsx hiển thị ModelConfig đã cập nhật cho User

---

## 4. Xóa ModelConfig

1. User click icon delete trên một ModelConfig
2. ModelConfigTab.tsx gọi ModelConfigUpdateDialog.tsx
3. ModelConfigUpdateDialog.tsx hiển thị dialog xác nhận cho User
4. User click "Xác nhận"
5. ModelConfigUpdateDialog.tsx gọi hàm handleDeleteModel()
6. ModelConfigUpdateDialog.tsx gọi ModelConfigService
7. ModelConfigService gọi hàm deleteModel()
8. ModelConfigService gửi yêu cầu DELETE "/model-config/{model_id}" đến model_config_routes.py
9. model_config_routes.py gọi ModelConfigService.delete_model()
10. ModelConfigService xử lý yêu cầu và gọi chính nó
11. ModelConfigService gọi MessageResponse
12. MessageResponse khởi tạo đối tượng MessageResponse()
13. MessageResponse trả về thông báo cho ModelConfigService
14. ModelConfigService trả về kết quả cho model_config_routes.py
15. model_config_routes.py trả về thông báo thành công cho ModelConfigService
16. ModelConfigService trả về kết quả cho ModelConfigTab.tsx
17. ModelConfigTab.tsx hiển thị thông báo cho User

---

## 5. Preload ModelConfig

1. User click "Tải mô hình mẫu" và xác nhận
2. ModelConfigPage.tsx gọi hàm handlePreloadModels()
3. ModelConfigPage.tsx gọi ModelConfigService
4. ModelConfigService gọi hàm preloadModels()
5. ModelConfigService gửi yêu cầu GET "/model-config/preload/{user_id}" đến model_config_routes.py
6. model_config_routes.py gọi ModelConfigService.preload_models()
7. ModelConfigService xử lý yêu cầu và gọi chính nó
8. ModelConfigService gọi ModelConfig
9. ModelConfig khởi tạo đối tượng ModelConfig()
10. ModelConfig trả về dữ liệu cho ModelConfigService
11. ModelConfigService trả về kết quả cho model_config_routes.py
12. model_config_routes.py trả về kết quả cho ModelConfigService
13. ModelConfigService trả về kết quả cho ModelConfigPage.tsx
14. ModelConfigPage.tsx gọi hàm loadModelsFromUser()
15. ModelConfigPage.tsx gọi ModelConfigService
16. ModelConfigService gọi hàm getModelsByUser()
17. ModelConfigService gửi yêu cầu GET "/model-config/by-user/{user_id}" đến model_config_routes.py
18. model_config_routes.py gọi ModelConfigService.get_models_by_user()
19. ModelConfigService xử lý yêu cầu và gọi chính nó
20. ModelConfigService gọi ModelConfig
21. ModelConfig khởi tạo đối tượng ModelConfig()
22. ModelConfig trả về dữ liệu cho ModelConfigService
23. ModelConfigService trả về danh sách ModelConfig cho model_config_routes.py
24. model_config_routes.py trả về kết quả cho ModelConfigService
25. ModelConfigService trả về danh sách ModelConfig cho ModelConfigPage.tsx
26. ModelConfigPage.tsx hiển thị danh sách ModelConfig cho User

---

## 6. Xem danh sách ModelWrapper

1. User click tab "Cấu hình Wrapper"
2. ModelWrapperTab.tsx gọi hàm loadWrappers()
3. ModelWrapperTab.tsx gọi ModelConfigService
4. ModelConfigService gọi hàm getWrappers()
5. ModelConfigService gửi yêu cầu GET "/model-config/wrapper" đến model_config_routes.py
6. model_config_routes.py gọi ModelConfigService.get_wrappers()
7. ModelConfigService xử lý yêu cầu và gọi chính nó
8. ModelConfigService gọi ModelWrapper
9. ModelWrapper khởi tạo đối tượng ModelWrapper()
10. ModelWrapper trả về dữ liệu cho ModelConfigService
11. ModelConfigService trả về danh sách ModelWrapper cho model_config_routes.py
12. model_config_routes.py trả về kết quả cho ModelConfigService
13. ModelConfigService trả về danh sách ModelWrapper cho ModelWrapperTab.tsx
14. ModelWrapperTab.tsx hiển thị danh sách ModelWrapper cho User

---

## 7. Tạo ModelWrapper mới (Admin only)

1. Admin click "Tạo Wrapper"
2. ModelWrapperTab.tsx gọi ModelWrapperCreateDialog.tsx
3. ModelWrapperCreateDialog.tsx hiển thị dialog cho Admin
4. Admin nhập thông tin và click "Tạo"
5. ModelWrapperCreateDialog.tsx gọi hàm validate()
6. ModelWrapperCreateDialog.tsx gọi hàm handleCreateWrapper()
7. ModelWrapperCreateDialog.tsx gọi ModelConfigService
8. ModelConfigService gọi hàm createWrapper()
9. ModelConfigService gửi yêu cầu POST "/model-config/wrapper" đến model_config_routes.py
10. model_config_routes.py gọi ModelConfigService.create_wrapper()
11. ModelConfigService xử lý yêu cầu và gọi chính nó
12. ModelConfigService gọi ModelWrapper
13. ModelWrapper khởi tạo đối tượng ModelWrapper()
14. ModelWrapper trả về dữ liệu cho ModelConfigService
15. ModelConfigService trả về ModelWrapper mới cho model_config_routes.py
16. model_config_routes.py trả về kết quả cho ModelConfigService
17. ModelConfigService trả về ModelWrapper mới cho ModelWrapperTab.tsx
18. ModelWrapperTab.tsx hiển thị ModelWrapper mới cho Admin

---

## 8. Chỉnh sửa ModelWrapper (Admin only)

1. Admin click icon update trên một ModelWrapper
2. ModelWrapperTab.tsx gọi ModelWrapperUdateDialog.tsx
3. ModelWrapperUdateDialog.tsx hiển thị dialog cho Admin
4. Admin nhập thông tin và click "Cập nhật"
5. ModelWrapperUdateDialog.tsx gọi hàm validate()
6. ModelWrapperUdateDialog.tsx gọi hàm handleUpdateWrapper()
7. ModelWrapperUdateDialog.tsx gọi ModelConfigService
8. ModelConfigService gọi hàm updateWrapper()
9. ModelConfigService gửi yêu cầu PUT "/model-config/wrapper/{wrapper_id}" đến model_config_routes.py
10. model_config_routes.py gọi ModelConfigService.update_wrapper()
11. ModelConfigService xử lý yêu cầu và gọi chính nó
12. ModelConfigService gọi ModelWrapper
13. ModelWrapper khởi tạo đối tượng ModelWrapper()
14. ModelWrapper trả về dữ liệu cho ModelConfigService
15. ModelConfigService trả về ModelWrapper đã cập nhật cho model_config_routes.py
16. model_config_routes.py trả về kết quả cho ModelConfigService
17. ModelConfigService trả về ModelWrapper đã cập nhật cho ModelWrapperTab.tsx
18. ModelWrapperTab.tsx hiển thị ModelWrapper đã cập nhật cho Admin

---

## 9. Xóa ModelWrapper (Admin only)

1. Admin click icon delete trên một ModelWrapper
2. ModelWrapperTab.tsx gọi ModelWrapperDeleteDialog.tsx
3. ModelWrapperDeleteDialog.tsx hiển thị dialog xác nhận cho Admin
4. Admin nhập thông tin và click "Cập nhật"
5. ModelWrapperDeleteDialog.tsx gọi hàm handleDeleteWrapper()
6. ModelWrapperDeleteDialog.tsx gọi ModelConfigService
7. ModelConfigService gọi hàm deleteWrapper()
8. ModelConfigService gửi yêu cầu DELETE "/model-config/wrapper/{wrapper_id}" đến model_config_routes.py
9. model_config_routes.py gọi ModelConfigService.delete_wrapper()
10. ModelConfigService xử lý yêu cầu và gọi chính nó
11. ModelConfigService gọi MessageResponse
12. MessageResponse khởi tạo đối tượng MessageResponse()
13. MessageResponse trả về thông báo cho ModelConfigService
14. ModelConfigService trả về kết quả cho model_config_routes.py
15. model_config_routes.py trả về thông báo thành công cho ModelConfigService
16. ModelConfigService trả về kết quả cho ModelWrapperTab.tsx
17. ModelWrapperTab.tsx hiển thị thông báo cho Admin
