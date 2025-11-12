# Sequence Diagram - Quản lý Question và Flashcard

## 1. Tạo Question mới

```mermaid
sequenceDiagram
    actor User
    participant EditTab as EditTab.js
    participant QuestionEditTab as QuestionEditTab.js
    participant QuestionEditItem as QuestionEditItem.tsx
    participant DocAgentService as DocAgentService
    participant QuestionRoutes as question_routes
    participant QuestionService as QuestionService

    User->>EditTab: Click tab "Edit"
    activate EditTab
    EditTab-->>User: Hiển thị giao diện Edit
    
    User->>EditTab: Click tab con "Question"
    EditTab->>QuestionEditTab: Khởi tạo QuestionEditTab
    activate QuestionEditTab
    
    QuestionEditTab->>DocAgentService: getQuestionsBySession()
    activate DocAgentService
    DocAgentService->>QuestionRoutes: GET /question/questions/by-session/{sessionId}
    activate QuestionRoutes
    QuestionRoutes->>QuestionService: get_questions_by_session()
    activate QuestionService
    QuestionService-->>QuestionRoutes: [QuestionResponse]
    deactivate QuestionService
    QuestionRoutes-->>DocAgentService: [Question]
    deactivate QuestionRoutes
    DocAgentService-->>QuestionEditTab: ServiceResult~Question[]~
    deactivate DocAgentService
    
    QuestionEditTab-->>User: Hiển thị danh sách Question
    
    User->>QuestionEditTab: Click "Thêm Question mới"
    QuestionEditTab->>QuestionEditTab: Create temp question
    QuestionEditTab->>QuestionEditItem: Render in edit mode
    activate QuestionEditItem
    QuestionEditItem-->>User: Hiển thị form tạo Question mới
    
    User->>QuestionEditItem: Nhập thông tin và click "Lưu"
    QuestionEditItem->>QuestionEditItem: Validate form
    QuestionEditItem->>QuestionEditTab: onSave(updatedQuestion)
    deactivate QuestionEditItem
    
    QuestionEditTab->>DocAgentService: createQuestion()
    activate DocAgentService
    
    DocAgentService->>QuestionRoutes: POST /question/questions
    activate QuestionRoutes
    
    QuestionRoutes->>QuestionService: create_question()
    activate QuestionService
    
    QuestionService->>QuestionService: Create Question & QuestionAnswers<br/>db.commit()
    
    QuestionService-->>QuestionRoutes: QuestionResponse
    deactivate QuestionService
    
    QuestionRoutes-->>DocAgentService: QuestionResponse
    deactivate QuestionRoutes
    
    DocAgentService-->>QuestionEditTab: ServiceResult~Question~
    deactivate DocAgentService
    
    QuestionEditTab->>QuestionEditTab: Replace temp with real question
    QuestionEditTab-->>User: Hiển thị danh sách và thông báo thành công
    deactivate QuestionEditTab
    deactivate EditTab
```

---

## 2. Chỉnh sửa Question

