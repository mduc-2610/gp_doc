# Sequence Diagram - Generation với Batch

## 1. Tạo yêu cầu generation

```mermaid
sequenceDiagram
    actor User
    participant DocAgentPage as DocAgentPage.js
    participant QuestionGenDialog as QuestionGenDialog.tsx
    participant DocAgentService as DocAgentService
    participant GenRoutes as gen_routes
    participant GenProcessService as GenerationProcessService
    participant QuestionGenService as QuestionGenerationService
    participant FlashcardGenService as FlashcardGenerationService
    participant DB as Database

    User->>DocAgentPage: Click nút "Generate"
    activate DocAgentPage
    DocAgentPage->>QuestionGenDialog: Hiển thị dialog
    activate QuestionGenDialog
    
    QuestionGenDialog->>DocAgentService: getUserGenerationProcess()
    activate DocAgentService
    DocAgentService->>GenRoutes: GET /gen/process/running?user_id&type
    activate GenRoutes
    GenRoutes->>GenProcessService: get_user_generation_process()
    activate GenProcessService
    GenProcessService->>DB: Query active GenerationProcess
    activate DB
    Note over DB: Filter: user_id, result_type, finished_at IS NULL,<br/>canceled_at IS NULL, status IS NULL,<br/>no FAILED steps
    DB-->>GenProcessService: null (no active process)
    deactivate DB
    GenProcessService-->>GenRoutes: null
    deactivate GenProcessService
    GenRoutes-->>DocAgentService: null
    deactivate GenRoutes
    DocAgentService-->>QuestionGenDialog: ServiceResult(null)
    deactivate DocAgentService
    
    QuestionGenDialog-->>User: Hiển thị form generation
    
    User->>QuestionGenDialog: Chọn documents, điền parameters, click "Generate"
    QuestionGenDialog->>QuestionGenDialog: Validate form data
    
    QuestionGenDialog->>DocAgentService: batchGenerate()
    activate DocAgentService
    DocAgentService->>GenRoutes: POST /gen/process/question
    activate GenRoutes
    
    GenRoutes->>QuestionGenService: Create service instance
    activate QuestionGenService
    QuestionGenService->>GenProcessService: _validate_no_active_process()
    activate GenProcessService
    GenProcessService->>DB: Query active process for user
    activate DB
    DB-->>GenProcessService: null (no conflict)
    deactivate DB
    GenProcessService-->>QuestionGenService: validation passed
    deactivate GenProcessService
    
    QuestionGenService->>DB: Create GenerationProcess(QUESTION, PENDING)
    activate DB
    DB-->>QuestionGenService: question_process
    deactivate DB
    
    QuestionGenService->>DB: Create GenerationBatch(question_process_id)
    activate DB
    DB-->>QuestionGenService: question_batch
    deactivate DB
    QuestionGenService-->>GenRoutes: question_process
    deactivate QuestionGenService
    
    GenRoutes-->>DocAgentService: GenerationProcessResponse
    deactivate GenRoutes
    
    DocAgentService->>GenRoutes: POST /gen/process/flashcard
    activate GenRoutes
    
    GenRoutes->>FlashcardGenService: Create service instance
    activate FlashcardGenService
    FlashcardGenService->>GenProcessService: _validate_no_active_process()
    activate GenProcessService
    GenProcessService->>DB: Query active process for user
    activate DB
    DB-->>GenProcessService: null (no conflict)
    deactivate DB
    GenProcessService-->>FlashcardGenService: validation passed
    deactivate GenProcessService
    
    FlashcardGenService->>DB: Create GenerationProcess(FLASHCARD, PENDING)
    activate DB
    DB-->>FlashcardGenService: flashcard_process
    deactivate DB
    
    FlashcardGenService->>DB: Create GenerationBatch(flashcard_process_id)
    activate DB
    DB-->>FlashcardGenService: flashcard_batch
    deactivate DB
    FlashcardGenService-->>GenRoutes: flashcard_process
    deactivate FlashcardGenService
    
    GenRoutes-->>DocAgentService: GenerationProcessResponse
    deactivate GenRoutes
    
    DocAgentService-->>QuestionGenDialog: ServiceResult(BatchGenerateResponse)
    deactivate DocAgentService
    
    QuestionGenDialog->>DocAgentPage: onContentGenerated()
    deactivate QuestionGenDialog
    
    DocAgentPage->>DocAgentPage: handleContentGenerated()
    DocAgentPage->>DocAgentPage: Connect WebSocket for Question
    DocAgentPage->>DocAgentPage: Connect WebSocket for Flashcard
    DocAgentPage-->>User: Hiển thị LiveProgress
    deactivate DocAgentPage
```

---

## 2. Theo dõi tiến trình generation qua WebSocket

### 2.1. Khởi tạo pipeline

