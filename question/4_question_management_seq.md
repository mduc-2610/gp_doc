# Sequence Diagram - Quản lý Question và Flashcard

## 1. Tạo Flashcard mới

```mermaid
sequenceDiagram
    actor User
    participant FlashcardEditTab as FlashcardEditTab.tsx
    participant FlashcardEditItem as FlashcardEditItem.tsx
    participant DocAgentService as DocAgentService
    participant QuestionRoutes as question_routes.py
    participant QuestionService as QuestionService
    participant Flashcard as Flashcard

    User->>FlashcardEditTab: Click "Thêm thẻ"
    activate FlashcardEditTab
    
    FlashcardEditTab->>FlashcardEditItem: addNewFlashcard()
    activate FlashcardEditItem
    FlashcardEditItem-->>User: Hiển thị dialog
    deactivate FlashcardEditItem
    
    User->>FlashcardEditItem: Nhập thông tin và click "Lưu"
    activate FlashcardEditItem
    
    FlashcardEditItem->>FlashcardEditItem: validate()
    FlashcardEditItem->>FlashcardEditItem: onSave()
    FlashcardEditItem->>FlashcardEditTab: gọi
    deactivate FlashcardEditItem
    
    FlashcardEditTab->>FlashcardEditTab: handleCreateFlashcard()
    FlashcardEditTab->>DocAgentService: gọi
    activate DocAgentService
    
    DocAgentService->>DocAgentService: createFlashcard()
    DocAgentService->>QuestionRoutes: POST "/question/flashcard"
    activate QuestionRoutes
    
    QuestionRoutes->>QuestionService: create_flashcard()
    activate QuestionService
    
    QuestionService->>QuestionService: gọi
    QuestionService->>Flashcard: gọi
    activate Flashcard
    
    Flashcard->>Flashcard: Flashcard()
    Flashcard-->>QuestionService: trả về
    deactivate Flashcard
    
    QuestionService-->>QuestionRoutes: trả về
    deactivate QuestionService
    
    QuestionRoutes-->>DocAgentService: trả về
    deactivate QuestionRoutes
    
    DocAgentService-->>FlashcardEditTab: trả về
    deactivate DocAgentService
    
    FlashcardEditTab-->>User: Hiển thị
    deactivate FlashcardEditTab
```

---

## 2. Xóa Flashcard

```mermaid
sequenceDiagram
    actor User
    participant FlashcardEditTab as FlashcardEditTab.tsx
    participant DocAgentService as DocAgentService
    participant QuestionRoutes as question_routes.py
    participant QuestionService as QuestionService
    participant MessageResponse as MessageResponse

    User->>FlashcardEditTab: Click icon xóa Flashcard item
    activate FlashcardEditTab
    
    FlashcardEditTab->>FlashcardEditTab: handleDeleteFlashcard()
    FlashcardEditTab->>DocAgentService: gọi
    activate DocAgentService
    
    DocAgentService->>DocAgentService: deleteFlashcard()
    DocAgentService->>QuestionRoutes: DELETE "/question/flashcard/{flashcard_id}"
    activate QuestionRoutes
    
    QuestionRoutes->>QuestionService: delete_flashcard()
    activate QuestionService
    
    QuestionService->>QuestionService: gọi
    QuestionService->>MessageResponse: gọi
    activate MessageResponse
    
    MessageResponse->>MessageResponse: MessageResponse()
    MessageResponse-->>QuestionService: trả về
    deactivate MessageResponse
    
    QuestionService-->>QuestionRoutes: trả về
    deactivate QuestionService
    
    QuestionRoutes-->>DocAgentService: trả về
    deactivate QuestionRoutes
    
    DocAgentService-->>FlashcardEditTab: trả về
    deactivate DocAgentService
    
    FlashcardEditTab-->>User: Hiển thị
    deactivate FlashcardEditTab
```

---

## 3. Xem danh sách Flashcard (Edit Tab)

