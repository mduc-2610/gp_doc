# Sequence Diagram - Generation với Batch

## 1. Xem kết quả Batch

```mermaid
sequenceDiagram
    actor User
    participant QuestionTab as QuestionTab.tsx
    participant QuestionApprovalTab as QuestionApprovalTab.tsx
    participant DocAgentService as DocAgentService
    participant BatchRoutes as batch_routes.py
    participant BatchService as BatchService
    participant GenerationBatch as GenerationBatch
    participant BatchItem as BatchItem

    User->>QuestionTab: Click "Câu hỏi" tab
    activate QuestionTab
    
    QuestionTab->>QuestionApprovalTab: gọi
    activate QuestionApprovalTab
    
    QuestionApprovalTab->>QuestionApprovalTab: loadUserPendingBatch()
    QuestionApprovalTab->>DocAgentService: gọi
    deactivate QuestionApprovalTab
    activate DocAgentService
    
    DocAgentService->>DocAgentService: getUserPendingBatch()
    DocAgentService->>BatchRoutes: GET "/batch/pending-batch?user_id&session_id&result_type"
    activate BatchRoutes
    
    BatchRoutes->>BatchService: get_user_pending_batch()
    activate BatchService
    
    BatchService->>BatchService: gọi
    BatchService->>GenerationBatch: gọi
    activate GenerationBatch
    
    GenerationBatch->>GenerationBatch: GenerationBatch()
    GenerationBatch-->>BatchService: trả về
    deactivate GenerationBatch
    
    BatchService-->>BatchRoutes: trả về
    deactivate BatchService
    
    BatchRoutes-->>DocAgentService: trả về
    deactivate BatchRoutes
    
    DocAgentService->>DocAgentService: getBatchItemsByBatch()
    DocAgentService->>BatchRoutes: GET "/batch/batch-items/by-batch/{batch_id}"
    activate BatchRoutes
    
    BatchRoutes->>BatchService: get_batch_items_by_batch()
    activate BatchService
    
    BatchService->>BatchService: gọi
    BatchService->>BatchItem: gọi
    activate BatchItem
    
    BatchItem->>BatchItem: BatchItem()
    BatchItem-->>BatchService: trả về
    deactivate BatchItem
    
    BatchService-->>BatchRoutes: trả về
    deactivate BatchService
    
    BatchRoutes-->>DocAgentService: trả về
    deactivate BatchRoutes
    
    alt batch != null
        DocAgentService-->>QuestionApprovalTab: trả về
    end
    deactivate DocAgentService
    
    QuestionApprovalTab-->>User: Hiển thị
    deactivate QuestionTab
```

---

## 2. Duyệt kết quả Batch

```mermaid
sequenceDiagram
    actor User
    participant FlashcardTab as FlashcardTab.tsx
    participant FlashcardApprovalTab as FlashcardApprovalTab.tsx
    participant DocAgentService as DocAgentService
    participant BatchRoutes as batch_routes.py
    participant BatchService as BatchService
    participant GenerationBatch as GenerationBatch
    participant MessageResponse as MessageResponse

    User->>FlashcardTab: Click "Câu hỏi" tab
    activate FlashcardTab
    
    FlashcardTab->>FlashcardApprovalTab: gọi
    activate FlashcardApprovalTab
    
    FlashcardApprovalTab->>FlashcardApprovalTab: loadUserPendingBatch()
    FlashcardApprovalTab-->>User: Hiển thị
    deactivate FlashcardApprovalTab
    
    User->>FlashcardApprovalTab: Chọn các bản ghi và click "Phê duyệt"
    activate FlashcardApprovalTab
    
    FlashcardApprovalTab->>FlashcardApprovalTab: handleApprovalBatch()
    FlashcardApprovalTab->>DocAgentService: gọi
    deactivate FlashcardApprovalTab
    activate DocAgentService
    
    DocAgentService->>DocAgentService: approvedBatch()
    DocAgentService->>BatchRoutes: POST "/batch/approve"
    activate BatchRoutes
    
    BatchRoutes->>BatchService: approve_batch()
    activate BatchService
    
    BatchService->>BatchService: gọi
    BatchService->>MessageResponse: gọi
    activate MessageResponse
    
    MessageResponse->>MessageResponse: MessageResponse()
    MessageResponse-->>BatchService: trả về
    deactivate MessageResponse
    
    BatchService-->>BatchRoutes: trả về
    deactivate BatchService
    
    BatchRoutes-->>DocAgentService: trả về
    deactivate BatchRoutes
    
    DocAgentService-->>FlashcardTab: trả về
    deactivate DocAgentService
    
    FlashcardTab-->>User: Hiển thị
    deactivate FlashcardTab
```