```mermaid
sequenceDiagram
    actor User
    participant DocAgentPage as DocAgentPage.js
    participant LiveProgress as LiveProgress.tsx
    participant DocSocketService as DocSocketService
    participant WebSocket as WebSocket
    participant GenRoutes as gen_routes
    participant WSManager as WSGenerationManager
    participant QuestionGenService as QuestionGenerationService
    participant GenProcessService as GenerationProcessService
    participant DB as Database

    Note over User,DB: User đang ở giao diện Session chi tiết, WebSocket đã kết nối
    
    User->>DocAgentPage: WebSocket connection established
    activate DocAgentPage
    
    Note over GenRoutes,QuestionGenService: Backend bắt đầu thực hiện pipeline
    GenRoutes->>QuestionGenService: execute_pipeline()
    activate QuestionGenService
    QuestionGenService->>GenProcessService: get_process()
    activate GenProcessService
    GenProcessService->>DB: Query GenerationProcess
    activate DB
    DB-->>GenProcessService: process
    deactivate DB
    GenProcessService-->>QuestionGenService: process
    deactivate GenProcessService
    
    QuestionGenService->>QuestionGenService: Get ordered steps
    QuestionGenService->>WSManager: send_snapshot()
    activate WSManager
    WSManager->>WebSocket: snapshot message
    deactivate WSManager
    
    WebSocket->>DocSocketService: snapshot event
    activate DocSocketService
    DocSocketService->>DocAgentPage: onSnapshot callback
    deactivate DocSocketService
    
    DocAgentPage->>LiveProgress: Initialize with steps
    activate LiveProgress
    LiveProgress-->>DocAgentPage: rendered
    deactivate LiveProgress
    
    DocAgentPage-->>User: Display pipeline overview
    deactivate DocAgentPage
    deactivate QuestionGenService
```

### 2.2. Step 1: Query Processing

```mermaid
sequenceDiagram
    actor User
    participant LiveProgress as LiveProgress.tsx
    participant DocAgentPage as DocAgentPage.js
    participant DocSocketService as DocSocketService
    participant WebSocket as WebSocket
    participant WSManager as WSGenerationManager
    participant QuestionGenService as QuestionGenerationService
    participant EmbeddingFactory as EmbeddingFactory
    participant EmbeddingModel as EmbeddingModel<br/>(FastEmbed/OpenAI)
    participant QueryRouter as QueryRouter
    participant QueryConstructor as QueryConstructor
    participant Generator as Generator<br/>(OpenAI/GenAI/etc)
    participant DB as Database

    Note over User,DB: Step 1: Query Processing
    QuestionGenService->>QuestionGenService: step_query_processing()
    QuestionGenService->>DB: Update step status to IN_PROGRESS
    activate DB
    DB-->>QuestionGenService: updated
    deactivate DB
    
    QuestionGenService->>WSManager: send_state()
    activate WSManager
    WSManager->>WebSocket: state message
    deactivate WSManager
    
    WebSocket->>DocSocketService: state event
    activate DocSocketService
    DocSocketService->>DocAgentPage: onState callback
    deactivate DocSocketService
    
    activate DocAgentPage
    DocAgentPage->>LiveProgress: Update step 1 progress
    activate LiveProgress
    LiveProgress-->>DocAgentPage: rendered
    deactivate LiveProgress
    DocAgentPage-->>User: Display "Processing query..."
    deactivate DocAgentPage
    
    QuestionGenService->>EmbeddingFactory: create_embedding_model()
    activate EmbeddingFactory
    EmbeddingFactory->>EmbeddingModel: Create instance
    activate EmbeddingModel
    EmbeddingFactory-->>QuestionGenService: embedding_model
    deactivate EmbeddingFactory
    
    QuestionGenService->>EmbeddingModel: create_embeddings()
    EmbeddingModel-->>QuestionGenService: query_embedding
    
    QuestionGenService->>QueryRouter: route_query()
    activate QueryRouter
    QueryRouter->>Generator: analyze_query_complexity()
    activate Generator
    Generator-->>QueryRouter: complexity analysis
    QueryRouter->>QueryRouter: Determine best strategy
    QueryRouter-->>QuestionGenService: routing_result {strategy, params}
    deactivate QueryRouter
    
    alt Strategy = MULTI_QUERY
        QuestionGenService->>QueryConstructor: generate_multi_query()
        activate QueryConstructor
        QueryConstructor->>Generator: generate_text()
        Generator-->>QueryConstructor: variations
        QueryConstructor-->>QuestionGenService: query_variations
        deactivate QueryConstructor
    
    else Strategy = DECOMPOSE
        QuestionGenService->>QueryConstructor: decompose_query()
        activate QueryConstructor
        QueryConstructor->>Generator: generate_text()
        Generator-->>QueryConstructor: sub_queries
        QueryConstructor-->>QuestionGenService: query_variations
        deactivate QueryConstructor
    
    else Strategy = HYBRID
        QuestionGenService->>QueryConstructor: generate_multi_query()
        activate QueryConstructor
        QueryConstructor->>Generator: generate_text()
        Generator-->>QueryConstructor: variations
        QueryConstructor-->>QuestionGenService: query_variations
        deactivate QueryConstructor
    end
    deactivate Generator
    
    QuestionGenService->>DB: Save query_embedding, strategy, variations
    activate DB
    DB-->>QuestionGenService: saved
    deactivate DB
    
    QuestionGenService->>DB: Update step to DONE
    activate DB
    DB-->>QuestionGenService: updated
    deactivate DB
    
    QuestionGenService->>WSManager: send_state()
    activate WSManager
    WSManager->>WebSocket: state message
    deactivate WSManager
    
    WebSocket->>DocSocketService: state event
    activate DocSocketService
    DocSocketService->>DocAgentPage: onState callback
    deactivate DocSocketService
    
    activate DocAgentPage
    DocAgentPage->>LiveProgress: Update step 1 completed
    activate LiveProgress
    LiveProgress-->>DocAgentPage: rendered
    deactivate LiveProgress
    DocAgentPage-->>User: Display "Query processed ✓"
    deactivate DocAgentPage
    deactivate EmbeddingModel
```

