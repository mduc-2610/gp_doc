# Sequence Scenario - Generation với Batch

## 1. Xem kết quả Batch

1. User click tab "Câu hỏi"
2. QuestionTab.tsx khởi tạo và gọi QuestionApprovalTab.tsx
3. QuestionApprovalTab.tsx gọi hàm loadUserPendingBatch()
4. QuestionApprovalTab.tsx gọi DocAgentService
5. DocAgentService gọi hàm getUserPendingBatch()
6. DocAgentService gửi yêu cầu GET "/batch/pending-batch?user_id&session_id&result_type" đến batch_routes.py
7. batch_routes.py gọi BatchService.get_user_pending_batch()
8. BatchService xử lý yêu cầu và gọi chính nó
9. BatchService gọi GenerationBatch
10. GenerationBatch khởi tạo đối tượng GenerationBatch()
11. GenerationBatch trả về dữ liệu cho BatchService
12. BatchService trả về batch cho batch_routes.py
13. batch_routes.py trả về kết quả cho DocAgentService
14. DocAgentService gọi hàm getBatchItemsByBatch()
15. DocAgentService gửi yêu cầu GET "/batch/batch-items/by-batch/{batch_id}" đến batch_routes.py
16. batch_routes.py gọi BatchService.get_batch_items_by_batch()
17. BatchService xử lý yêu cầu và gọi chính nó
18. BatchService gọi BatchItem
19. BatchItem khởi tạo đối tượng BatchItem()
20. BatchItem trả về dữ liệu cho BatchService
21. BatchService trả về danh sách batch items cho batch_routes.py
22. batch_routes.py trả về kết quả cho DocAgentService
23. Nếu batch không null, DocAgentService trả về kết quả cho QuestionApprovalTab.tsx
24. QuestionApprovalTab.tsx hiển thị kết quả batch cho User

---

## 2. Duyệt kết quả Batch

1. User click tab "Câu hỏi"
2. FlashcardTab.tsx khởi tạo và gọi FlashcardApprovalTab.tsx
3. FlashcardApprovalTab.tsx gọi hàm loadUserPendingBatch()
4. FlashcardApprovalTab.tsx hiển thị danh sách batch cho User
5. User chọn các bản ghi và click "Phê duyệt"
6. FlashcardApprovalTab.tsx gọi hàm handleApprovalBatch()
7. FlashcardApprovalTab.tsx gọi DocAgentService
8. DocAgentService gọi hàm approvedBatch()
9. DocAgentService gửi yêu cầu POST "/batch/approve" đến batch_routes.py
10. batch_routes.py gọi BatchService.approve_batch()
11. BatchService xử lý yêu cầu approve và gọi chính nó
12. BatchService gọi MessageResponse
13. MessageResponse khởi tạo đối tượng MessageResponse()
14. MessageResponse trả về thông báo cho BatchService
15. BatchService trả về kết quả cho batch_routes.py
16. batch_routes.py trả về thông báo thành công cho DocAgentService
17. DocAgentService trả về kết quả cho FlashcardTab.tsx
18. FlashcardTab.tsx hiển thị thông báo thành công cho User

---

## 3. Từ chối kết quả Batch

1. User click tab "Câu hỏi"
2. FlashcardTab.tsx khởi tạo và gọi FlashcardApprovalTab.tsx
3. FlashcardApprovalTab.tsx gọi hàm loadUserPendingBatch()
4. FlashcardApprovalTab.tsx hiển thị danh sách batch cho User
5. User chọn các bản ghi và click "Từ chối"
6. FlashcardApprovalTab.tsx gọi hàm handleRejectBatch()
7. FlashcardApprovalTab.tsx gọi DocAgentService
8. DocAgentService gọi hàm rejectBatch()
9. DocAgentService gửi yêu cầu POST "/batch/reject" đến batch_routes.py
10. batch_routes.py gọi BatchService.reject_batch()
11. BatchService xử lý yêu cầu reject và gọi chính nó
12. BatchService gọi MessageResponse
13. MessageResponse khởi tạo đối tượng MessageResponse()
14. MessageResponse trả về thông báo cho BatchService
15. BatchService trả về kết quả cho batch_routes.py
16. batch_routes.py trả về thông báo thành công cho DocAgentService
17. DocAgentService trả về kết quả cho FlashcardTab.tsx
18. FlashcardTab.tsx hiển thị thông báo cho User

---

## 4. Tạo yêu cầu regenerate