```mermaid
sequenceDiagram
    actor User
    participant EditTab as EditTab.tsx
    participant FlashcardEditTab as FlashcardEditTab.tsx
    participant DocAgentService as DocAgentService
    participant QuestionRoutes as question_routes.py
    participant QuestionService as QuestionService
    participant Flashcard as Flashcard

    User->>EditTab: Click "Chỉnh sửa" tab
    activate EditTab
    EditTab-->>User: Hiển thị
    deactivate EditTab
    
    User->>EditTab: Click "Thẻ ghi nhớ" tab
    activate EditTab
    
    EditTab->>FlashcardEditTab: gọi
    activate FlashcardEditTab
    
    FlashcardEditTab->>DocAgentService: gọi
    activate DocAgentService
    
    DocAgentService->>DocAgentService: getFlashcardsBySession()
    DocAgentService->>QuestionRoutes: GET "/question/flashcard/by-session/{session_id}"
    activate QuestionRoutes
    
    QuestionRoutes->>QuestionService: get_flashcards_by_session()
    activate QuestionService
    
    QuestionService->>QuestionService: gọi
    QuestionService->>Flashcard: gọi
    activate Flashcard
    
    Flashcard->>Flashcard: Flashcard()
    Flashcard-->>QuestionService: trả về
    deactivate Flashcard
    
    QuestionService-->>QuestionRoutes: trả về
    deactivate QuestionService
    
    QuestionRoutes-->>DocAgentService: trả về
    deactivate QuestionRoutes
    
    DocAgentService-->>FlashcardEditTab: trả về
    deactivate DocAgentService
    
    FlashcardEditTab-->>User: Hiển thị
    deactivate FlashcardEditTab
    deactivate EditTab
```

---

## 4. Xem danh sách Flashcard (Practice Tab)

```mermaid
sequenceDiagram
    actor User
    participant DocAgentPage as DocAgentPage.tsx
    participant FlashcardTab as FlashcardTab.tsx
    participant DocAgentService as DocAgentService
    participant QuestionRoutes as question_routes.py
    participant QuestionService as QuestionService
    participant Flashcard as Flashcard

    User->>DocAgentPage: Click "Thẻ ghi nhớ" tab
    activate DocAgentPage
    
    DocAgentPage->>FlashcardTab: gọi
    activate FlashcardTab
    
    FlashcardTab->>DocAgentService: gọi
    activate DocAgentService
    
    DocAgentService->>DocAgentService: getFlashcardsBySession()
    DocAgentService->>QuestionRoutes: GET "/question/flashcard/by-session/{session_id}"
    activate QuestionRoutes
    
    QuestionRoutes->>QuestionService: get_flashcards_by_session()
    activate QuestionService
    
    QuestionService->>QuestionService: gọi
    QuestionService->>Flashcard: gọi
    activate Flashcard
    
    Flashcard->>Flashcard: Flashcard()
    Flashcard-->>QuestionService: trả về
    deactivate Flashcard
    
    QuestionService-->>QuestionRoutes: trả về
    deactivate QuestionService
    
    QuestionRoutes-->>DocAgentService: trả về
    deactivate QuestionRoutes
    
    DocAgentService-->>FlashcardTab: trả về
    deactivate DocAgentService
    
    FlashcardTab-->>User: Hiển thị
    deactivate FlashcardTab
    
    User->>FlashcardTab: Nhấn key left, right, enter
    activate FlashcardTab
    
    FlashcardTab->>FlashcardTab: handleKeyPress()
    
    alt sd key == "ArrowLeft"
        FlashcardTab->>FlashcardTab: goToPrevious()
    else sd key == "ArrowRight"
        FlashcardTab->>FlashcardTab: goToNext()
    else sd key == "Enter"
        FlashcardTab->>FlashcardTab: flipCard()
    end
    
    FlashcardTab-->>User: Hiển thị
    deactivate FlashcardTab
    deactivate DocAgentPage
```

---

## 5. Chỉnh sửa Flashcard