### 2.3. Step 2: Retrieval

```mermaid
sequenceDiagram
    actor User
    participant LiveProgress as LiveProgress.tsx
    participant DocAgentPage as DocAgentPage.js
    participant DocSocketService as DocSocketService
    participant WebSocket as WebSocket
    participant WSManager as WSGenerationManager
    participant QuestionGenService as QuestionGenerationService
    participant EmbeddingFactory as EmbeddingFactory
    participant EmbeddingModel as EmbeddingModel<br/>(FastEmbed/OpenAI)
    participant HyDETranslator as HyDETranslator
    participant QuerySimilarProcessor as QuerySimilarProcessor
    participant RAGFusion as RAGFusion
    participant Reranker as Reranker
    participant Generator as Generator<br/>(OpenAI/GenAI/etc)
    participant StorageProvider as StorageProvider
    participant DB as Database

    Note over User,DB: Step 2: Retrieval
    QuestionGenService->>QuestionGenService: step_retrieval()
    QuestionGenService->>DB: Update step status to IN_PROGRESS
    activate DB
    DB-->>QuestionGenService: updated
    deactivate DB
    
    QuestionGenService->>WSManager: send_state()
    activate WSManager
    WSManager->>WebSocket: state message
    deactivate WSManager
    
    WebSocket->>DocSocketService: state event
    activate DocSocketService
    DocSocketService->>DocAgentPage: onState callback
    deactivate DocSocketService
    
    activate DocAgentPage
    DocAgentPage->>LiveProgress: Update step 2 progress
    activate LiveProgress
    LiveProgress-->>DocAgentPage: rendered
    deactivate LiveProgress
    DocAgentPage-->>User: Display "Retrieving context..."
    deactivate DocAgentPage
    
    QuestionGenService->>EmbeddingFactory: create_embedding_model()
    activate EmbeddingFactory
    EmbeddingFactory->>EmbeddingModel: Create instance
    activate EmbeddingModel
    EmbeddingFactory-->>QuestionGenService: Return embedding_model
    deactivate EmbeddingFactory
    
    alt Strategy = VECTOR_SEARCH
        QuestionGenService->>QuerySimilarProcessor: similarity_search()
        activate QuerySimilarProcessor
        QuerySimilarProcessor->>DB: Query chunks with pgvector
        DB-->>QuerySimilarProcessor: Return similar chunks
        QuerySimilarProcessor-->>QuestionGenService: Return results
        deactivate QuerySimilarProcessor
    
    else Strategy = HYDE
        QuestionGenService->>HyDETranslator: generate_hypothetical_document()
        activate HyDETranslator
        HyDETranslator->>Generator: generate_text()
        activate Generator
        Generator-->>HyDETranslator: Return hyde_doc
        deactivate Generator
        HyDETranslator-->>QuestionGenService: Return hyde_doc
        deactivate HyDETranslator
        
        QuestionGenService->>QuerySimilarProcessor: similarity_search()
        activate QuerySimilarProcessor
        QuerySimilarProcessor->>DB: Query chunks
        DB-->>QuerySimilarProcessor: Return chunks
        QuerySimilarProcessor-->>QuestionGenService: Return hyde_results
        deactivate QuerySimilarProcessor
    
    else Strategy = MULTI_QUERY or DECOMPOSE
        loop For each query variation
            QuestionGenService->>QuerySimilarProcessor: similarity_search()
            activate QuerySimilarProcessor
            QuerySimilarProcessor->>DB: Query chunks
            DB-->>QuerySimilarProcessor: Return chunks
            QuerySimilarProcessor-->>QuestionGenService: Return variation_results
            deactivate QuerySimilarProcessor
        end
    
    else Strategy = HYBRID
        QuestionGenService->>QuerySimilarProcessor: similarity_search()
        activate QuerySimilarProcessor
        QuerySimilarProcessor->>DB: Query chunks
        QuerySimilarProcessor-->>QuestionGenService: Return vec_results
        deactivate QuerySimilarProcessor
        
        QuestionGenService->>HyDETranslator: generate_hypothetical_document()
        activate HyDETranslator
        HyDETranslator->>Generator: generate_text()
        activate Generator
        Generator-->>HyDETranslator: Return hyde_doc
        deactivate Generator
        HyDETranslator-->>QuestionGenService: Return hyde_doc
        deactivate HyDETranslator
        
        QuestionGenService->>QuerySimilarProcessor: similarity_search()
        activate QuerySimilarProcessor
        QuerySimilarProcessor->>DB: Query chunks
        QuerySimilarProcessor-->>QuestionGenService: Return hyde_results
        deactivate QuerySimilarProcessor
        
        loop For each query variation
            QuestionGenService->>QuerySimilarProcessor: similarity_search()
            activate QuerySimilarProcessor
            QuerySimilarProcessor->>DB: Query chunks
            QuerySimilarProcessor-->>QuestionGenService: Return multi_results
            deactivate QuerySimilarProcessor
        end
    end
    
    alt Multiple result sets
        QuestionGenService->>RAGFusion: fuse_multi_strategy_results()
        activate RAGFusion
        RAGFusion->>RAGFusion: reciprocal_rank_fusion(ranked_lists)
        RAGFusion-->>QuestionGenService: Return fused_results
        deactivate RAGFusion
    end
    
    alt Reranking enabled
        QuestionGenService->>Reranker: hybrid_rerank()
        activate Reranker
        Reranker->>Reranker: rerank_with_llm(query, candidates)
        Reranker->>Generator: generate_text()
        activate Generator
        Generator-->>Reranker: Return relevance scores
        deactivate Generator
        Reranker->>Reranker: rerank_by_diversity(candidates)
        Reranker-->>QuestionGenService: Return reranked_results
        deactivate Reranker
    end
    
    QuestionGenService->>QuestionGenService: Build context from top-k chunks
    QuestionGenService->>DB: bulk_insert_document_chunk_used()
    
    QuestionGenService->>StorageProvider: save_context_used_file()
    activate StorageProvider
    StorageProvider-->>QuestionGenService: Return context_url
    deactivate StorageProvider
    
    QuestionGenService->>EmbeddingModel: create_embeddings()
    EmbeddingModel-->>QuestionGenService: Return context_embedding
    deactivate EmbeddingModel
    
    QuestionGenService->>DB: Save context_url, context_embedding
    QuestionGenService->>DB: Update step to DONE
    QuestionGenService->>WSManager: send_state()
    activate WSManager
    WSManager->>WebSocket: Send state message
    WebSocket-->>DocSocketService: Receive state
    activate DocSocketService
    DocSocketService->>DocAgentPage: onState callback
    activate DocAgentPage
    DocAgentPage->>LiveProgress: Update step 2 completed
    activate LiveProgress
    LiveProgress->>User: Display "Context retrieved ✓"
    deactivate LiveProgress
    deactivate DocAgentPage
    deactivate DocSocketService
    deactivate WSManager
```