1. User click tab "Câu hỏi"
2. FlashcardTab.tsx khởi tạo và gọi FlashcardApprovalTab.tsx
3. FlashcardApprovalTab.tsx gọi hàm loadUserPendingBatch()
4. FlashcardApprovalTab.tsx hiển thị danh sách batch cho User
5. User nhập yêu cầu sau khi bị từ chối và click "Tạo lại"
6. FlashcardApprovalTab.tsx gọi hàm handleRegenerate()
7. FlashcardApprovalTab.tsx gọi hàm validate()
8. FlashcardApprovalTab.tsx gọi DocAgentService
9. DocAgentService gọi hàm createFlashcardProcess()
10. DocAgentService gửi yêu cầu POST "/gen/process/flashcard?queryConfigId&Req=queryConfig&Req" đến batch_routes.py
11. batch_routes.py gọi GenerationProcessService.create_flashcard_process()
12. GenerationProcessService xử lý yêu cầu và gọi chính nó
13. GenerationProcessService gọi ModelConfig
14. ModelConfig khởi tạo đối tượng ModelConfig()
15. ModelConfig trả về dữ liệu cho GenerationProcessService
16. GenerationProcessService gọi hàm create_process()
17. GenerationProcessService gọi GenerationProcess
18. GenerationProcess khởi tạo đối tượng GenerationProcess()
19. GenerationProcess trả về dữ liệu cho GenerationProcessService
20. GenerationProcessService gọi ModelUsage
21. ModelUsage khởi tạo đối tượng ModelUsage()
22. ModelUsage trả về dữ liệu cho GenerationProcessService
23. GenerationProcessService gọi QueryContext
24. QueryContext khởi tạo đối tượng QueryContext()
25. QueryContext trả về dữ liệu cho GenerationProcessService
26. GenerationProcessService gọi hàm create_steps()
27. GenerationProcessService gọi GenerationProcessStep
28. GenerationProcessStep khởi tạo đối tượng GenerationProcessStep()
29. GenerationProcessStep trả về dữ liệu cho GenerationProcessService
30. GenerationProcessService trả về process đã tạo cho batch_routes.py
31. batch_routes.py trả về kết quả cho DocAgentService
32. DocAgentService trả về process cho FlashcardTab.tsx
33. FlashcardTab.tsx hiển thị thông báo cho User

---

## 5. Hủy tiến trình Generation

1. User click "Hủy tiến trình" và xác nhận
2. LiveProgress.tsx gọi hàm handleCancelProcess()
3. LiveProgress.tsx gọi DocAgentService
4. DocAgentService gọi hàm cancelProcess()
5. DocAgentService gửi yêu cầu POST "/process/{process_id}/cancel" đến gen_routes.py
6. gen_routes.py gọi GenerationProcessService.cancel_process()
7. GenerationProcessService gọi hàm cancel_process()
8. GenerationProcessService xử lý yêu cầu và gọi chính nó
9. GenerationProcessService gọi MessageResponse
10. MessageResponse khởi tạo đối tượng MessageResponse()
11. MessageResponse trả về thông báo cho GenerationProcessService
12. GenerationProcessService trả về kết quả cho gen_routes.py
13. gen_routes.py trả về thông báo thành công cho DocAgentService
14. DocAgentService trả về kết quả cho LiveProgress.tsx
15. LiveProgress.tsx hiển thị thông báo cho User

---

## 6. Tạo yêu cầu Generation

1. User nhập thông tin và click "Tạo"
2. QuestionGenDialog.tsx gọi hàm handleGenerate()
3. QuestionGenDialog.tsx gọi hàm validate()
4. QuestionGenDialog.tsx gọi DocAgentService
5. DocAgentService gọi hàm createFlashcardProcess()
6. DocAgentService gửi yêu cầu POST "/gen/process/flashcard?queryConfigId&Req=queryConfig&Req" đến gen_routes.py
7. gen_routes.py gọi GenerationProcessService.create_flashcard_process()
8. GenerationProcessService xử lý yêu cầu và gọi chính nó
9. GenerationProcessService gọi ModelConfigService.get_model_by_team()
10. ModelConfigService xử lý yêu cầu và gọi chính nó
11. ModelConfigService gọi ModelConfig
12. ModelConfig khởi tạo đối tượng ModelConfig()
13. ModelConfig trả về dữ liệu cho ModelConfigService
14. ModelConfigService trả về model config cho GenerationProcessService
15. GenerationProcessService gọi hàm create_process()
16. GenerationProcessService gọi GenerationProcess
17. GenerationProcess khởi tạo đối tượng GenerationProcess()
18. GenerationProcess trả về dữ liệu cho GenerationProcessService
19. GenerationProcessService gọi ModelUsage
20. ModelUsage khởi tạo đối tượng ModelUsage()
21. ModelUsage trả về dữ liệu cho GenerationProcessService
22. GenerationProcessService gọi QueryContext
23. QueryContext khởi tạo đối tượng QueryContext()
24. QueryContext trả về dữ liệu cho GenerationProcessService
25. GenerationProcessService gọi hàm create_steps()
26. GenerationProcessService gọi GenerationProcessStep
27. GenerationProcessStep khởi tạo đối tượng GenerationProcessStep()
28. GenerationProcessStep trả về dữ liệu cho GenerationProcessService
29. GenerationProcessService trả về process đã tạo cho gen_routes.py
30. gen_routes.py trả về kết quả cho DocAgentService
31. DocAgentService hiển thị kết quả cho User