```mermaid
sequenceDiagram
    actor User
    participant FlashcardEditTab as FlashcardEditTab.tsx
    participant FlashcardEditItem as FlashcardEditItem.tsx
    participant DocAgentService as DocAgentService
    participant QuestionRoutes as question_routes.py
    participant QuestionService as QuestionService
    participant Flashcard as Flashcard

    User->>FlashcardEditTab: Click 1 Flashcard item
    activate FlashcardEditTab
    
    FlashcardEditTab->>FlashcardEditItem: handleEdit()
    activate FlashcardEditItem
    FlashcardEditItem-->>User: Hiển thị
    deactivate FlashcardEditItem
    
    User->>FlashcardEditItem: Nhập thông tin và click "Lưu"
    activate FlashcardEditItem
    
    FlashcardEditItem->>FlashcardEditItem: validate()
    FlashcardEditItem->>FlashcardEditItem: onSave()
    FlashcardEditItem->>FlashcardEditTab: gọi
    deactivate FlashcardEditItem
    
    FlashcardEditTab->>FlashcardEditTab: handleUpdateFlashcard()
    FlashcardEditTab->>DocAgentService: gọi
    activate DocAgentService
    
    DocAgentService->>DocAgentService: updateFlashcard()
    DocAgentService->>QuestionRoutes: PUT "/question/flashcard/{flashcard_id}"
    activate QuestionRoutes
    
    QuestionRoutes->>QuestionService: update_flashcard()
    activate QuestionService
    
    QuestionService->>QuestionService: gọi
    QuestionService->>Flashcard: gọi
    activate Flashcard
    
    Flashcard->>Flashcard: Flashcard()
    Flashcard-->>QuestionService: trả về
    deactivate Flashcard
    
    QuestionService-->>QuestionRoutes: trả về
    deactivate QuestionService
    
    QuestionRoutes-->>DocAgentService: trả về
    deactivate QuestionRoutes
    
    DocAgentService-->>FlashcardEditTab: trả về
    deactivate DocAgentService
    
    FlashcardEditTab-->>User: Hiển thị
    deactivate FlashcardEditTab
```

---

## 6. Tạo Question mới

```mermaid
sequenceDiagram
    actor User
    participant QuestionEditTab as QuestionEditTab.tsx
    participant QuestionEditItem as QuestionEditItem.tsx
    participant DocAgentService as DocAgentService
    participant QuestionRoutes as question_routes.py
    participant QuestionService as QuestionService
    participant Question as Question

    User->>QuestionEditTab: Click "Thêm câu hỏi"
    activate QuestionEditTab
    
    QuestionEditTab->>QuestionEditItem: addNewQuestion()
    activate QuestionEditItem
    QuestionEditItem-->>User: Hiển thị dialog
    deactivate QuestionEditItem
    
    User->>QuestionEditItem: Nhập thông tin và click "Lưu"
    activate QuestionEditItem
    
    QuestionEditItem->>QuestionEditItem: validate()
    QuestionEditItem->>QuestionEditItem: onSave()
    QuestionEditItem->>QuestionEditTab: gọi
    deactivate QuestionEditItem
    
    QuestionEditTab->>QuestionEditTab: handleCreateQuestion()
    QuestionEditTab->>DocAgentService: gọi
    activate DocAgentService
    
    DocAgentService->>DocAgentService: createQuestion()
    DocAgentService->>QuestionRoutes: POST "/question/question"
    activate QuestionRoutes
    
    QuestionRoutes->>QuestionService: create_question()
    activate QuestionService
    
    QuestionService->>QuestionService: gọi
    QuestionService->>Question: gọi
    activate Question
    
    Question->>Question: Question()
    Question-->>QuestionService: trả về
    deactivate Question
    
    QuestionService-->>QuestionRoutes: trả về
    deactivate QuestionService
    
    QuestionRoutes-->>DocAgentService: trả về
    deactivate QuestionRoutes
    
    DocAgentService-->>QuestionEditTab: trả về
    deactivate DocAgentService
    
    QuestionEditTab-->>User: Hiển thị
    deactivate QuestionEditTab
```

