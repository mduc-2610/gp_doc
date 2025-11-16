# Sequence Diagram - Generation với Batch

## 1. Tạo yêu cầu generation

```mermaid
sequenceDiagram
    actor User
    participant DocAgentPage as DocAgentPage.js
    participant QuestionGenDialog as QuestionGenDialog.tsx
    participant DocAgentService as DocAgentService
    participant GenRoutes as gen_routes
    participant QuestionGenService as QuestionGenerationService
    participant FlashcardGenService as FlashcardGenerationService
    participant DB as Database

    User->>DocAgentPage: Click nút "Generate"
    activate DocAgentPage
    DocAgentPage->>QuestionGenDialog: Hiển thị dialog
    activate QuestionGenDialog
    
    QuestionGenDialog->>DocAgentService: getUserGenerationProcess(userId, "question")
    activate DocAgentService
    DocAgentService->>GenRoutes: GET /gen/user-process?user_id&result_type
    activate GenRoutes
    GenRoutes->>DB: Query active process
    DB-->>GenRoutes: null (no active process)
    GenRoutes-->>DocAgentService: null
    deactivate GenRoutes
    DocAgentService-->>QuestionGenDialog: ServiceResult(null)
    deactivate DocAgentService
    
    QuestionGenDialog-->>User: Hiển thị form generation
    
    User->>QuestionGenDialog: Chọn documents, điền parameters, click "Generate"
    QuestionGenDialog->>QuestionGenDialog: Validate form data
    
    QuestionGenDialog->>DocAgentService: batchGenerate({question_req, flashcard_req})
    activate DocAgentService
    DocAgentService->>GenRoutes: POST /gen/batch-generate
    activate GenRoutes
    
    GenRoutes->>QuestionGenService: Create service
    activate QuestionGenService
    GenRoutes->>DB: Create GenerationProcess(QUESTION, PENDING)
    DB-->>GenRoutes: question_process
    GenRoutes->>DB: Create GenerationBatch(question_process_id)
    DB-->>GenRoutes: question_batch
    deactivate QuestionGenService
    
    GenRoutes->>FlashcardGenService: Create service
    activate FlashcardGenService
    GenRoutes->>DB: Create GenerationProcess(FLASHCARD, PENDING)
    DB-->>GenRoutes: flashcard_process
    GenRoutes->>DB: Create GenerationBatch(flashcard_process_id)
    DB-->>GenRoutes: flashcard_batch
    deactivate FlashcardGenService
    
    GenRoutes-->>DocAgentService: BatchGenerateResponse
    deactivate GenRoutes
    DocAgentService-->>QuestionGenDialog: ServiceResult(response)
    deactivate DocAgentService
    
    QuestionGenDialog->>DocAgentPage: onContentGenerated(response)
    deactivate QuestionGenDialog
    
    DocAgentPage->>DocAgentPage: handleContentGenerated()
    DocAgentPage->>DocAgentPage: Connect WebSocket for Question
    DocAgentPage->>DocAgentPage: Connect WebSocket for Flashcard
    DocAgentPage-->>User: Hiển thị LiveProgress
    deactivate DocAgentPage
```

---

## 2. Theo dõi tiến trình generation qua WebSocket