```mermaid
sequenceDiagram
    actor User
    participant EditTab as EditTab.js
    participant QuestionEditTab as QuestionEditTab.js
    participant QuestionEditItem as QuestionEditItem.tsx
    participant DocAgentService as DocAgentService
    participant QuestionRoutes as question_routes
    participant QuestionService as QuestionService

    User->>EditTab: Click tab "Edit"
    activate EditTab
    EditTab-->>User: Hiển thị giao diện Edit
    
    User->>EditTab: Click tab con "Question"
    EditTab->>QuestionEditTab: Khởi tạo QuestionEditTab
    activate QuestionEditTab
    
    QuestionEditTab->>DocAgentService: getQuestionsBySession()
    activate DocAgentService
    DocAgentService->>QuestionRoutes: GET /question/questions/by-session/{sessionId}
    activate QuestionRoutes
    QuestionRoutes->>QuestionService: get_questions_by_session()
    activate QuestionService
    QuestionService-->>QuestionRoutes: [QuestionResponse]
    deactivate QuestionService
    QuestionRoutes-->>DocAgentService: [Question]
    deactivate QuestionRoutes
    DocAgentService-->>QuestionEditTab: ServiceResult~Question[]~
    deactivate DocAgentService
    
    QuestionEditTab-->>User: Hiển thị danh sách Question
    
    User->>QuestionEditTab: Click icon chỉnh sửa
    QuestionEditTab->>QuestionEditTab: Set editingId
    QuestionEditTab->>QuestionEditItem: Render in edit mode
    activate QuestionEditItem
    QuestionEditItem-->>User: Hiển thị form chỉnh sửa Question
    
    User->>QuestionEditItem: Nhập thông tin và click "Lưu"
    QuestionEditItem->>QuestionEditItem: Validate form
    QuestionEditItem->>QuestionEditTab: onSave(id, updatedQuestion)
    deactivate QuestionEditItem
    
    QuestionEditTab->>DocAgentService: updateQuestion()
    activate DocAgentService
    
    DocAgentService->>QuestionRoutes: PUT /question/questions/{questionId}
    activate QuestionRoutes
    
    QuestionRoutes->>QuestionService: update_question()
    activate QuestionService
    
    QuestionService->>QuestionService: Get question<br/>Update fields<br/>Update QuestionAnswers<br/>db.commit()
    
    QuestionService-->>QuestionRoutes: QuestionResponse
    deactivate QuestionService
    
    QuestionRoutes-->>DocAgentService: QuestionResponse
    deactivate QuestionRoutes
    
    DocAgentService-->>QuestionEditTab: ServiceResult~Question~
    deactivate DocAgentService
    
    QuestionEditTab->>QuestionEditTab: Update in list<br/>Set editingId=null
    QuestionEditTab-->>User: Hiển thị danh sách và thông báo thành công
    deactivate QuestionEditTab
    deactivate EditTab
```

---

## 3. Xóa Question

```mermaid
sequenceDiagram
    actor User
    participant EditTab as EditTab.js
    participant QuestionEditTab as QuestionEditTab.js
    participant DocAgentService as DocAgentService
    participant QuestionRoutes as question_routes
    participant QuestionService as QuestionService

    User->>EditTab: Click tab "Edit"
    activate EditTab
    EditTab-->>User: Hiển thị giao diện Edit
    
    User->>EditTab: Click tab con "Question"
    EditTab->>QuestionEditTab: Khởi tạo QuestionEditTab
    activate QuestionEditTab
    
    QuestionEditTab->>DocAgentService: getQuestionsBySession()
    activate DocAgentService
    DocAgentService->>QuestionRoutes: GET /question/questions/by-session/{sessionId}
    activate QuestionRoutes
    QuestionRoutes->>QuestionService: get_questions_by_session()
    activate QuestionService
    QuestionService-->>QuestionRoutes: [QuestionResponse]
    deactivate QuestionService
    QuestionRoutes-->>DocAgentService: [Question]
    deactivate QuestionRoutes
    DocAgentService-->>QuestionEditTab: ServiceResult~Question[]~
    deactivate DocAgentService
    
    QuestionEditTab-->>User: Hiển thị danh sách Question
    
    User->>QuestionEditTab: Click icon xóa
    User->>QuestionEditTab: Xác nhận xóa
    
    alt Nếu id bắt đầu bằng "temp-"
        QuestionEditTab->>QuestionEditTab: Remove from internalQuestions
        QuestionEditTab-->>User: Hiển thị danh sách Question được cập nhật
    else Nếu là real id
        QuestionEditTab->>DocAgentService: deleteQuestion(id)
        activate DocAgentService
        DocAgentService->>QuestionRoutes: DELETE /question/questions/{questionId}
        activate QuestionRoutes
        QuestionRoutes->>QuestionService: delete_question()
        activate QuestionService
        QuestionService->>QuestionService: Delete question
        QuestionService-->>QuestionRoutes: MessageResponse
        deactivate QuestionService
        QuestionRoutes-->>DocAgentService: MessageResponse
        deactivate QuestionRoutes
        DocAgentService-->>QuestionEditTab: ServiceResult
        deactivate DocAgentService
        QuestionEditTab->>QuestionEditTab: Remove from internalQuestions
        QuestionEditTab-->>User: Hiển thị danh sách và thông báo thành công
    end
    
    deactivate QuestionEditTab
    deactivate EditTab
```