---

## 7. Xóa Question

```mermaid
sequenceDiagram
    actor User
    participant QuestionEditTab as QuestionEditTab.tsx
    participant DocAgentService as DocAgentService
    participant QuestionRoutes as question_routes.py
    participant QuestionService as QuestionService
    participant MessageResponse as MessageResponse

    User->>QuestionEditTab: Click icon xóa Question item
    activate QuestionEditTab
    
    QuestionEditTab->>QuestionEditTab: handleDeleteQuestion()
    QuestionEditTab->>DocAgentService: gọi
    activate DocAgentService
    
    DocAgentService->>DocAgentService: deleteQuestion()
    DocAgentService->>QuestionRoutes: DELETE "/question/question/{question_id}"
    activate QuestionRoutes
    
    QuestionRoutes->>QuestionService: delete_question()
    activate QuestionService
    
    QuestionService->>QuestionService: gọi
    QuestionService->>MessageResponse: gọi
    activate MessageResponse
    
    MessageResponse->>MessageResponse: MessageResponse()
    MessageResponse-->>QuestionService: trả về
    deactivate MessageResponse
    
    QuestionService-->>QuestionRoutes: trả về
    deactivate QuestionService
    
    QuestionRoutes-->>DocAgentService: trả về
    deactivate QuestionRoutes
    
    DocAgentService-->>QuestionEditTab: trả về
    deactivate DocAgentService
    
    QuestionEditTab-->>User: Hiển thị
    deactivate QuestionEditTab
```

---

## 8. Xem danh sách Question (Edit Tab)

```mermaid
sequenceDiagram
    actor User
    participant EditTab as EditTab.tsx
    participant QuestionEditTab as QuestionEditTab.tsx
    participant DocAgentService as DocAgentService
    participant QuestionRoutes as question_routes.py
    participant QuestionService as QuestionService
    participant Question as Question

    User->>EditTab: Click "Chỉnh sửa" tab
    activate EditTab
    EditTab-->>User: Hiển thị
    deactivate EditTab
    
    User->>EditTab: Click "Câu hỏi" tab
    activate EditTab
    
    EditTab->>QuestionEditTab: gọi
    activate QuestionEditTab
    
    QuestionEditTab->>DocAgentService: gọi
    activate DocAgentService
    
    DocAgentService->>DocAgentService: getQuestionsBySession()
    DocAgentService->>QuestionRoutes: GET "/question/question/by-session/{session_id}"
    activate QuestionRoutes
    
    QuestionRoutes->>QuestionService: get_questions_by_session()
    activate QuestionService
    
    QuestionService->>QuestionService: gọi
    QuestionService->>Question: gọi
    activate Question
    
    Question->>Question: Question()
    Question-->>QuestionService: trả về
    deactivate Question
    
    QuestionService-->>QuestionRoutes: trả về
    deactivate QuestionService
    
    QuestionRoutes-->>DocAgentService: trả về
    deactivate QuestionRoutes
    
    DocAgentService-->>QuestionEditTab: trả về
    deactivate DocAgentService
    
    QuestionEditTab-->>User: Hiển thị
    deactivate QuestionEditTab
    deactivate EditTab
```

---

## 9. Xem danh sách Question (Practice Tab)

