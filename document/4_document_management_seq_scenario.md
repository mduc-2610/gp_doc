# Sequence Scenario - Quản lý Document

## 1. Xem danh sách Document

1. User mở trang chi tiết Session
2. DocAgentPage.tsx khởi tạo và gọi SideBarLeft.tsx
3. SideBarLeft.tsx gọi hàm loadDocumentsFromSession()
4. SideBarLeft.tsx gọi DocumentActionsDialog.tsx
5. DocumentActionsDialog.tsx gọi DocAgentService
6. DocAgentService gọi hàm getDocumentsBySession()
7. DocAgentService gửi yêu cầu GET "/document" đến document_routes.py
8. document_routes.py gọi DocumentService.get_documents_by_session()
9. DocumentService xử lý yêu cầu và gọi chính nó
10. DocumentService gọi Document
11. Document khởi tạo đối tượng Document()
12. Document trả về dữ liệu cho DocumentService
13. DocumentService trả về danh sách document cho document_routes.py
14. document_routes.py trả về kết quả cho DocAgentService
15. DocAgentService trả về danh sách document cho SideBarLeft.tsx
16. SideBarLeft.tsx hiển thị danh sách document cho User

---

## 2. Xóa Document

1. User click icon đốt lên một Document trong SideBarLeft
2. SideBarLeft gọi DocumentActionsDialog.tsx
3. DocumentActionsDialog.tsx hiển thị dialog với các tùy chọn
4. User click nút "Xóa"
5. DocumentActionsDialog.tsx gọi DocumentDeleteDialog.tsx
6. DocumentDeleteDialog.tsx hiển thị dialog xác nhận xóa
7. User nhập tên document và click "Đối tên" (xác nhận xóa)
8. DocumentDeleteDialog.tsx gọi hàm handleDeleteDocument()
9. DocumentDeleteDialog.tsx gọi DocAgentService
10. DocAgentService gọi hàm deleteDocument()
11. DocAgentService gửi yêu cầu DELETE "/document/{document_id}" đến document_routes.py
12. document_routes.py gọi DocumentService.delete_document()
13. DocumentService xử lý yêu cầu xóa và gọi chính nó
14. DocumentService gọi MessageResponse
15. MessageResponse khởi tạo đối tượng MessageResponse()
16. MessageResponse trả về thông báo cho DocumentService
17. DocumentService trả về kết quả cho document_routes.py
18. document_routes.py trả về thông báo thành công cho DocAgentService
19. DocAgentService trả về kết quả cho SideBarLeft
20. SideBarLeft hiển thị thông báo xóa thành công cho User

---

## 3. Cập nhật Document

1. User click icon đốt lên một Document trong SideBarLeft
2. SideBarLeft gọi DocumentActionsDialog.tsx
3. DocumentActionsDialog.tsx hiển thị dialog với các tùy chọn
4. User click nút "Đối tên"
5. DocumentActionsDialog.tsx gọi DocumentUpdateDialog.tsx
6. DocumentUpdateDialog.tsx hiển thị dialog cập nhật
7. User nhập tên mới và click "Đối tên"
8. DocumentUpdateDialog.tsx gọi hàm validate()
9. DocumentUpdateDialog.tsx gọi hàm handleUpdatetoDocument()
10. DocumentUpdateDialog.tsx gọi DocAgentService
11. DocAgentService gọi hàm updateDocument()
12. DocAgentService gửi yêu cầu PUT "/document/{document_id}" đến document_routes.py
13. document_routes.py gọi DocumentService.update_document()
14. DocumentService xử lý yêu cầu cập nhật và gọi chính nó
15. DocumentService gọi Document
16. Document khởi tạo đối tượng Document() với dữ liệu mới
17. Document trả về dữ liệu đã cập nhật cho DocumentService
18. DocumentService trả về document đã cập nhật cho document_routes.py
19. document_routes.py trả về kết quả cho DocAgentService
20. DocAgentService trả về document đã cập nhật cho SideBarLeft
21. SideBarLeft hiển thị document đã cập nhật cho User

---

## 4. Upload Document

1. User click nút "Tải lên" từ icon (+) trong SideBarLeft
2. SideBarLeft hiển thị UploadDialog.tsx

**Trường hợp Tab "Tệp":**
3a. User chọn tab "Tệp" và gọi hàm addFileItem()
4a. UploadDialog.tsx gọi hàm addUpload()
5a. UploadDialog.tsx gọi hàm validateBatchSize()
6a. UploadDialog.tsx gọi hàm handleUploadAll()

**Trường hợp Tab "Liên kết":**
3b. User chọn tab "Liên kết" và gọi hàm addUrlItem()
4b. UploadDialog.tsx gọi hàm validateUrlCount()
5b. UploadDialog.tsx gọi hàm handleUploadAll()

7. UploadDialog.tsx gọi DocAgentService
8. DocAgentService gọi hàm process_file()
9. DocAgentService gửi yêu cầu POST "/document/upload" đến document_routes.py
10. document_routes.py gọi DocumentService.create_file()
11. DocumentService gọi hàm get_parse_file()
12. DocumentService gọi hàm create_file()
13. DocumentService gọi hàm get_parse()
14. DocumentService gọi hàm process_url()

**Nếu get_parse.return == url:**
15a. DocumentService gọi hàm create_http()

16. DocumentService gọi hàm process_url()
17. DocumentService gọi hàm create_force_file()
18. DocumentService gọi Document
19. Document khởi tạo đối tượng Document()
20. Document trả về dữ liệu document mới cho DocumentService
21. DocumentService trả về document đã tạo cho document_routes.py
22. document_routes.py trả về kết quả cho DocAgentService
23. DocAgentService trả về document đã upload cho SideBarLeft
24. SideBarLeft hiển thị document mới cho User