---

## 3. Từ chối kết quả Batch

```mermaid
sequenceDiagram
    actor User
    participant FlashcardTab as FlashcardTab.tsx
    participant FlashcardApprovalTab as FlashcardApprovalTab.tsx
    participant DocAgentService as DocAgentService
    participant BatchRoutes as batch_routes.py
    participant BatchService as BatchService
    participant MessageResponse as MessageResponse

    User->>FlashcardTab: Click "Câu hỏi" tab
    activate FlashcardTab
    
    FlashcardTab->>FlashcardApprovalTab: gọi
    activate FlashcardApprovalTab
    
    FlashcardApprovalTab->>FlashcardApprovalTab: loadUserPendingBatch()
    FlashcardApprovalTab-->>User: Hiển thị
    deactivate FlashcardApprovalTab
    
    User->>FlashcardApprovalTab: Chọn và click "Từ chối"
    activate FlashcardApprovalTab
    
    FlashcardApprovalTab->>FlashcardApprovalTab: handleRejectBatch()
    FlashcardApprovalTab->>DocAgentService: gọi
    deactivate FlashcardApprovalTab
    activate DocAgentService
    
    DocAgentService->>DocAgentService: rejectBatch()
    DocAgentService->>BatchRoutes: POST "/batch/reject"
    activate BatchRoutes
    
    BatchRoutes->>BatchService: reject_batch()
    activate BatchService
    
    BatchService->>BatchService: gọi
    BatchService->>MessageResponse: gọi
    activate MessageResponse
    
    MessageResponse->>MessageResponse: MessageResponse()
    MessageResponse-->>BatchService: trả về
    deactivate MessageResponse
    
    BatchService-->>BatchRoutes: trả về
    deactivate BatchService
    
    BatchRoutes-->>DocAgentService: trả về
    deactivate BatchRoutes
    
    DocAgentService-->>FlashcardTab: trả về
    deactivate DocAgentService
    
    FlashcardTab-->>User: Hiển thị
    deactivate FlashcardTab
```

---

## 4. Tạo yêu cầu regenerate

