# Sequence Scenario - Quản lý Document

## Kịch bản 1: Upload Document

1. Tại giao diện Session chi tiết, hệ thống hiển thị DocAgentPage với SideBarLeft
2. SideBarLeft.tsx tự động gọi `fetchSessionInfo()` để lấy danh sách Document
3. DocAgentPage gọi `DocAgentService.getSessionDetail()` để lấy thông tin Session và danh sách Document
4. DocAgentService gửi request GET tới endpoint `/session/sessions/{sessionId}`
5. Backend route nhận request và session_service được gọi hàm `get_session_detail()`
6. session_service truy vấn cơ sở dữ liệu và trả về Session với danh sách Document
7. session_routes.py trả về SessionDetailResponse JSON
8. DocAgentService nhận response và đóng gói với ServiceResult<Session>
9. DocAgentPage nhận ServiceResult<Session> và cập nhật state documents
10. SideBarLeft.tsx hiển thị danh sách Document
11. User click "Upload" trên SideBarLeft
12. SideBarLeft.tsx gọi `onUploadClick()`
13. DocAgentPage cập nhật state `isUploadDialogOpen = true`
14. DocumentUploadDialog component được render với 2 tabs: "Tệp" và "Liên kết"

**Nếu upload File:**
15a. User chọn tab "Tệp"
16a. User chọn một hoặc nhiều file từ máy tính
17a. DocumentUploadDialog gọi `addFileItems()` để thêm file vào uploadItems
18a. DocumentUploadDialog validate kích thước file bằng `validateBatchSize()`

**Nếu upload URL:**
15b. User chọn tab "Liên kết"
16b. User nhập URL vào input field
17b. User click "Thêm"
18b. DocumentUploadDialog gọi `addUrlItem()`
19b. DocumentUploadDialog validate số lượng URL bằng `validateUrlCount()`
20b. DocumentUploadDialog thêm URL vào uploadItems

**Tiếp tục cho cả File và URL:**
21. DocumentUploadDialog hiển thị danh sách items trong upload queue
22. User click tải lên (hiển thị số lượng mục)
23. DocumentUploadDialog gọi `handleUploadAll()`
24. DocumentUploadDialog lặp qua từng uploadItem và gọi `uploadSingleItem()`

**Đối với mỗi item:**
25. Nếu là file: DocumentUploadDialog gọi `DocAgentService.uploadFile()` để thêm file vào uploadItems
26. Nếu là URL: DocumentUploadDialog gọi `DocAgentService.uploadUrl()`
27. DocAgentService tạo FormData với dữ liệu tương ứng (file/url, session_id, user_id)
28. DocAgentService gửi POST request tới `/document/file` hoặc `/document/url` endpoint
29. Backend route nhận request và DocumentProcessService được gọi `process_file()` hoặc `process_url()`

**Backend Processing - Step 1: Lấy Parse ModelConfig**
30. DocumentProcessService gọi `get_parse_task_type()` để xác định task type
31. DocumentProcessService gọi `ModelConfigService.get_model_by_type_and_task()`
32. ModelConfigService truy vấn database để lấy ModelConfig với wrapper.task_type tương ứng
33. ModelConfigService trả về parse_model_config (chứa thông tin model và wrapper)

**Backend Processing - Step 2: Tạo Parser từ Factory**
34. DocumentProcessService gọi `ParserFactory.create_from_source_type()`
35. ParserFactory mapping source_type và file_category để xác định ParserType:
    - UPLOAD + IMAGE → ImageParser
    - UPLOAD + AUDIO/VIDEO → AudioParser
    - UPLOAD + DOCUMENT → DocParser
    - YOUTUBE → YoutubeParser
    - WEB → WebParser
36. ParserFactory khởi tạo Parser tương ứng với model_config
37. ParserFactory trả về parser instance

**Backend Processing - Step 3: Lưu file và tạo Document**
38. Nếu là upload file: DocumentProcessService lưu file vào storage provider (Local/S3)
39. DocumentProcessService gọi `DocumentService.create_document()` với thông tin:
    - filename, source_name, file_type, source_type, file_size
    - storage_provider, storage_bucket
    - processing_status = PROCESSING
40. DocumentService INSERT Document record vào database
41. DocumentService trả về document object

**Backend Processing - Step 4: Parse Document**
42. DocumentProcessService gọi `parser.parse()` với source_data là file path hoặc URL
43. Parser xử lý theo loại:
    - DocParser: Extract text từ PDF/DOCX/PPTX
    - AudioParser: Transcribe audio/video thành text bằng model
    - YoutubeParser: Download video và transcribe
    - WebParser: Scrape nội dung web
    - ImageParser: OCR để extract text từ image
44. Parser trả về raw_text (text đã được extract)
45. DocumentProcessService gọi `DocumentService.update_document()`
46. DocumentService UPDATE Document trong database với processing_status mới