```mermaid
sequenceDiagram
    actor User
    participant DocAgentPage as DocAgentPage.js
    participant DocSocketService as DocSocketService
    participant LiveProgress as LiveProgress.tsx
    participant WebSocket as WebSocket
    participant GenRoutes as gen_routes
    participant QuestionGenService as QuestionGenerationService
    participant WSManager as WSGenerationManager
    participant DB as Database

    DocAgentPage->>DocSocketService: connectQuestionSocket(processId, userId, handlers, {mode: "start"})
    activate DocSocketService
    DocSocketService->>WebSocket: Create connection to /gen/ws/question/{processId}
    activate WebSocket
    WebSocket-->>DocSocketService: Connection established
    
    DocSocketService->>WebSocket: Send message {type: "start", user_id}
    deactivate DocSocketService
    
    WebSocket->>GenRoutes: WebSocket message received
    activate GenRoutes
    GenRoutes->>WSManager: connect(processId, websocket)
    activate WSManager
    WSManager->>WSManager: Register WebSocket
    deactivate WSManager
    
    GenRoutes->>GenRoutes: Check if pipeline not running
    GenRoutes->>QuestionGenService: generate(process, db)
    activate QuestionGenService
    
    QuestionGenService->>WSManager: send_snapshot(service, process)
    activate WSManager
    WSManager->>WSManager: Create WsEnvelope(event="snapshot")
    WSManager->>WebSocket: Broadcast snapshot
    deactivate WSManager
    
    WebSocket->>DocSocketService: Receive snapshot message
    activate DocSocketService
    DocSocketService->>DocSocketService: Parse message
    DocSocketService->>DocAgentPage: onSnapshot(data)
    deactivate DocSocketService
    
    activate DocAgentPage
    DocAgentPage->>DocAgentPage: Update generationState with steps
    DocAgentPage->>LiveProgress: Render with steps
    activate LiveProgress
    LiveProgress-->>User: Hiển thị danh sách steps
    deactivate LiveProgress
    deactivate DocAgentPage
    
    QuestionGenService->>QuestionGenService: execute_pipeline()
    QuestionGenService->>QuestionGenService: _step_retrieve_context()
    QuestionGenService->>DB: Update step status to RUNNING
    
    QuestionGenService->>WSManager: send_state(processId, currentStep, stepData)
    activate WSManager
    WSManager->>WSManager: Create WsEnvelope(event="state")
    WSManager->>WebSocket: Broadcast state
    deactivate WSManager
    
    WebSocket->>DocSocketService: Receive state message
    activate DocSocketService
    DocSocketService->>DocAgentPage: onState(data)
    deactivate DocSocketService
    
    activate DocAgentPage
    DocAgentPage->>DocAgentPage: Update step in questionSteps
    DocAgentPage->>LiveProgress: Re-render
    activate LiveProgress
    LiveProgress-->>User: Hiển thị step RUNNING
    deactivate LiveProgress
    deactivate DocAgentPage
    
    QuestionGenService->>QuestionGenService: Complete retrieve_context
    QuestionGenService->>DB: Update step status to COMPLETED
    QuestionGenService->>WSManager: send_state() COMPLETED
    
    Note over QuestionGenService: Repeat for steps: generate_content,<br/>validate_results, save_batch
    
    QuestionGenService->>QuestionGenService: Pipeline completed
    QuestionGenService->>DB: Update process status to COMPLETED
    
    QuestionGenService->>WSManager: send_notify(processId, code="finished")
    activate WSManager
    WSManager->>WSManager: Create WsEnvelope(event="notify")
    WSManager->>WebSocket: Broadcast notify
    deactivate WSManager
    
    WebSocket->>DocSocketService: Receive notify message
    activate DocSocketService
    DocSocketService->>DocAgentPage: onFinished()
    deactivate DocSocketService
    
    activate DocAgentPage
    DocAgentPage->>DocAgentPage: handleQuestionSocketFinished()
    DocAgentPage->>DocAgentPage: Remove questionSocket, questionSteps
    DocAgentPage->>LiveProgress: Re-render
    activate LiveProgress
    LiveProgress-->>User: Hiển thị completed, nút "View Results"
    deactivate LiveProgress
    deactivate DocAgentPage
    
    deactivate QuestionGenService
    deactivate GenRoutes
    deactivate WebSocket
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
    
    LiveProgress->>DocAgentService: cancelProcess(processId, userId)
    activate DocAgentService
    DocAgentService->>GenRoutes: POST /gen/cancel/{processId}
    activate GenRoutes
    
    GenRoutes->>GenProcessService: cancel_process(db, processId, userId)
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
    QuestionGenService->>WSManager: send_notify(code="cancelled")
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
    QuestionTab->>DocAgentService: getPendingBatch(userId, sessionId, "question")
    activate DocAgentService
    DocAgentService->>BatchRoutes: GET /batch/pending-batch?user_id&session_id&result_type
    activate BatchRoutes
    
    BatchRoutes->>BatchService: get_pending_batch(db, userId, sessionId, resultType)
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
    
    QuestionTab->>DocAgentService: getBatchItems(batchId)
    activate DocAgentService
    DocAgentService->>BatchRoutes: GET /batch/batch-item/by-batch/{batchId}
    activate BatchRoutes
    
    BatchRoutes->>BatchService: get_batch_items_by_batch(db, batchId)
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
    
    QuestionApprovalTab->>DocAgentService: approve({batch_id, selected_item_ids, user_id})
    activate DocAgentService
    DocAgentService->>BatchRoutes: POST /batch/approve
    activate BatchRoutes
    
    BatchRoutes->>BatchService: approve_batch(db, request)
    activate BatchService
    BatchService->>DB: BEGIN TRANSACTION
    
    BatchService->>DB: Bulk update BatchItem<br/>status = APPROVED
    DB-->>BatchService: Updated count
    
    BatchService->>BatchService: Extract question_ids from items
    
    BatchService->>BulkOpsService: bulk_insert_questions(db, questions_data)
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
    QuestionTab->>DocAgentService: getQuestionsBySession(sessionId)
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
    
    QuestionApprovalTab->>DocAgentService: reject({batch_id, selected_item_ids, feedback, user_id})
    activate DocAgentService
    DocAgentService->>BatchRoutes: POST /batch/reject
    activate BatchRoutes
    
    BatchRoutes->>BatchService: reject_batch(db, request)
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
    
    QuestionApprovalTab->>DocAgentService: regenerate({batch_id, user_id})
    activate DocAgentService
    DocAgentService->>GenRoutes: POST /gen/regenerate
    activate GenRoutes
    
    GenRoutes->>QuestionGenService: regenerate(batch_id, user_id, db)
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

---

## 8. Kết nối lại với tiến trình đang chạy

```mermaid
sequenceDiagram
    actor User
    participant DocAgentPage as DocAgentPage.js
    participant DocSocketService as DocSocketService
    participant DocAgentService as DocAgentService
    participant GenRoutes as gen_routes
    participant GenProcessService as GenerationProcessService
    participant WSManager as WSGenerationManager
    participant WebSocket as WebSocket
    participant DB as Database

    User->>DocAgentPage: Reload page / Navigate to Session
    activate DocAgentPage
    
    DocAgentPage->>DocAgentService: getSessionDetail(sessionId)
    DocAgentService-->>DocAgentPage: Session info
    DocAgentPage->>DocAgentPage: Set sessionConfirmed = true
    
    DocAgentPage->>DocAgentPage: checkAndConnectRunningProcesses()
    
    DocAgentPage->>DocAgentService: getUserGenerationProcess(userId, "question")
    activate DocAgentService
    DocAgentService->>GenRoutes: GET /gen/user-process?user_id&result_type
    activate GenRoutes
    
    GenRoutes->>GenProcessService: get_user_generation_process(db, userId, resultType)
    activate GenProcessService
    GenProcessService->>DB: Query active process<br/>(status IN [PENDING, RUNNING])
    DB-->>GenProcessService: process (if exists)
    GenProcessService-->>GenRoutes: GenerationProcess
    deactivate GenProcessService
    
    GenRoutes-->>DocAgentService: GenerationProcessResponse
    deactivate GenRoutes
    DocAgentService-->>DocAgentPage: ServiceResult(process)
    deactivate DocAgentService
    
    alt Has running process
        DocAgentPage->>DocSocketService: connectQuestionSocket(processId, userId, handlers, {mode: "resume"})
        activate DocSocketService
        DocSocketService->>WebSocket: Create connection
        activate WebSocket
        WebSocket-->>DocSocketService: Connection established
        
        DocSocketService->>WebSocket: Send message {type: "resume", user_id}
        deactivate DocSocketService
        
        WebSocket->>GenRoutes: WebSocket message received
        activate GenRoutes
        GenRoutes->>WSManager: connect(processId, websocket)
        activate WSManager
        WSManager->>WSManager: Register WebSocket
        deactivate WSManager
        
        GenRoutes->>GenRoutes: Check pipeline is running
        GenRoutes->>WSManager: send_snapshot(service, process)
        activate WSManager
        WSManager->>WSManager: Create WsEnvelope(event="snapshot")
        WSManager->>WebSocket: Broadcast snapshot
        deactivate WSManager
        
        WebSocket->>DocSocketService: Receive snapshot
        activate DocSocketService
        DocSocketService->>DocAgentPage: onSnapshot(data)
        deactivate DocSocketService
        
        DocAgentPage->>DocAgentPage: Update generationState with steps
        DocAgentPage-->>User: Hiển thị LiveProgress với trạng thái hiện tại
        
        deactivate GenRoutes
        deactivate WebSocket
        
        Note over DocAgentPage: Tiếp tục nhận state updates<br/>từ WebSocket
    else No running process
        DocAgentPage-->>User: Không hiển thị LiveProgress
    end
    
    Note over DocAgentPage: Lặp lại cho resultType="flashcard"
    
    DocAgentPage->>DocAgentPage: Set isGenerating = true if has process
    deactivate DocAgentPage
```