```mermaid
sequenceDiagram
    actor User
    participant FlashcardTab as FlashcardTab.tsx
    participant FlashcardApprovalTab as FlashcardApprovalTab.tsx
    participant DocAgentService as DocAgentService
    participant BatchRoutes as batch_routes.py
    participant GenerationProcessService as GenerationProcessService
    participant GenerationProcess as GenerationProcess
    participant ModelConfig as ModelConfig
    participant ModelUsage as ModelUsage
    participant QueryContext as QueryContext
    participant GenerationProcessStep as GenerationProcessStep

    User->>FlashcardTab: Click "Câu hỏi" tab
    activate FlashcardTab
    
    FlashcardTab->>FlashcardApprovalTab: gọi
    activate FlashcardApprovalTab
    
    FlashcardApprovalTab->>FlashcardApprovalTab: loadUserPendingBatch()
    FlashcardApprovalTab-->>User: Hiển thị
    deactivate FlashcardApprovalTab
    
    User->>FlashcardApprovalTab: Nhập yêu cầu sau khi bị từ chối và click "Tạo lại"
    activate FlashcardApprovalTab
    
    FlashcardApprovalTab->>FlashcardApprovalTab: handleRegenerate()
    FlashcardApprovalTab->>FlashcardApprovalTab: validate()
    FlashcardApprovalTab->>DocAgentService: gọi
    deactivate FlashcardApprovalTab
    activate DocAgentService
    
    DocAgentService->>DocAgentService: createFlashcardProcess()
    DocAgentService->>BatchRoutes: POST "/gen/process/flashcard?queryConfigId&Req=queryConfig&Req"
    activate BatchRoutes
    
    BatchRoutes->>GenerationProcessService: create_flashcard_process()
    activate GenerationProcessService
    
    GenerationProcessService->>GenerationProcessService: gọi
    GenerationProcessService->>ModelConfig: gọi
    activate ModelConfig
    
    ModelConfig->>ModelConfig: ModelConfig()
    ModelConfig-->>GenerationProcessService: trả về
    deactivate ModelConfig
    
    GenerationProcessService->>GenerationProcessService: create_process()
    GenerationProcessService->>GenerationProcess: gọi
    activate GenerationProcess
    
    GenerationProcess->>GenerationProcess: GenerationProcess()
    GenerationProcess-->>GenerationProcessService: trả về
    deactivate GenerationProcess
    
    GenerationProcessService->>ModelUsage: gọi
    activate ModelUsage
    
    ModelUsage->>ModelUsage: ModelUsage()
    ModelUsage-->>GenerationProcessService: trả về
    deactivate ModelUsage
    
    GenerationProcessService->>QueryContext: gọi
    activate QueryContext
    
    QueryContext->>QueryContext: QueryContext()
    QueryContext-->>GenerationProcessService: trả về
    deactivate QueryContext
    
    GenerationProcessService->>GenerationProcessService: create_steps()
    GenerationProcessService->>GenerationProcessStep: gọi
    activate GenerationProcessStep
    
    GenerationProcessStep->>GenerationProcessStep: GenerationProcessStep()
    GenerationProcessStep-->>GenerationProcessService: trả về
    deactivate GenerationProcessStep
    
    GenerationProcessService-->>BatchRoutes: trả về
    deactivate GenerationProcessService
    
    BatchRoutes-->>DocAgentService: trả về
    deactivate BatchRoutes
    
    DocAgentService-->>FlashcardTab: trả về
    deactivate DocAgentService
    
    FlashcardTab-->>User: Hiển thị
    deactivate FlashcardTab
```

---

## 5. Hủy tiến trình Generation

```mermaid
sequenceDiagram
    actor User
    participant LiveProgress as LiveProgress.tsx
    participant DocAgentService as DocAgentService
    participant GenRoutes as gen_routes.py
    participant GenerationProcessService as GenerationProcessService
    participant MessageResponse as MessageResponse

    User->>LiveProgress: Click "Hủy tiến trình" và xác nhận
    activate LiveProgress
    
    LiveProgress->>LiveProgress: handleCancelProcess()
    LiveProgress->>DocAgentService: gọi
    deactivate LiveProgress
    activate DocAgentService
    
    DocAgentService->>DocAgentService: cancelProcess()
    DocAgentService->>GenRoutes: POST "/process/{process_id}/cancel"
    activate GenRoutes
    
    GenRoutes->>GenerationProcessService: cancel_process()
    activate GenerationProcessService
    
    GenerationProcessService->>GenerationProcessService: cancel_process()
    GenerationProcessService->>GenerationProcessService: gọi
    GenerationProcessService->>MessageResponse: gọi
    activate MessageResponse
    
    MessageResponse->>MessageResponse: MessageResponse()
    MessageResponse-->>GenerationProcessService: trả về
    deactivate MessageResponse
    
    GenerationProcessService-->>GenRoutes: trả về
    deactivate GenerationProcessService
    
    GenRoutes-->>DocAgentService: trả về
    deactivate GenRoutes
    
    DocAgentService-->>LiveProgress: trả về
    deactivate DocAgentService
    
    LiveProgress-->>User: Hiển thị
```

---

## 6. Tạo yêu cầu Generation