---

## 7. Theo dõi tiến trình Generation - Khởi tạo

1. User theo dõi tiến trình
2. LiveProgress.tsx gọi DocSocketService
3. DocSocketService gọi GenerationPipelineService
4. GenerationPipelineService gọi hàm execute_pipeline()
5. GenerationPipelineService gọi hàm step_query_preprocessing()
6. GenerationPipelineService gọi WsGenerationManager
7. WsGenerationManager gọi hàm create()
8. WsGenerationManager trả về cho GenerationPipelineService
9. GenerationPipelineService gọi EmbeddingFactory
10. EmbeddingFactory gọi BaseEmbedding
11. BaseEmbedding khởi tạo đối tượng BaseEmbedding()
12. BaseEmbedding trả về cho EmbeddingFactory
13. EmbeddingFactory trả về cho GenerationPipelineService
14. GenerationPipelineService gọi QueryDecoder
15. QueryDecoder gọi hàm route_query()
16. QueryDecoder trả về cho GenerationPipelineService
17. GenerationPipelineService gọi hàm create_embeddings()
18. GenerationPipelineService gọi QueryConstructor
19. QueryConstructor gọi hàm generate_multi_query()
20. QueryConstructor trả về cho GenerationPipelineService
21. Nếu DataMap == database, GenerationPipelineService gọi hàm do.composite_query()
22. GenerationPipelineService gọi hàm update_step()
23. GenerationPipelineService gọi GenerationProcessStep
24. GenerationProcessStep khởi tạo đối tượng GenerationProcessStep()
25. GenerationProcessStep trả về cho GenerationPipelineService
26. GenerationPipelineService gọi WsGenerationManager
27. WsGenerationManager gọi hàm send_state()
28. WsGenerationManager gọi WsEnvelope
29. WsEnvelope khởi tạo đối tượng WsEnvelope()
30. WsEnvelope trả về cho WsGenerationManager
31. WsGenerationManager trả về cho LiveProgress.tsx
32. WsGenerationManager broadcast thông tin
33. LiveProgress.tsx hiển thị cho User

---

## 8. Theo dõi tiến trình Generation - Retrieval

1. User theo dõi tiến trình
2. LiveProgress.tsx gọi DocSocketService
3. DocSocketService gọi GenerationPipelineService
4. GenerationPipelineService gọi hàm execute_pipeline()
5. GenerationPipelineService gọi hàm step_query_preprocessing()
6. GenerationPipelineService gọi WsGenerationManager
7. WsGenerationManager gọi hàm create()
8. WsGenerationManager trả về cho GenerationPipelineService
9. GenerationPipelineService gọi EmbeddingFactory
10. EmbeddingFactory gọi BaseEmbedding
11. BaseEmbedding khởi tạo đối tượng BaseEmbedding()
12. BaseEmbedding trả về cho EmbeddingFactory
13. EmbeddingFactory trả về cho GenerationPipelineService
14. GenerationPipelineService gọi QueryConstructor
15. QueryConstructor gọi hàm create_embeddings()
16. QueryConstructor trả về cho GenerationPipelineService
17. GenerationPipelineService gọi hàm route_query()
18. GenerationPipelineService gọi hàm create_embeddings()
19. GenerationPipelineService gọi Retriever
20. Retriever gọi hàm similarity_search()
21. Retriever trả về cho GenerationPipelineService
22. GenerationPipelineService gọi hàm re_hybrid_rerank()
23. GenerationPipelineService gọi hàm update_step()
24. GenerationPipelineService gọi GenerationProcessStep
25. GenerationProcessStep khởi tạo đối tượng GenerationProcessStep()
26. GenerationProcessStep trả về cho GenerationPipelineService
27. GenerationPipelineService gọi WsGenerationManager
28. WsGenerationManager gọi hàm send_state()
29. WsGenerationManager gọi WsEnvelope
30. WsEnvelope khởi tạo đối tượng WsEnvelope()
31. WsEnvelope trả về cho WsGenerationManager
32. WsGenerationManager trả về cho LiveProgress.tsx
33. WsGenerationManager broadcast thông tin
34. LiveProgress.tsx hiển thị cho User

