# Sequence Scenario - Generation với Batch

## Kịch bản 1: Tạo yêu cầu generation

1. Tại giao diện Session chi tiết, User click nút "Generate"
2. DocAgentPage.js hiển thị QuestionGenDialog
3. QuestionGenDialog gọi `DocAgentService.getUserGenerationProcess()` để kiểm tra process đang chạy
4. DocAgentService gửi GET request tới `/gen/user-process` với userId và resultType
5. Backend GenerationProcessService kiểm tra có process active không
6. Backend trả về null (không có process đang chạy)
7. QuestionGenDialog hiển thị form generation với danh sách Documents
8. User chọn documents, điền số lượng, topic, và các tham số nâng cao
9. User click nút "Generate"
10. QuestionGenDialog validate form data
11. QuestionGenDialog gọi `DocAgentService.batchGenerate()` với QuestionGenRequest và FlashcardGenRequest
12. DocAgentService gửi POST request tới `/gen/batch-generate`
13. Backend gen_routes nhận request và tạo QuestionGenerationService và FlashcardGenerationService
14. Backend tạo GenerationProcess cho Question với status PENDING
15. Backend tạo GenerationBatch cho Question
16. Backend tạo GenerationProcess cho Flashcard với status PENDING
17. Backend tạo GenerationBatch cho Flashcard
18. Backend trả về BatchGenerateResponse với question_process_id và flashcard_process_id
19. DocAgentService nhận response và trả về ServiceResult
20. QuestionGenDialog gọi onContentGenerated callback
21. DocAgentPage nhận callback và gọi handleContentGenerated()
22. DocAgentPage kết nối WebSocket cho Question với DocSocketService.connectQuestionSocket()
23. DocSocketService tạo WebSocket connection tới `/gen/ws/question/{processId}`
24. WebSocket connection established
25. DocSocketService gửi message "start" với userId
26. Lặp lại bước 22-25 cho Flashcard
27. DocAgentPage đóng QuestionGenDialog
28. DocAgentPage hiển thị LiveProgress trong QuestionTab và FlashcardTab

---

## Kịch bản 2: Theo dõi tiến trình generation qua WebSocket

1. Sau khi tạo yêu cầu generation thành công, frontend nhận được process_id
2. Frontend gọi DocSocketService để thiết lập WebSocket connection
3. DocSocketService tạo WebSocket URL và kết nối tới backend endpoint
4. Backend nhận WebSocket connection request
5. Backend kiểm tra model config cho generation và embedding
6. Backend validate API keys nếu là remote model
7. Backend tạo service instance (QuestionGenerationService hoặc FlashcardGenerationService)
8. Backend register WebSocket vào manager
9. Frontend gửi message "start" kèm user_id qua WebSocket
10. Backend nhận message "start"
11. Backend kiểm tra nếu pipeline chưa chạy, khởi động pipeline
12. Backend gọi service.generate() để bắt đầu pipeline execution
13. Backend gửi snapshot ban đầu với thông tin process và danh sách steps
14. Frontend nhận snapshot message
15. Frontend parse snapshot data và cập nhật state với danh sách steps và process info
16. Frontend render LiveProgress component với danh sách steps
17. Backend bắt đầu thực hiện step đầu tiên của pipeline
18. Backend cập nhật step status thành RUNNING
19. Backend gửi state update message với thông tin step đang chạy
20. Frontend nhận state update message
21. Frontend cập nhật step tương ứng trong danh sách steps
22. Frontend re-render LiveProgress với step status mới
23. Backend hoàn thành step và cập nhật status thành COMPLETED
24. Backend gửi state update message với step COMPLETED
25. Frontend nhận message và cập nhật UI
26. Backend lặp lại bước 17-25 cho các steps tiếp theo (retrieve_context → generate_content → validate_results → save_batch)
27. Sau khi tất cả steps hoàn thành, backend gửi notify message với code "finished"
28. Frontend nhận finished message
29. Frontend gọi callback xử lý khi generation hoàn thành
30. Frontend đóng WebSocket connection
31. Frontend cập nhật state, xóa process và socket info
32. Frontend gọi API kiểm tra batch pending
33. Nếu có batch pending, hiển thị tab approval để User xem kết quả