```mermaid
sequenceDiagram
    actor User
    participant QuestionGenDialog as QuestionGenDialog.tsx
    participant DocAgentService as DocAgentService
    participant GenRoutes as gen_routes.py
    participant GenerationProcessService as GenerationProcessService
    participant ModelConfigService as ModelConfigService
    participant GenerationProcess as GenerationProcess
    participant ModelConfig as ModelConfig
    participant ModelUsage as ModelUsage
    participant QueryContext as QueryContext
    participant GenerationProcessStep as GenerationProcessStep

    User->>QuestionGenDialog: Nhập thông tin và click "Tạo"
    activate QuestionGenDialog
    
    QuestionGenDialog->>QuestionGenDialog: handleGenerate()
    QuestionGenDialog->>QuestionGenDialog: validate()
    QuestionGenDialog->>DocAgentService: gọi
    deactivate QuestionGenDialog
    activate DocAgentService
    
    DocAgentService->>DocAgentService: createFlashcardProcess()
    DocAgentService->>GenRoutes: POST "/gen/process/flashcard?queryConfigId&Req=queryConfig&Req"
    activate GenRoutes
    
    GenRoutes->>GenerationProcessService: create_flashcard_process()
    activate GenerationProcessService
    
    GenerationProcessService->>GenerationProcessService: gọi
    GenerationProcessService->>ModelConfigService: get_model_by_team()
    activate ModelConfigService
    
    ModelConfigService->>ModelConfigService: gọi
    ModelConfigService->>ModelConfig: gọi
    activate ModelConfig
    
    ModelConfig->>ModelConfig: ModelConfig()
    ModelConfig-->>ModelConfigService: trả về
    deactivate ModelConfig
    
    ModelConfigService-->>GenerationProcessService: trả về
    deactivate ModelConfigService
    
    GenerationProcessService->>GenerationProcessService: create_process()
    GenerationProcessService->>GenerationProcess: gọi
    activate GenerationProcess
    
    GenerationProcess->>GenerationProcess: GenerationProcess()
    GenerationProcess-->>GenerationProcessService: trả về
    deactivate GenerationProcess
    
    GenerationProcessService->>ModelUsage: gọi
    activate ModelUsage
    
    ModelUsage->>ModelUsage: ModelUsage()
    ModelUsage-->>GenerationProcessService: trả về
    deactivate ModelUsage
    
    GenerationProcessService->>QueryContext: gọi
    activate QueryContext
    
    QueryContext->>QueryContext: QueryContext()
    QueryContext-->>GenerationProcessService: trả về
    deactivate QueryContext
    
    GenerationProcessService->>GenerationProcessService: create_steps()
    GenerationProcessService->>GenerationProcessStep: gọi
    activate GenerationProcessStep
    
    GenerationProcessStep->>GenerationProcessStep: GenerationProcessStep()
    GenerationProcessStep-->>GenerationProcessService: trả về
    deactivate GenerationProcessStep
    
    GenerationProcessService-->>GenRoutes: trả về
    deactivate GenerationProcessService
    
    GenRoutes-->>DocAgentService: trả về
    deactivate GenRoutes
    
    DocAgentService-->>User: Hiển thị
    deactivate DocAgentService
```

---

## 7. Theo dõi tiến trình Generation - Khởi tạo