### 2.4. Step 3: Duplicate Prevention

```mermaid
sequenceDiagram
    actor User
    participant LiveProgress as LiveProgress.tsx
    participant DocAgentPage as DocAgentPage.js
    participant DocSocketService as DocSocketService
    participant WebSocket as WebSocket
    participant WSManager as WSGenerationManager
    participant QuestionGenService as QuestionGenerationService
    participant QuerySimilarProcessor as QuerySimilarProcessor
    participant BatchService as BatchService
    participant DB as Database

    Note over User,DB: Step 3: Duplicate Prevention
    QuestionGenService->>QuestionGenService: step_duplicate_prevention()
    QuestionGenService->>DB: Update step status to IN_PROGRESS
    activate DB
    DB-->>QuestionGenService: updated
    deactivate DB
    
    QuestionGenService->>WSManager: send_state()
    activate WSManager
    WSManager->>WebSocket: state message
    deactivate WSManager
    
    WebSocket->>DocSocketService: state event
    activate DocSocketService
    DocSocketService->>DocAgentPage: onState callback
    deactivate DocSocketService
    
    activate DocAgentPage
    DocAgentPage->>LiveProgress: Update step 3 progress
    activate LiveProgress
    LiveProgress-->>DocAgentPage: rendered
    deactivate LiveProgress
    DocAgentPage-->>User: Display "Checking duplicates..."
    deactivate DocAgentPage
    
    QuestionGenService->>QuerySimilarProcessor: find_similar_processes()
    activate QuerySimilarProcessor
    QuerySimilarProcessor->>QuerySimilarProcessor: find_similar_processes_by_query()
    QuerySimilarProcessor->>DB: Query similar processes by query embedding
    activate DB
    DB-->>QuerySimilarProcessor: candidate_process_ids
    deactivate DB
    
    QuerySimilarProcessor->>QuerySimilarProcessor: find_similar_processes_by_context()
    QuerySimilarProcessor->>DB: Query similar processes by context embedding
    activate DB
    DB-->>QuerySimilarProcessor: filtered_process_ids
    deactivate DB
    QuerySimilarProcessor-->>QuestionGenService: similar_process_ids
    deactivate QuerySimilarProcessor
    
    QuestionGenService->>BatchService: get_approved_items_by_processes()
    activate BatchService
    BatchService->>DB: Query approved BatchItems
    activate DB
    DB-->>BatchService: approved_items
    deactivate DB
    BatchService-->>QuestionGenService: approved_items
    deactivate BatchService
    
    QuestionGenService->>DB: Update step to DONE with approved_item_ids
    activate DB
    DB-->>QuestionGenService: updated
    deactivate DB
    
    QuestionGenService->>WSManager: send_state()
    activate WSManager
    WSManager->>WebSocket: state message
    deactivate WSManager
    
    WebSocket->>DocSocketService: state event
    activate DocSocketService
    DocSocketService->>DocAgentPage: onState callback
    deactivate DocSocketService
    
    activate DocAgentPage
    DocAgentPage->>LiveProgress: Update step 3 completed
    activate LiveProgress
    LiveProgress-->>DocAgentPage: rendered
    deactivate LiveProgress
    DocAgentPage-->>User: Display "Duplicates checked ✓"
    deactivate DocAgentPage
```