---

## 4. Luyện tập với Question

```mermaid
sequenceDiagram
    actor User
    participant QuestionTab as QuestionTab.js
    participant DocAgentService as DocAgentService
    participant QuestionRoutes as question_routes
    participant QuestionService as QuestionService

    User->>QuestionTab: Click tab "Question"
    activate QuestionTab
    
    QuestionTab->>DocAgentService: getQuestionsBySession()
    activate DocAgentService
    
    DocAgentService->>QuestionRoutes: GET /question/questions/by-session/{sessionId}
    activate QuestionRoutes
    
    QuestionRoutes->>QuestionService: get_questions_by_session()
    activate QuestionService
    
    QuestionService-->>QuestionRoutes: [QuestionResponse]
    deactivate QuestionService
    
    QuestionRoutes-->>DocAgentService: [Question]
    deactivate QuestionRoutes
    
    DocAgentService-->>QuestionTab: ServiceResult~Question[]~
    deactivate DocAgentService
    
    QuestionTab->>QuestionTab: Display first Question
    QuestionTab-->>User: Hiển thị Question đầu tiên
    
    loop Cho đến Question cuối cùng
        User->>QuestionTab: Chọn đáp án / Nhập câu trả lời
        QuestionTab-->>User: Cập nhật trạng thái form
        QuestionTab->>QuestionTab: Save user answer
        
        User->>QuestionTab: Click "Tiếp theo"
        QuestionTab->>QuestionTab: Move to next Question
        QuestionTab-->>User: Hiển thị Question tiếp theo
    end
    
    User->>QuestionTab: Click "Hoàn thành"
    QuestionTab->>QuestionTab: Calculate score<br/>Compare answers<br/>Set showResults=true
    QuestionTab-->>User: Hiển thị kết quả
    deactivate QuestionTab
```

---

## 5. Tạo Flashcard mới

```mermaid
sequenceDiagram
    actor User
    participant EditTab as EditTab.js
    participant FlashcardEditTab as FlashcardEditTab.js
    participant FlashcardEditItem as FlashcardEditItem.tsx
    participant DocAgentService as DocAgentService
    participant FlashcardRoutes as flashcard_routes
    participant FlashcardService as FlashcardService

    User->>EditTab: Click tab "Edit"
    activate EditTab
    EditTab-->>User: Hiển thị giao diện Edit
    
    User->>EditTab: Click tab con "Flashcard"
    EditTab->>FlashcardEditTab: Khởi tạo FlashcardEditTab
    activate FlashcardEditTab
    
    FlashcardEditTab->>DocAgentService: getFlashcardsBySession()
    activate DocAgentService
    DocAgentService->>FlashcardRoutes: GET /question/flashcards/by-session/{sessionId}
    activate FlashcardRoutes
    FlashcardRoutes->>FlashcardService: get_flashcards_by_session()
    activate FlashcardService
    FlashcardService-->>FlashcardRoutes: [FlashcardResponse]
    deactivate FlashcardService
    FlashcardRoutes-->>DocAgentService: [Flashcard]
    deactivate FlashcardRoutes
    DocAgentService-->>FlashcardEditTab: ServiceResult~Flashcard[]~
    deactivate DocAgentService
    
    FlashcardEditTab-->>User: Hiển thị danh sách Flashcard
    
    User->>FlashcardEditTab: Click "Thêm flashcard mới"
    FlashcardEditTab->>FlashcardEditTab: Create temp flashcard
    FlashcardEditTab->>FlashcardEditItem: Render in edit mode
    activate FlashcardEditItem
    FlashcardEditItem-->>User: Hiển thị form tạo Flashcard mới
    
    User->>FlashcardEditItem: Nhập thông tin và click "Lưu"
    FlashcardEditItem->>FlashcardEditItem: Validate form
    FlashcardEditItem->>FlashcardEditTab: onSave(updatedFlashcard)
    deactivate FlashcardEditItem
    
    FlashcardEditTab->>DocAgentService: createFlashcard()
    activate DocAgentService
    
    DocAgentService->>FlashcardRoutes: POST /question/flashcards
    activate FlashcardRoutes
    
    FlashcardRoutes->>FlashcardService: create_flashcard()
    activate FlashcardService
    
    FlashcardService->>FlashcardService: Create Flashcard<br/>db.commit()
    
    FlashcardService-->>FlashcardRoutes: FlashcardResponse
    deactivate FlashcardService
    
    FlashcardRoutes-->>DocAgentService: FlashcardResponse
    deactivate FlashcardRoutes
    
    DocAgentService-->>FlashcardEditTab: ServiceResult~Flashcard~
    deactivate DocAgentService
    
    FlashcardEditTab->>FlashcardEditTab: Replace temp with real flashcard
    FlashcardEditTab-->>User: Hiển thị danh sách và thông báo thành công
    deactivate FlashcardEditTab
    deactivate EditTab
```