```mermaid
sequenceDiagram
    actor User
    participant LiveProgress as LiveProgress.tsx
    participant DocSocketService as DocSocketService
    participant GenerationPipelineService as GenerationPipelineService
    participant WsGenerationManager as WsGenerationManager
    participant EmbeddingFactory as EmbeddingFactory
    participant BaseEmbedding as BaseEmbedding
    participant QueryDecoder as QueryDecoder
    participant QueryConstructor as QueryConstructor
    participant Retriever as Retriever
    participant QuerySimilarProcessor as QuerySimilarProcessor
    participant BatchItem as BatchItem
    participant Generator as Generator
    participant GenerationProcessStep as GenerationProcessStep
    participant WsEnvelope as WsEnvelope

    User->>LiveProgress: Theo dõi tiến trình
    activate LiveProgress
    
    LiveProgress->>DocSocketService: gọi
    activate DocSocketService
    
    DocSocketService->>GenerationPipelineService: gọi
    activate GenerationPipelineService
    
    GenerationPipelineService->>GenerationPipelineService: execute_pipeline()
    GenerationPipelineService->>GenerationPipelineService: step_query_preprocessing()
    GenerationPipelineService->>WsGenerationManager: gọi
    activate WsGenerationManager
    
    WsGenerationManager->>WsGenerationManager: create()
    WsGenerationManager-->>GenerationPipelineService: trả về
    deactivate WsGenerationManager
    
    GenerationPipelineService->>EmbeddingFactory: gọi
    activate EmbeddingFactory
    
    EmbeddingFactory->>BaseEmbedding: gọi
    activate BaseEmbedding
    
    BaseEmbedding->>BaseEmbedding: BaseEmbedding()
    BaseEmbedding-->>EmbeddingFactory: trả về
    deactivate BaseEmbedding
    
    EmbeddingFactory-->>GenerationPipelineService: trả về
    deactivate EmbeddingFactory
    
    GenerationPipelineService->>QueryDecoder: gọi
    activate QueryDecoder
    
    QueryDecoder->>QueryDecoder: route_query()
    QueryDecoder-->>GenerationPipelineService: trả về
    deactivate QueryDecoder
    
    GenerationPipelineService->>GenerationPipelineService: create_embeddings()
    GenerationPipelineService->>QueryConstructor: gọi
    activate QueryConstructor
    
    QueryConstructor->>QueryConstructor: generate_multi_query()
    QueryConstructor-->>GenerationPipelineService: trả về
    deactivate QueryConstructor
    
    alt DataMap=={database}
        GenerationPipelineService->>GenerationPipelineService: do.composite_query()
    end
    
    GenerationPipelineService->>GenerationPipelineService: update_step()
    GenerationPipelineService->>GenerationProcessStep: gọi
    activate GenerationProcessStep
    
    GenerationProcessStep->>GenerationProcessStep: GenerationProcessStep()
    GenerationProcessStep-->>GenerationPipelineService: trả về
    deactivate GenerationProcessStep
    
    GenerationPipelineService->>WsGenerationManager: gọi
    activate WsGenerationManager
    
    WsGenerationManager->>WsGenerationManager: send_state()
    WsGenerationManager->>WsEnvelope: gọi
    activate WsEnvelope
    
    WsEnvelope->>WsEnvelope: WsEnvelope()
    WsEnvelope-->>WsGenerationManager: trả về
    deactivate WsEnvelope
    
    WsGenerationManager-->>LiveProgress: trả về
    deactivate WsGenerationManager
    
    WsGenerationManager->>WsGenerationManager: broadcast
    LiveProgress-->>User: Hiển thị
    deactivate GenerationPipelineService
    deactivate DocSocketService
    deactivate LiveProgress
```

---

## 8. Theo dõi tiến trình Generation - Retrieval