```mermaid
sequenceDiagram
    actor User
    participant EditTab as EditTab.tsx
    participant QuestionEditTab as QuestionEditTab.tsx
    participant QuestionResultTab as QuestionResultTab.tsx
    participant DocAgentService as DocAgentService
    participant QuestionRoutes as question_routes.py
    participant QuestionService as QuestionService
    participant Question as Question

    User->>EditTab: Click "Câu hỏi" tab
    activate EditTab
    
    EditTab->>QuestionEditTab: gọi
    activate QuestionEditTab
    
    QuestionEditTab->>DocAgentService: gọi
    activate DocAgentService
    
    DocAgentService->>DocAgentService: getQuestionsBySession()
    DocAgentService->>QuestionRoutes: GET "/question/question/by-session/{session_id}"
    activate QuestionRoutes
    
    QuestionRoutes->>QuestionService: get_questions_by_session()
    activate QuestionService
    
    QuestionService->>QuestionService: gọi
    QuestionService->>Question: gọi
    activate Question
    
    Question->>Question: Question()
    Question-->>QuestionService: trả về
    deactivate Question
    
    QuestionService-->>QuestionRoutes: trả về
    deactivate QuestionService
    
    QuestionRoutes-->>DocAgentService: trả về
    deactivate QuestionRoutes
    
    DocAgentService-->>QuestionEditTab: trả về
    deactivate DocAgentService
    
    QuestionEditTab-->>User: Hiển thị
    deactivate QuestionEditTab
    
    User->>QuestionEditTab: Nhấn key left, right, enter
    activate QuestionEditTab
    
    QuestionEditTab->>QuestionEditTab: handleKeyPress()
    
    alt sd key == "ArrowLeft"
        QuestionEditTab->>QuestionEditTab: goToPrevious()
    else sd key == "ArrowRight"
        QuestionEditTab->>QuestionEditTab: goToNext()
    else sd key == "Enter"
        alt [needResults == true]
            QuestionEditTab->>QuestionEditTab: redoPractice()
            QuestionEditTab->>QuestionEditTab: goToNext()
        end
    end
    
    QuestionEditTab-->>User: Hiển thị
    deactivate QuestionEditTab
    
    User->>QuestionEditTab: Click "Hoàn thành" hoặc nhấn key enter
    activate QuestionEditTab
    
    QuestionEditTab->>QuestionEditTab: calculateScore()
    QuestionEditTab-->>User: Hiển thị
    deactivate QuestionEditTab
    deactivate EditTab
```

---

## 10. Chỉnh sửa Question

```mermaid
sequenceDiagram
    actor User
    participant EditTab as EditTab.tsx
    participant QuestionEditTab as QuestionEditTab.tsx
    participant QuestionEditItem as QuestionEditItem.tsx
    participant DocAgentService as DocAgentService
    participant QuestionRoutes as question_routes.py
    participant QuestionService as QuestionService
    participant Question as Question

    User->>EditTab: Click 1 Question item
    activate EditTab
    
    EditTab->>QuestionEditTab: handleEdit()
    activate QuestionEditTab
    
    QuestionEditTab->>QuestionEditItem: gọi
    activate QuestionEditItem
    QuestionEditItem-->>User: Hiển thị
    deactivate QuestionEditItem
    
    User->>QuestionEditItem: Nhập thông tin và click "Lưu"
    activate QuestionEditItem
    
    QuestionEditItem->>QuestionEditItem: validate()
    QuestionEditItem->>QuestionEditItem: onSave()
    QuestionEditItem->>QuestionEditTab: gọi
    deactivate QuestionEditItem
    
    QuestionEditTab->>QuestionEditTab: handleUpdateQuestion()
    QuestionEditTab->>DocAgentService: gọi
    deactivate QuestionEditTab
    activate DocAgentService
    
    DocAgentService->>DocAgentService: updateQuestion()
    DocAgentService->>QuestionRoutes: PUT "/question/question/{question_id}"
    activate QuestionRoutes
    
    QuestionRoutes->>QuestionService: update_question()
    activate QuestionService
    
    QuestionService->>QuestionService: gọi
    QuestionService->>Question: gọi
    activate Question
    
    Question->>Question: Question()
    Question-->>QuestionService: trả về
    deactivate Question
    
    QuestionService-->>QuestionRoutes: trả về
    deactivate QuestionService
    
    QuestionRoutes-->>DocAgentService: trả về
    deactivate QuestionRoutes
    
    DocAgentService-->>EditTab: trả về
    deactivate DocAgentService
    
    EditTab-->>User: Hiển thị
    deactivate EditTab
```