---

## 6. Chỉnh sửa Flashcard

```mermaid
sequenceDiagram
    actor User
    participant EditTab as EditTab.js
    participant FlashcardEditTab as FlashcardEditTab.js
    participant FlashcardEditItem as FlashcardEditItem.tsx
    participant DocAgentService as DocAgentService
    participant FlashcardRoutes as flashcard_routes
    participant FlashcardService as FlashcardService

    User->>EditTab: Click tab "Edit"
    activate EditTab
    EditTab-->>User: Hiển thị giao diện Edit
    
    User->>EditTab: Click tab con "Flashcard"
    EditTab->>FlashcardEditTab: Khởi tạo FlashcardEditTab
    activate FlashcardEditTab
    
    FlashcardEditTab->>DocAgentService: getFlashcardsBySession()
    activate DocAgentService
    DocAgentService->>FlashcardRoutes: GET /question/flashcards/by-session/{sessionId}
    activate FlashcardRoutes
    FlashcardRoutes->>FlashcardService: get_flashcards_by_session()
    activate FlashcardService
    FlashcardService-->>FlashcardRoutes: [FlashcardResponse]
    deactivate FlashcardService
    FlashcardRoutes-->>DocAgentService: [Flashcard]
    deactivate FlashcardRoutes
    DocAgentService-->>FlashcardEditTab: ServiceResult~Flashcard[]~
    deactivate DocAgentService
    
    FlashcardEditTab-->>User: Hiển thị danh sách Flashcard
    
    User->>FlashcardEditTab: Click icon chỉnh sửa
    FlashcardEditTab->>FlashcardEditTab: Set editingId
    FlashcardEditTab->>FlashcardEditItem: Render in edit mode
    activate FlashcardEditItem
    FlashcardEditItem-->>User: Hiển thị form chỉnh sửa Flashcard
    
    User->>FlashcardEditItem: Nhập thông tin và click "Lưu"
    FlashcardEditItem->>FlashcardEditItem: Validate form
    FlashcardEditItem->>FlashcardEditTab: onSave(id, updatedFlashcard)
    deactivate FlashcardEditItem
    
    FlashcardEditTab->>DocAgentService: updateFlashcard()
    activate DocAgentService
    
    DocAgentService->>FlashcardRoutes: PUT /question/flashcards/{flashcardId}
    activate FlashcardRoutes
    
    FlashcardRoutes->>FlashcardService: update_flashcard()
    activate FlashcardService
    
    FlashcardService->>FlashcardService: Update Flashcard<br/>db.commit()
    
    FlashcardService-->>FlashcardRoutes: FlashcardResponse
    deactivate FlashcardService
    
    FlashcardRoutes-->>DocAgentService: FlashcardResponse
    deactivate FlashcardRoutes
    
    DocAgentService-->>FlashcardEditTab: ServiceResult~Flashcard~
    deactivate DocAgentService
    
    FlashcardEditTab->>FlashcardEditTab: Update in list<br/>Set editingId=null
    FlashcardEditTab-->>User: Hiển thị danh sách và thông báo thành công
    deactivate FlashcardEditTab
    deactivate EditTab
```

---

## 7. Xóa Flashcard