### 2.5. Step 4: Generation

```mermaid
sequenceDiagram
    actor User
    participant LiveProgress as LiveProgress.tsx
    participant DocAgentPage as DocAgentPage.js
    participant DocSocketService as DocSocketService
    participant WebSocket as WebSocket
    participant WSManager as WSGenerationManager
    participant QuestionGenService as QuestionGenerationService
    participant PromptBuilder as PromptBuilder
    participant StorageProvider as StorageProvider
    participant GeneratorFactory as GeneratorFactory
    participant Generator as Generator<br/>(OpenAI/GenAI/etc)
    participant BatchService as BatchService
    participant DB as Database

    Note over User,DB: Step 4: Generation
    QuestionGenService->>QuestionGenService: step_generation()
    QuestionGenService->>DB: Update step status to IN_PROGRESS
    activate DB
    DB-->>QuestionGenService: updated
    deactivate DB
    
    QuestionGenService->>WSManager: send_state()
    activate WSManager
    WSManager->>WebSocket: state message
    deactivate WSManager
    
    WebSocket->>DocSocketService: state event
    activate DocSocketService
    DocSocketService->>DocAgentPage: onState callback
    deactivate DocSocketService
    
    activate DocAgentPage
    DocAgentPage->>LiveProgress: Update step 4 progress
    activate LiveProgress
    LiveProgress-->>DocAgentPage: rendered
    deactivate LiveProgress
    DocAgentPage-->>User: Display "Generating content..."
    deactivate DocAgentPage
    
    QuestionGenService->>QuestionGenService: generate_items()
    
    QuestionGenService->>PromptBuilder: build_prompt_for_generation()
    activate PromptBuilder
    PromptBuilder->>StorageProvider: Load context from storage
    activate StorageProvider
    StorageProvider-->>PromptBuilder: context_content
    deactivate StorageProvider
    
    PromptBuilder->>PromptBuilder: _build_existing_items_context()
    PromptBuilder-->>QuestionGenService: complete_prompt
    deactivate PromptBuilder
    
    QuestionGenService->>GeneratorFactory: create_generator()
    activate GeneratorFactory
    GeneratorFactory->>Generator: Create instance based on wrapper type
    activate Generator
    GeneratorFactory-->>QuestionGenService: generator
    deactivate GeneratorFactory
    
    QuestionGenService->>Generator: generate_text()
    Generator-->>QuestionGenService: response (JSON)
    
    QuestionGenService->>QuestionGenService: parse_response(response)
    QuestionGenService->>QuestionGenService: _validate_item() for each item
    
    QuestionGenService->>BatchService: create_generation_batch()
    activate BatchService
    BatchService->>DB: Insert GenerationBatch
    activate DB
    DB-->>BatchService: batch
    deactivate DB
    
    loop For each validated item
        BatchService->>DB: Insert BatchItem with item data
        activate DB
        DB-->>BatchService: batch_item
        deactivate DB
    end
    
    BatchService-->>QuestionGenService: batch with items
    deactivate BatchService
    
    QuestionGenService->>DB: Update result_generated_count
    activate DB
    DB-->>QuestionGenService: updated
    deactivate DB
    
    QuestionGenService->>DB: Update step to DONE
    activate DB
    DB-->>QuestionGenService: updated
    deactivate DB
    
    QuestionGenService->>WSManager: send_state()
    activate WSManager
    WSManager->>WebSocket: state message
    deactivate WSManager
    
    WebSocket->>DocSocketService: state event
    activate DocSocketService
    DocSocketService->>DocAgentPage: onState callback
    deactivate DocSocketService
    
    activate DocAgentPage
    DocAgentPage->>LiveProgress: Update step 4 completed
    activate LiveProgress
    LiveProgress-->>DocAgentPage: rendered
    deactivate LiveProgress
    DocAgentPage-->>User: Display "Generation completed ✓"
    deactivate DocAgentPage
    deactivate Generator
```