---

## Kịch bản 3: Hủy tiến trình generation

1. User đang xem LiveProgress của generation process
2. User click nút "Cancel Process"
3. LiveProgress hiển thị confirm dialog
4. User xác nhận muốn hủy
5. LiveProgress gọi DocAgentService.cancelProcess() với processId và userId
6. DocAgentService gửi POST request tới `/gen/cancel/{processId}`
7. Backend gen_routes nhận request
8. Backend gọi GenerationProcessService.cancel_process()
9. GenerationProcessService lấy process từ database
10. GenerationProcessService validate user_id matches
11. GenerationProcessService cập nhật process status thành CANCELLED
12. GenerationProcessService commit transaction
13. GenerationProcessService trả về dict với status="cancelled"
14. Backend gen_routes trả về MessageResponse
15. DocAgentService nhận response và parse
16. DocAgentService trả về ServiceResult với success
17. LiveProgress nhận response
18. LiveProgress gọi onCancelComplete callback
19. DocAgentPage đóng WebSocket connection
20. DocAgentPage cập nhật generationState, remove process
21. LiveProgress hiển thị thông báo "Process cancelled successfully"
22. Backend pipeline check process status, thấy CANCELLED và throw ProcessCanceledException
23. Backend catch exception và dừng execution
24. Backend gọi ws_generation_manager.send_notify() với code="cancelled"
25. WSGenerationManager broadcast message tới các WebSocket còn lại (nếu có)

---

## Kịch bản 4: Xem kết quả batch sau khi generation hoàn thành

1. Generation process hoàn thành và gọi onFinished handler
2. QuestionTab/FlashcardTab gọi checkForPendingBatch()
3. Tab gọi DocAgentService.getPendingBatch() với userId, sessionId, resultType
4. DocAgentService gửi GET request tới `/batch/pending-batch`
5. Backend batch_routes nhận request
6. Backend gọi BatchService.get_pending_batch()
7. BatchService query database tìm GenerationBatch với status PENDING và result_type
8. BatchService trả về GenerationBatch hoặc None
9. Backend batch_routes trả về GenerationBatchResponse
10. DocAgentService nhận response và parse
11. DocAgentService trả về ServiceResult với GenerationBatch
12. Tab nhận batch và set pendingBatchProcessId
13. Tab gọi DocAgentService.getBatchItems() với batchId
14. DocAgentService gửi GET request tới `/batch/batch-item/by-batch/{batchId}`
15. Backend batch_routes nhận request
16. Backend gọi BatchService.get_batch_items_by_batch()
17. BatchService query database lấy tất cả BatchItem của batch
18. BatchService join với Question hoặc Flashcard table
19. BatchService trả về list of BatchItem với item data
20. Backend batch_routes trả về list of BatchItemResponse
21. DocAgentService nhận response và parse
22. DocAgentService trả về ServiceResult với BatchItem[]
23. Tab nhận batch items và extract Questions/Flashcards
24. Tab set showApprovalTab = true
25. QuestionApprovalTab/FlashcardApprovalTab render với danh sách items
26. User xem items trong approval tab, có thể switch giữa single và list layout

---

## Kịch bản 5: Duyệt kết quả batch