**Backend Processing - Step 5: Lấy Embedding ModelConfig**
47. DocumentProcessService gọi `ModelConfigService.get_model_by_type_and_task()`
48. ModelConfigService truy vấn database để lấy ModelConfig cho embedding
49. ModelConfigService trả về embedding_model_config (chứa wrapper info như "fastembed", "openai")

**Backend Processing - Step 6: Tạo Embedding Model từ Factory**
50. DocumentProcessService gọi `EmbeddingFactory.create()`
51. EmbeddingFactory lấy wrapper_name từ model_config.wrapper.name
52. EmbeddingFactory mapping wrapper_name để chọn embedding class:
    - "fastembed" → FastEmbedModel
    - "openai" → OpenAIEmbeddingModel
53. EmbeddingFactory khởi tạo EmbeddingModel với model_config
54. EmbeddingFactory trả về embedding_model instance

**Backend Processing - Step 7: Tạo Splitter và split text**
55. DocumentProcessService xác định splitter_type (RECURSIVE hoặc SEMANTIC)
56. DocumentProcessService gọi `TextSplitterFactory.create_splitter()`
57. TextSplitterFactory lấy splitter_class từ registry theo splitter_type:
    - RECURSIVE → RecursiveSplitter
    - SEMANTIC → SemanticSplitter
58. TextSplitterFactory khởi tạo Splitter với kwargs (nếu có)
59. TextSplitterFactory trả về splitter instance
60. DocumentProcessService gọi `splitter.split_text()`
61. Splitter phân tách raw_text thành các chunks:
    - RecursiveSplitter: Tự động điều chỉnh chunk_size/overlap dựa trên độ dài document
    - SemanticSplitter: Phân tách dựa trên semantic similarity với embedding
62. Splitter trả về text_chunks[] (mảng các dict chứa chunk info)

**Backend Processing - Step 8: Tạo embeddings và lưu chunks**
63. DocumentProcessService lặp qua từng chunk trong text_chunks[]
64. Với mỗi chunk, gọi `embedding_model.embed()`
65. EmbeddingModel generate embedding vector cho chunk_text
66. EmbeddingModel trả về embedding_vector (mảng số float)
67. DocumentProcessService gọi `DocumentService.create_document_chunk()` với:
    - document_id
    - chunk_index (vị trí chunk)
    - chunk_text (nội dung text)
    - embedding (embedding_vector)
68. DocumentService INSERT DocumentChunk record vào database
69. Lặp lại bước 64-68 cho tất cả các chunks

**Hoàn tất và trả về Frontend:**
70. DocumentProcessService trả về document object hoàn chỉnh
71. document_routes.py convert document thành DocumentResponse JSON
72. DocAgentService nhận response và đóng gói với ServiceResult<Document>
73. DocumentUploadDialog nhận ServiceResult<Document>
74. DocumentUploadDialog cập nhật uploadItem status = 'completed'
75. Lặp lại bước 25-74 cho tất cả các items trong upload queue
76. DocumentUploadDialog gọi callback `onDocumentsUploaded()` với danh sách Document đã upload
77. DocAgentPage cập nhật state documents với Document mới
78. SideBarLeft.tsx re-render và hiển thị danh sách Document được cập nhật
79. Toast success notification được hiển thị: "Upload thành công"

---

## Kịch bản 2: Xem danh sách Document

1. Tại giao diện Session chi tiết, hệ thống hiển thị DocAgentPage
2. DocAgentPage trong useEffect gọi `fetchSessionInfo()`
3. DocAgentPage gọi `DocAgentService.getSessionDetail()`
4. DocAgentService gửi request GET tới endpoint `/session/sessions/{sessionId}`
5. Backend route nhận request và session_service được gọi `get_session_detail()`
6. session_service truy vấn database và trả về Session với danh sách Document
7. session_routes.py trả về SessionDetailResponse JSON bao gồm danh sách Document
8. DocAgentService nhận response và đóng gói với ServiceResult<Session>
9. DocAgentPage nhận ServiceResult<Session> và cập nhật state:
   - sessionInfo = result.data
   - documents = result.data.documents
10. SideBarLeft.tsx nhận props documents và hiển thị danh sách
11. User có thể nhập vào ô search để lọc Document theo tên
12. SideBarLeft.tsx filter documents theo searchTerm
13. Component re-render và hiển thị danh sách Document đã lọc

---

## Kịch bản 3: Xem thông tin Document

1. Tại giao diện Session chi tiết, SideBarLeft hiển thị danh sách Document
2. User click vào một Document trong danh sách
3. SideBarLeft.tsx gọi `handleDocumentClick()`
4. SideBarLeft.tsx gọi callback `onDocumentSelect()`
5. DocAgentPage cập nhật state `selectedDocumentId = doc.id`
6. SideBarLeft.tsx nhận props selectedDocumentId và highlight Document được chọn
7. User click vào icon info hoặc double click vào Document
8. SideBarLeft.tsx cập nhật state `summaryDialogOpen = true` và `selectedDocument = doc`
9. DocumentInfoDialog component được render với props document
10. DocumentInfoDialog hiển thị thông tin chi tiết:
    - Thumbnail (nếu có)
    - Tên Document (source_name hoặc filename)
    - Loại file (file_type)
    - Kích thước (file_size)
    - Nguồn (source_type)
    - Trạng thái xử lý (processing_status)
    - Ngày tạo (created_at)