```mermaid
sequenceDiagram
    actor User
    participant EditTab as EditTab.js
    participant FlashcardEditTab as FlashcardEditTab.js
    participant DocAgentService as DocAgentService
    participant FlashcardRoutes as flashcard_routes
    participant FlashcardService as FlashcardService

    User->>EditTab: Click tab "Edit"
    activate EditTab
    EditTab-->>User: Hiển thị giao diện Edit
    
    User->>EditTab: Click tab con "Flashcard"
    EditTab->>FlashcardEditTab: Khởi tạo FlashcardEditTab
    activate FlashcardEditTab
    
    FlashcardEditTab->>DocAgentService: getFlashcardsBySession()
    activate DocAgentService
    DocAgentService->>FlashcardRoutes: GET /question/flashcards/by-session/{sessionId}
    activate FlashcardRoutes
    FlashcardRoutes->>FlashcardService: get_flashcards_by_session()
    activate FlashcardService
    FlashcardService-->>FlashcardRoutes: [FlashcardResponse]
    deactivate FlashcardService
    FlashcardRoutes-->>DocAgentService: [Flashcard]
    deactivate FlashcardRoutes
    DocAgentService-->>FlashcardEditTab: ServiceResult~Flashcard[]~
    deactivate DocAgentService
    
    FlashcardEditTab-->>User: Hiển thị danh sách Flashcard
    
    User->>FlashcardEditTab: Click icon xóa
    User->>FlashcardEditTab: Xác nhận xóa
    
    alt Nếu id bắt đầu bằng "temp-"
        FlashcardEditTab->>FlashcardEditTab: Remove from internalFlashcards
        FlashcardEditTab-->>User: Hiển thị danh sách Flashcard được cập nhật
    else Nếu là real id
        FlashcardEditTab->>DocAgentService: deleteFlashcard(id)
        activate DocAgentService
        DocAgentService->>FlashcardRoutes: DELETE /question/flashcards/{flashcardId}
        activate FlashcardRoutes
        FlashcardRoutes->>FlashcardService: delete_flashcard()
        activate FlashcardService
        FlashcardService->>FlashcardService: Delete flashcard
        FlashcardService-->>FlashcardRoutes: MessageResponse
        deactivate FlashcardService
        FlashcardRoutes-->>DocAgentService: MessageResponse
        deactivate FlashcardRoutes
        DocAgentService-->>FlashcardEditTab: ServiceResult
        deactivate DocAgentService
        FlashcardEditTab->>FlashcardEditTab: Remove from internalFlashcards
        FlashcardEditTab-->>User: Hiển thị danh sách và thông báo thành công
    end
    
    deactivate FlashcardEditTab
    deactivate EditTab
```

---

## 8. Luyện tập với Flashcard

```mermaid
sequenceDiagram
    actor User
    participant FlashcardTab as FlashcardTab.js
    participant DocAgentService as DocAgentService
    participant FlashcardRoutes as flashcard_routes
    participant FlashcardService as FlashcardService

    User->>FlashcardTab: Click tab "Flashcard"
    activate FlashcardTab
    
    FlashcardTab->>DocAgentService: getFlashcardsBySession()
    activate DocAgentService
    
    DocAgentService->>FlashcardRoutes: GET /question/flashcards/by-session/{sessionId}
    activate FlashcardRoutes
    
    FlashcardRoutes->>FlashcardService: get_flashcards_by_session()
    activate FlashcardService
    
    FlashcardService-->>FlashcardRoutes: [FlashcardResponse]
    deactivate FlashcardService
    
    FlashcardRoutes-->>DocAgentService: [Flashcard]
    deactivate FlashcardRoutes
    
    DocAgentService-->>FlashcardTab: ServiceResult~Flashcard[]~
    deactivate DocAgentService
    
    FlashcardTab->>FlashcardTab: Display first flashcard<br/>Set isFlipped=false
    FlashcardTab-->>User: Hiển thị Flashcard đầu tiên
    
    loop Cho đến Flashcard cuối cùng
        User->>FlashcardTab: Click Flashcard để lật
        FlashcardTab->>FlashcardTab: Toggle isFlipped
        FlashcardTab-->>User: Hiển thị mặt đáp án
        
        User->>FlashcardTab: Click icon mũi tên trái/phải
        FlashcardTab->>FlashcardTab: Update currentIndex<br/>Reset isFlipped=false
        FlashcardTab-->>User: Hiển thị Flashcard trước/sau
    end
    
    deactivate FlashcardTab
```