```mermaid
sequenceDiagram
    actor User
    participant LiveProgress as LiveProgress.tsx
    participant DocSocketService as DocSocketService
    participant GenerationPipelineService as GenerationPipelineService
    participant WsGenerationManager as WsGenerationManager
    participant EmbeddingFactory as EmbeddingFactory
    participant BaseEmbedding as BaseEmbedding
    participant QueryConstructor as QueryConstructor
    participant Retriever as Retriever
    participant GenerationProcessStep as GenerationProcessStep
    participant WsEnvelope as WsEnvelope

    User->>LiveProgress: Theo dõi tiến trình
    activate LiveProgress
    
    LiveProgress->>DocSocketService: gọi
    activate DocSocketService
    
    DocSocketService->>GenerationPipelineService: gọi
    activate GenerationPipelineService
    
    GenerationPipelineService->>GenerationPipelineService: execute_pipeline()
    GenerationPipelineService->>GenerationPipelineService: step_query_preprocessing()
    GenerationPipelineService->>WsGenerationManager: gọi
    activate WsGenerationManager
    
    WsGenerationManager->>WsGenerationManager: create()
    WsGenerationManager-->>GenerationPipelineService: trả về
    deactivate WsGenerationManager
    
    GenerationPipelineService->>EmbeddingFactory: gọi
    activate EmbeddingFactory
    
    EmbeddingFactory->>BaseEmbedding: gọi
    activate BaseEmbedding
    
    BaseEmbedding->>BaseEmbedding: BaseEmbedding()
    BaseEmbedding-->>EmbeddingFactory: trả về
    deactivate BaseEmbedding
    
    EmbeddingFactory-->>GenerationPipelineService: trả về
    deactivate EmbeddingFactory
    
    GenerationPipelineService->>QueryConstructor: gọi
    activate QueryConstructor
    
    QueryConstructor->>QueryConstructor: create_embeddings()
    QueryConstructor-->>GenerationPipelineService: trả về
    deactivate QueryConstructor
    
    GenerationPipelineService->>GenerationPipelineService: route_query()
    GenerationPipelineService->>GenerationPipelineService: create_embeddings()
    GenerationPipelineService->>Retriever: gọi
    activate Retriever
    
    Retriever->>Retriever: similarity_search()
    Retriever-->>GenerationPipelineService: trả về
    deactivate Retriever
    
    GenerationPipelineService->>GenerationPipelineService: re_hybrid_rerank()
    GenerationPipelineService->>GenerationPipelineService: update_step()
    GenerationPipelineService->>GenerationProcessStep: gọi
    activate GenerationProcessStep
    
    GenerationProcessStep->>GenerationProcessStep: GenerationProcessStep()
    GenerationProcessStep-->>GenerationPipelineService: trả về
    deactivate GenerationProcessStep
    
    GenerationPipelineService->>WsGenerationManager: gọi
    activate WsGenerationManager
    
    WsGenerationManager->>WsGenerationManager: send_state()
    WsGenerationManager->>WsEnvelope: gọi
    activate WsEnvelope
    
    WsEnvelope->>WsEnvelope: WsEnvelope()
    WsEnvelope-->>WsGenerationManager: trả về
    deactivate WsEnvelope
    
    WsGenerationManager-->>LiveProgress: trả về
    deactivate WsGenerationManager
    
    WsGenerationManager->>WsGenerationManager: broadcast
    LiveProgress-->>User: Hiển thị
    deactivate GenerationPipelineService
    deactivate DocSocketService
    deactivate LiveProgress
```

---

## 9. Theo dõi tiến trình Generation - Duplicate Prevention

```mermaid
sequenceDiagram
    actor User
    participant LiveProgress as LiveProgress.tsx
    participant DocSocketService as DocSocketService
    participant GenerationPipelineService as GenerationPipelineService
    participant WsGenerationManager as WsGenerationManager
    participant BatchService as BatchService
    participant QuerySimilarProcessor as QuerySimilarProcessor
    participant GenerationProcessStep as GenerationProcessStep
    participant WsEnvelope as WsEnvelope

    User->>LiveProgress: Theo dõi tiến trình
    activate LiveProgress
    
    LiveProgress->>DocSocketService: gọi
    activate DocSocketService
    
    DocSocketService->>GenerationPipelineService: gọi
    activate GenerationPipelineService
    
    GenerationPipelineService->>GenerationPipelineService: execute_pipeline()
    GenerationPipelineService->>GenerationPipelineService: step_duplicate_preventation()
    GenerationPipelineService->>WsGenerationManager: gọi
    activate WsGenerationManager
    
    WsGenerationManager->>WsGenerationManager: find_similar_processed()
    WsGenerationManager-->>GenerationPipelineService: trả về
    deactivate WsGenerationManager
    
    GenerationPipelineService->>BatchService: gọi
    activate BatchService
    
    BatchService->>BatchService: get_batch_items_by_processed()
    BatchService-->>GenerationPipelineService: trả về
    deactivate BatchService
    
    GenerationPipelineService->>QuerySimilarProcessor: gọi
    activate QuerySimilarProcessor
    
    QuerySimilarProcessor->>QuerySimilarProcessor: find()
    QuerySimilarProcessor-->>GenerationPipelineService: trả về
    deactivate QuerySimilarProcessor
    
    alt (DataMap=={database})
        GenerationPipelineService->>GenerationPipelineService: get_batch_items_by_processed()
    end
    
    GenerationPipelineService->>GenerationPipelineService: update_step()
    GenerationPipelineService->>GenerationProcessStep: gọi
    activate GenerationProcessStep
    
    GenerationProcessStep->>GenerationProcessStep: GenerationProcessStep()
    GenerationProcessStep-->>GenerationPipelineService: trả về
    deactivate GenerationProcessStep
    
    GenerationPipelineService->>WsGenerationManager: gọi
    activate WsGenerationManager
    
    WsGenerationManager->>WsGenerationManager: send_state()
    WsGenerationManager->>WsEnvelope: gọi
    activate WsEnvelope
    
    WsEnvelope->>WsEnvelope: WsEnvelope()
    WsEnvelope-->>WsGenerationManager: trả về
    deactivate WsEnvelope
    
    WsGenerationManager-->>LiveProgress: trả về
    deactivate WsGenerationManager
    
    WsGenerationManager->>WsGenerationManager: broadcast
    LiveProgress-->>User: Hiển thị
    deactivate GenerationPipelineService
    deactivate DocSocketService
    deactivate LiveProgress
```