11. User có thể click "View Source" để xem file gốc
12. DocumentInfoDialog gọi `onSourceFileClick()`
13. Hệ thống mở file trong tab mới hoặc download file
14. User có thể click "View Content" để xem nội dung đã xử lý
15. DocumentInfoDialog gọi `onContentFileClick()`
16. Hệ thống mở file content trong tab mới hoặc download file

---

## Kịch bản 4: Cập nhật Document (Đổi tên)

1. Tại giao diện Session chi tiết, SideBarLeft hiển thị danh sách Document
2. User hover vào một Document
3. SideBarLeft.tsx hiển thị icon menu (3 chấm)
4. User click vào icon menu
5. SideBarLeft.tsx gọi `handleActionsClick()`
6. SideBarLeft.tsx cập nhật state:
   - dropdownOpen = true
   - selectedDocumentForActions = doc
   - dropdownPosition = { x, y }
   - isLastDocument = (docIndex === filteredDocuments.length - 1)
7. DocumentActionDialog component được render tại vị trí dropdownPosition
8. User click vào option "Update"
9. DocumentActionDialog gọi `handleUpdateClick()`
10. DocumentActionDialog cập nhật state `showUpdateDialog = true` và đóng dropdown
11. DocumentUpdateDialog component được render với props document
12. DocumentUpdateDialog auto-focus vào input và điền sẵn tên hiện tại
13. User nhập tên hiển thị mới vào input field
14. User click "Update"
15. DocumentUpdateDialog gọi `handleUpdate()`
16. DocumentUpdateDialog validate tên không rỗng
17. DocumentUpdateDialog gọi `DocAgentService.updateDocument()`
18. DocAgentService gửi PUT request tới `/document/documents/{documentId}`
19. Backend route nhận request và DocumentService được gọi `update_document()`
20. DocumentService lấy document từ database, cập nhật source_name và commit
21. document_routes.py trả về DocumentResponse JSON
22. DocAgentService nhận response và đóng gói với ServiceResult<Document>
23. DocumentUpdateDialog nhận ServiceResult<Document>
24. DocumentUpdateDialog gọi callback `onDocumentUpdated()`
25. SideBarLeft.tsx cập nhật document trong danh sách
26. DocAgentPage cũng cập nhật documents state
27. Component re-render và hiển thị tên Document mới
28. Toast success notification được hiển thị: "Cập nhật Document thành công"
29. DocumentUpdateDialog đóng

---

## Kịch bản 5: Xóa Document

1. Tại giao diện Session chi tiết, SideBarLeft hiển thị danh sách Document
2. User hover vào một Document
3. SideBarLeft.tsx hiển thị icon menu (3 chấm)
4. User click vào icon menu
5. SideBarLeft.tsx gọi `handleActionsClick()`
6. SideBarLeft.tsx cập nhật state:
   - dropdownOpen = true
   - selectedDocumentForActions = doc
   - dropdownPosition = { x, y }
   - isLastDocument = (docIndex === filteredDocuments.length - 1)
7. DocumentActionDialog component được render tại vị trí dropdownPosition
8. User click vào option "Delete"
9. DocumentActionDialog gọi `handleDeleteClick()`
10. DocumentActionDialog cập nhật state `showDeleteDialog = true` và đóng dropdown
11. DocumentDeleteDialog component được render với props document
12. DocumentDeleteDialog hiển thị confirm message với tên Document
13. User click "Delete" để xác nhận xóa
14. DocumentDeleteDialog gọi `handleDelete()`
15. DocumentDeleteDialog gọi `DocAgentService.deleteDocument()`
16. DocAgentService gửi DELETE request tới `/document/documents/{documentId}`
17. Backend route nhận request và DocumentService được gọi `delete_document()`
18. DocumentService thực hiện soft delete: set is_active = false cho Document
19. DocumentService cũng set is_active = false cho tất cả DocumentChunk liên quan
20. document_routes.py trả về MessageResponse JSON
21. DocAgentService nhận response và đóng gói với ServiceResult<MessageResponse>
22. DocumentDeleteDialog nhận ServiceResult<MessageResponse>
23. DocumentDeleteDialog gọi callback `onDocumentDeleted()`
24. SideBarLeft.tsx loại bỏ document khỏi danh sách
25. DocAgentPage cũng cập nhật documents state (loại bỏ document)
26. Nếu Document bị xóa đang được chọn, DocAgentPage reset selectedDocumentId = null
27. Component re-render và hiển thị danh sách Document được cập nhật
28. Toast success notification được hiển thị: "Xóa Document thành công"
29. DocumentDeleteDialog đóng