### 2.6. Hoàn thành pipeline

```mermaid
sequenceDiagram
    actor User
    participant LiveProgress as LiveProgress.tsx
    participant DocAgentPage as DocAgentPage.js
    participant DocSocketService as DocSocketService
    participant WebSocket as WebSocket
    participant WSManager as WSGenerationManager
    participant QuestionGenService as QuestionGenerationService
    participant DB as Database

    Note over User,DB: Pipeline completed
    QuestionGenService->>DB: Mark process as finished (status=SUCCESS)
    activate DB
    DB-->>QuestionGenService: updated
    deactivate DB
    
    QuestionGenService->>WSManager: notify()
    activate WSManager
    WSManager->>WebSocket: finished message
    deactivate WSManager
    
    WebSocket->>DocSocketService: finished event
    activate DocSocketService
    DocSocketService->>DocAgentPage: onFinished callback
    deactivate DocSocketService
    
    activate DocAgentPage
    DocAgentPage->>DocAgentPage: handleQuestionSocketFinished()
    DocAgentPage->>WebSocket: close()
    WebSocket-->>DocAgentPage: closed
    DocAgentPage->>DocAgentPage: Clear socket state
    DocAgentPage->>LiveProgress: Update to show completion
    activate LiveProgress
    LiveProgress->>LiveProgress: Display "Generation completed" message
    LiveProgress-->>DocAgentPage: rendered with "View Results" button
    deactivate LiveProgress
    
    DocAgentPage-->>User: Show completion with action button
    DocAgentPage->>DocAgentPage: Trigger check for pending batch
    deactivate DocAgentPage
```

---

## 3. Hủy tiến trình generation

```mermaid
sequenceDiagram
    actor User
    participant LiveProgress as LiveProgress.tsx
    participant DocAgentService as DocAgentService
    participant GenRoutes as gen_routes
    participant GenProcessService as GenerationProcessService
    participant DB as Database
    participant QuestionGenService as QuestionGenerationService
    participant WSManager as WSGenerationManager

    User->>LiveProgress: Click "Cancel Process"
    activate LiveProgress
    LiveProgress->>LiveProgress: Hiển thị confirm dialog
    User->>LiveProgress: Xác nhận hủy
    
    LiveProgress->>DocAgentService: cancelProcess()
    activate DocAgentService
    DocAgentService->>GenRoutes: POST /gen/cancel/{processId}
    activate GenRoutes
    
    GenRoutes->>GenProcessService: cancel_process()
    activate GenProcessService
    GenProcessService->>DB: Query process
    DB-->>GenProcessService: process
    GenProcessService->>GenProcessService: Validate user_id
    GenProcessService->>DB: Update status to CANCELLED
    DB-->>GenProcessService: Success
    GenProcessService-->>GenRoutes: {status: "cancelled"}
    deactivate GenProcessService
    
    GenRoutes-->>DocAgentService: MessageResponse
    deactivate GenRoutes
    DocAgentService-->>LiveProgress: ServiceResult(success)
    deactivate DocAgentService
    
    LiveProgress->>LiveProgress: onCancelComplete()
    LiveProgress-->>User: Hiển thị "Process cancelled successfully"
    deactivate LiveProgress
    
    Note over QuestionGenService: Pipeline đang chạy
    QuestionGenService->>DB: Check process status
    DB-->>QuestionGenService: CANCELLED
    QuestionGenService->>QuestionGenService: Throw ProcessCanceledException
    QuestionGenService->>WSManager: send_notify()
    activate WSManager
    WSManager->>WSManager: Broadcast notify
    deactivate WSManager
```

---

## 4. Xem kết quả batch