---

## 9. Theo dõi tiến trình Generation - Duplicate Prevention

1. User theo dõi tiến trình
2. LiveProgress.tsx gọi DocSocketService
3. DocSocketService gọi GenerationPipelineService
4. GenerationPipelineService gọi hàm execute_pipeline()
5. GenerationPipelineService gọi hàm step_duplicate_preventation()
6. GenerationPipelineService gọi WsGenerationManager
7. WsGenerationManager gọi hàm find_similar_processed()
8. WsGenerationManager trả về cho GenerationPipelineService
9. GenerationPipelineService gọi BatchService
10. BatchService gọi hàm get_batch_items_by_processed()
11. BatchService trả về cho GenerationPipelineService
12. GenerationPipelineService gọi QuerySimilarProcessor
13. QuerySimilarProcessor gọi hàm find()
14. QuerySimilarProcessor trả về cho GenerationPipelineService
15. Nếu DataMap == database, GenerationPipelineService gọi hàm get_batch_items_by_processed()
16. GenerationPipelineService gọi hàm update_step()
17. GenerationPipelineService gọi GenerationProcessStep
18. GenerationProcessStep khởi tạo đối tượng GenerationProcessStep()
19. GenerationProcessStep trả về cho GenerationPipelineService
20. GenerationPipelineService gọi WsGenerationManager
21. WsGenerationManager gọi hàm send_state()
22. WsGenerationManager gọi WsEnvelope
23. WsEnvelope khởi tạo đối tượng WsEnvelope()
24. WsEnvelope trả về cho WsGenerationManager
25. WsGenerationManager trả về cho LiveProgress.tsx
26. WsGenerationManager broadcast thông tin
27. LiveProgress.tsx hiển thị cho User

---

## 10. Theo dõi tiến trình Generation - Generation

1. User theo dõi tiến trình
2. LiveProgress.tsx gọi DocSocketService
3. DocSocketService gọi GenerationPipelineService
4. GenerationPipelineService gọi hàm execute_pipeline()
5. GenerationPipelineService gọi hàm step_generation()
6. GenerationPipelineService gọi WsGenerationManager
7. WsGenerationManager gọi hàm get_batch_items_by_data()
8. WsGenerationManager trả về cho GenerationPipelineService
9. GenerationPipelineService gọi PromptBuilder
10. PromptBuilder gọi hàm build_element_prompt()
11. PromptBuilder trả về cho GenerationPipelineService
12. GenerationPipelineService gọi PromptInputor
13. PromptInputor gọi hàm build_element_prompt()
14. PromptInputor trả về cho GenerationPipelineService
15. GenerationPipelineService gọi Generator
16. Generator gọi hàm validate_for_generation()
17. Generator gọi hàm build_example_prompt()
18. Generator gọi hàm hybrid_rerank()
19. Generator gọi hàm generate_text()
20. Generator trả về cho GenerationPipelineService
21. GenerationPipelineService gọi hàm regenerate_items()
22. GenerationPipelineService gọi BulkItem
23. BulkItem khởi tạo đối tượng BulkItem()
24. BulkItem trả về cho GenerationPipelineService
25. GenerationPipelineService gọi hàm update_step()
26. GenerationPipelineService gọi GenerationProcessStep
27. GenerationProcessStep khởi tạo đối tượng GenerationProcessStep()
28. GenerationProcessStep trả về cho GenerationPipelineService
29. GenerationPipelineService gọi WsGenerationManager
30. WsGenerationManager gọi hàm send_state()
31. WsGenerationManager gọi WsEnvelope
32. WsEnvelope khởi tạo đối tượng WsEnvelope()
33. WsEnvelope trả về cho WsGenerationManager
34. WsGenerationManager trả về cho LiveProgress.tsx
35. WsGenerationManager broadcast thông tin
36. LiveProgress.tsx hiển thị cho User