---

## 10. Theo dõi tiến trình Generation - Generation

```mermaid
sequenceDiagram
    actor User
    participant LiveProgress as LiveProgress.tsx
    participant DocSocketService as DocSocketService
    participant GenerationPipelineService as GenerationPipelineService
    participant WsGenerationManager as WsGenerationManager
    participant PromptBuilder as PromptBuilder
    participant PromptInputor as PromptInputor
    participant Generator as Generator
    participant BulkItem as BulkItem
    participant GenerationProcessStep as GenerationProcessStep
    participant WsEnvelope as WsEnvelope

    User->>LiveProgress: Theo dõi tiến trình
    activate LiveProgress
    
    LiveProgress->>DocSocketService: gọi
    activate DocSocketService
    
    DocSocketService->>GenerationPipelineService: gọi
    activate GenerationPipelineService
    
    GenerationPipelineService->>GenerationPipelineService: execute_pipeline()
    GenerationPipelineService->>GenerationPipelineService: step_generation()
    GenerationPipelineService->>WsGenerationManager: gọi
    activate WsGenerationManager
    
    WsGenerationManager->>WsGenerationManager: get_batch_items_by_data()
    WsGenerationManager-->>GenerationPipelineService: trả về
    deactivate WsGenerationManager
    
    GenerationPipelineService->>PromptBuilder: gọi
    activate PromptBuilder
    
    PromptBuilder->>PromptBuilder: build_element_prompt()
    PromptBuilder-->>GenerationPipelineService: trả về
    deactivate PromptBuilder
    
    GenerationPipelineService->>PromptInputor: gọi
    activate PromptInputor
    
    PromptInputor->>PromptInputor: build_element_prompt()
    PromptInputor-->>GenerationPipelineService: trả về
    deactivate PromptInputor
    
    GenerationPipelineService->>Generator: gọi
    activate Generator
    
    Generator->>Generator: validate_for_generation()
    Generator->>Generator: build_example_prompt()
    Generator->>Generator: hybrid_rerank()
    Generator->>Generator: generate_text()
    Generator-->>GenerationPipelineService: trả về
    deactivate Generator
    
    GenerationPipelineService->>GenerationPipelineService: regenerate_items()
    GenerationPipelineService->>BulkItem: gọi
    activate BulkItem
    
    BulkItem->>BulkItem: BulkItem()
    BulkItem-->>GenerationPipelineService: trả về
    deactivate BulkItem
    
    GenerationPipelineService->>GenerationPipelineService: update_step()
    GenerationPipelineService->>GenerationProcessStep: gọi
    activate GenerationProcessStep
    
    GenerationProcessStep->>GenerationProcessStep: GenerationProcessStep()
    GenerationProcessStep-->>GenerationPipelineService: trả về
    deactivate GenerationProcessStep
    
    GenerationPipelineService->>WsGenerationManager: gọi
    activate WsGenerationManager
    
    WsGenerationManager->>WsGenerationManager: send_state()
    WsGenerationManager->>WsEnvelope: gọi
    activate WsEnvelope
    
    WsEnvelope->>WsEnvelope: WsEnvelope()
    WsEnvelope-->>WsGenerationManager: trả về
    deactivate WsEnvelope
    
    WsGenerationManager-->>LiveProgress: trả về
    deactivate WsGenerationManager
    
    WsGenerationManager->>WsGenerationManager: broadcast
    LiveProgress-->>User: Hiển thị
    deactivate GenerationPipelineService
    deactivate DocSocketService
    deactivate LiveProgress
```