```mermaid
sequenceDiagram
    actor User
    participant QuestionTab as QuestionTab.tsx
    participant QuestionApprovalTab as QuestionApprovalTab.tsx
    participant DocAgentService as DocAgentService
    participant BatchRoutes as batch_routes
    participant BatchService as BatchService
    participant DB as Database

    Note over QuestionTab: Generation hoàn thành, onFinished() called
    
    QuestionTab->>QuestionTab: checkForPendingBatch()
    QuestionTab->>DocAgentService: getPendingBatch()
    activate DocAgentService
    DocAgentService->>BatchRoutes: GET /batch/pending-batch?user_id&session_id&result_type
    activate BatchRoutes
    
    BatchRoutes->>BatchService: get_pending_batch()
    activate BatchService
    BatchService->>DB: Query GenerationBatch<br/>(status=PENDING, result_type=QUESTION)
    DB-->>BatchService: batch
    BatchService-->>BatchRoutes: GenerationBatch
    deactivate BatchService
    
    BatchRoutes-->>DocAgentService: GenerationBatchResponse
    deactivate BatchRoutes
    DocAgentService-->>QuestionTab: ServiceResult(batch)
    deactivate DocAgentService
    
    QuestionTab->>QuestionTab: Set pendingBatchProcessId
    
    QuestionTab->>DocAgentService: getBatchItems()
    activate DocAgentService
    DocAgentService->>BatchRoutes: GET /batch/batch-item/by-batch/{batchId}
    activate BatchRoutes
    
    BatchRoutes->>BatchService: get_batch_items_by_batch()
    activate BatchService
    BatchService->>DB: Query BatchItem<br/>JOIN Question
    DB-->>BatchService: [BatchItem with Question data]
    BatchService-->>BatchRoutes: [BatchItem]
    deactivate BatchService
    
    BatchRoutes-->>DocAgentService: [BatchItemResponse]
    deactivate BatchRoutes
    DocAgentService-->>QuestionTab: ServiceResult([BatchItem])
    deactivate DocAgentService
    
    QuestionTab->>QuestionTab: Extract Questions from items
    QuestionTab->>QuestionTab: Set showApprovalTab = true
    
    QuestionTab->>QuestionApprovalTab: Render với batchId, items
    activate QuestionApprovalTab
    QuestionApprovalTab-->>User: Hiển thị approval tab với questions
    deactivate QuestionApprovalTab
```

---

## 5. Duyệt kết quả batch

```mermaid
sequenceDiagram
    actor User
    participant QuestionApprovalTab as QuestionApprovalTab.tsx
    participant QuestionTab as QuestionTab.tsx
    participant DocAgentService as DocAgentService
    participant BatchRoutes as batch_routes
    participant BatchService as BatchService
    participant BulkOpsService as BulkOperationsService
    participant DB as Database

    User->>QuestionApprovalTab: Chọn items, click "Approve Selected"
    activate QuestionApprovalTab
    
    QuestionApprovalTab->>QuestionApprovalTab: Validate selected items
    
    QuestionApprovalTab->>DocAgentService: approve()
    activate DocAgentService
    DocAgentService->>BatchRoutes: POST /batch/approve
    activate BatchRoutes
    
    BatchRoutes->>BatchService: approve_batch()
    activate BatchService
    BatchService->>DB: BEGIN TRANSACTION
    
    BatchService->>DB: Bulk update BatchItem<br/>status = APPROVED
    DB-->>BatchService: Updated count
    
    BatchService->>BatchService: Extract question_ids from items
    
    BatchService->>BulkOpsService: bulk_insert_questions()
    activate BulkOpsService
    BulkOpsService->>DB: Bulk INSERT into Question table
    DB-->>BulkOpsService: Insert count
    BulkOpsService-->>BatchService: Count
    deactivate BulkOpsService
    
    BatchService->>DB: Update batch.approved_items_count
    
    BatchService->>BatchService: Check if all items processed
    alt All items processed
        BatchService->>DB: Update batch.status = COMPLETED
    end
    
    BatchService->>DB: COMMIT TRANSACTION
    
    BatchService-->>BatchRoutes: MessageResponse
    deactivate BatchService
    
    BatchRoutes-->>DocAgentService: MessageResponse
    deactivate BatchRoutes
    DocAgentService-->>QuestionApprovalTab: ServiceResult(success)
    deactivate DocAgentService
    
    QuestionApprovalTab->>QuestionApprovalTab: onApprovalComplete()
    
    QuestionApprovalTab->>QuestionTab: Reload tab data
    activate QuestionTab
    QuestionTab->>DocAgentService: getQuestionsBySession()
    DocAgentService-->>QuestionTab: Questions (including approved)
    QuestionTab-->>User: Hiển thị questions mới
    deactivate QuestionTab
    
    QuestionApprovalTab->>QuestionApprovalTab: Check remaining items
    
    alt No remaining items
        QuestionApprovalTab->>QuestionApprovalTab: Set showApprovalTab = false
    else Has remaining items
        QuestionApprovalTab-->>User: Hiển thị remaining items
    end
    
    QuestionApprovalTab-->>User: Toast "Items approved successfully"
    deactivate QuestionApprovalTab
```