1. User đang xem batch items trong QuestionApprovalTab
2. User chọn các items bằng checkbox (hoặc select all)
3. User click nút "Approve Selected"
4. QuestionApprovalTab validate có items được chọn
5. QuestionApprovalTab gọi DocAgentService.approve() với BatchApprovalRequest
6. DocAgentService gửi POST request tới `/batch/approve`
7. Backend batch_routes nhận request
8. Backend gọi BatchService.approve_batch()
9. BatchService start database transaction
10. BatchService bulk update batch_item status thành APPROVED cho selected items
11. BatchService extract question_ids từ selected items
12. BatchService bulk insert Questions vào session
13. BatchService update batch approved_items_count
14. BatchService check nếu tất cả items đã processed, update batch status
15. BatchService commit transaction
16. BatchService trả về MessageResponse
17. Backend batch_routes trả về MessageResponse
18. DocAgentService nhận response
19. DocAgentService trả về ServiceResult với success
20. QuestionApprovalTab nhận response
21. QuestionApprovalTab gọi onApprovalComplete callback
22. QuestionTab reload data bằng cách gọi DocAgentService.getQuestionsBySession()
23. Backend trả về danh sách Questions đã được approve
24. QuestionTab cập nhật internalQuestions và hiển thị
25. QuestionApprovalTab check nếu còn pending items
26. Nếu không còn, set showApprovalTab = false
27. Nếu còn, tiếp tục hiển thị approval tab với remaining items
28. Hiển thị toast notification "Items approved successfully"

---

## Kịch bản 6: Từ chối kết quả batch

1. User đang xem batch items trong QuestionApprovalTab
2. User chọn các items muốn reject bằng checkbox
3. User nhập feedback text trong textarea (tùy chọn)
4. User click nút "Reject Selected"
5. QuestionApprovalTab validate có items được chọn
6. QuestionApprovalTab validate feedback length <= 500 characters
7. QuestionApprovalTab gọi DocAgentService.reject() với BatchRejectionRequest
8. DocAgentService gửi POST request tới `/batch/reject`
9. Backend batch_routes nhận request
10. Backend gọi BatchService.reject_batch()
11. BatchService start database transaction
12. BatchService bulk update batch_item status thành REJECTED cho selected items
13. Nếu có feedback, BatchService create BatchFeedback với feedback_type=REJECTION
14. BatchService update batch rejected_items_count
15. BatchService check nếu tất cả items đã processed, update batch status
16. BatchService commit transaction
17. BatchService trả về MessageResponse
18. Backend batch_routes trả về MessageResponse
19. DocAgentService nhận response
20. DocAgentService trả về ServiceResult với success
21. QuestionApprovalTab nhận response
22. QuestionApprovalTab reload batch items để cập nhật status
23. QuestionApprovalTab check nếu có rejected items, hiển thị nút "Regenerate"
24. QuestionApprovalTab check nếu còn pending items
25. Nếu không còn và không có rejected items, set showApprovalTab = false
26. Hiển thị toast notification "Items rejected successfully"

---

## Kịch bản 7: Tạo yêu cầu regenerate

1. User đang xem batch với rejected items
2. User click nút "Regenerate"
3. QuestionApprovalTab gọi DocAgentService.regenerate() với BatchRegenerationRequest
4. DocAgentService gửi POST request tới `/gen/regenerate`
5. Backend gen_routes nhận request
6. Backend gọi QuestionGenerationService.regenerate()
7. QuestionGenerationService lấy batch cũ từ database
8. QuestionGenerationService lấy rejected items từ batch
9. QuestionGenerationService lấy feedback từ BatchFeedback
10. QuestionGenerationService tạo GenerationProcess mới với process_order + 1
11. QuestionGenerationService tạo GenerationBatch mới với parent_batch_id = batch cũ
12. QuestionGenerationService build regeneration prompt với feedback guidance
13. QuestionGenerationService bắt đầu execute_pipeline() trong background
14. Backend gen_routes trả về GenerationBatch mới
15. DocAgentService nhận response
16. DocAgentService trả về ServiceResult với GenerationBatch
17. QuestionApprovalTab nhận response
18. QuestionApprovalTab set showApprovalTab = false (tạm thời)
19. DocAgentPage tự động kết nối WebSocket với process mới
20. DocAgentPage hiển thị LiveProgress cho regeneration process
21. Pipeline regeneration chạy tương tự như generation ban đầu
22. Sau khi hoàn thành, hiển thị approval tab mới với batch mới
23. User xem kết quả regeneration và approve/reject lại