---

## 6. Từ chối kết quả batch

```mermaid
sequenceDiagram
    actor User
    participant QuestionApprovalTab as QuestionApprovalTab.tsx
    participant DocAgentService as DocAgentService
    participant BatchRoutes as batch_routes
    participant BatchService as BatchService
    participant DB as Database

    User->>QuestionApprovalTab: Chọn items, nhập feedback, click "Reject Selected"
    activate QuestionApprovalTab
    
    QuestionApprovalTab->>QuestionApprovalTab: Validate selected items<br/>Validate feedback length
    
    QuestionApprovalTab->>DocAgentService: reject()
    activate DocAgentService
    DocAgentService->>BatchRoutes: POST /batch/reject
    activate BatchRoutes
    
    BatchRoutes->>BatchService: reject_batch()
    activate BatchService
    BatchService->>DB: BEGIN TRANSACTION
    
    BatchService->>DB: Bulk update BatchItem<br/>status = REJECTED
    DB-->>BatchService: Updated count
    
    alt Has feedback
        BatchService->>DB: INSERT BatchFeedback<br/>(feedback_type=REJECTION)
        DB-->>BatchService: Feedback created
    end
    
    BatchService->>DB: Update batch.rejected_items_count
    
    BatchService->>BatchService: Check if all items processed
    alt All items processed
        BatchService->>DB: Update batch.status = COMPLETED
    end
    
    BatchService->>DB: COMMIT TRANSACTION
    
    BatchService-->>BatchRoutes: MessageResponse
    deactivate BatchService
    
    BatchRoutes-->>DocAgentService: MessageResponse
    deactivate BatchRoutes
    DocAgentService-->>QuestionApprovalTab: ServiceResult(success)
    deactivate DocAgentService
    
    QuestionApprovalTab->>QuestionApprovalTab: Reload batch items
    
    alt Has rejected items
        QuestionApprovalTab-->>User: Hiển thị nút "Regenerate"
    end
    
    alt Has remaining items
        QuestionApprovalTab-->>User: Hiển thị remaining items
    else No remaining items
        QuestionApprovalTab->>QuestionApprovalTab: Set showApprovalTab = false
    end
    
    QuestionApprovalTab-->>User: Toast "Items rejected successfully"
    deactivate QuestionApprovalTab
```

---

## 7. Tạo yêu cầu regenerate

```mermaid
sequenceDiagram
    actor User
    participant QuestionApprovalTab as QuestionApprovalTab.tsx
    participant DocAgentPage as DocAgentPage.js
    participant DocAgentService as DocAgentService
    participant GenRoutes as gen_routes
    participant QuestionGenService as QuestionGenerationService
    participant DB as Database

    User->>QuestionApprovalTab: Click "Regenerate"
    activate QuestionApprovalTab
    
    QuestionApprovalTab->>DocAgentService: regenerate()
    activate DocAgentService
    DocAgentService->>GenRoutes: POST /gen/regenerate
    activate GenRoutes
    
    GenRoutes->>QuestionGenService: regenerate()
    activate QuestionGenService
    
    QuestionGenService->>DB: Query old batch
    DB-->>QuestionGenService: old_batch
    
    QuestionGenService->>DB: Query rejected items
    DB-->>QuestionGenService: rejected_items
    
    QuestionGenService->>DB: Query feedback
    DB-->>QuestionGenService: feedback
    
    QuestionGenService->>DB: Create new GenerationProcess<br/>(process_order + 1)
    DB-->>QuestionGenService: new_process
    
    QuestionGenService->>DB: Create new GenerationBatch<br/>(parent_batch_id = old_batch.id)
    DB-->>QuestionGenService: new_batch
    
    QuestionGenService->>QuestionGenService: Build prompt with<br/>regeneration guidance
    
    QuestionGenService->>QuestionGenService: Start execute_pipeline()<br/>in background
    
    QuestionGenService-->>GenRoutes: new_batch
    deactivate QuestionGenService
    
    GenRoutes-->>DocAgentService: GenerationBatchResponse
    deactivate GenRoutes
    DocAgentService-->>QuestionApprovalTab: ServiceResult(new_batch)
    deactivate DocAgentService
    
    QuestionApprovalTab->>QuestionApprovalTab: Set showApprovalTab = false
    QuestionApprovalTab->>DocAgentPage: Trigger reconnect
    deactivate QuestionApprovalTab
    
    activate DocAgentPage
    DocAgentPage->>DocAgentPage: Connect WebSocket with new process
    DocAgentPage-->>User: Hiển thị LiveProgress cho regeneration
    deactivate DocAgentPage
```



