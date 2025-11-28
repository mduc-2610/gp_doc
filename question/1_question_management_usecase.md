
# Use Case Diagram - Quản lý Question và Flashcard

```mermaid
graph TB
    Actor["👤 User"]
    
    Login["Đăng nhập"]
    ListSessions["Xem danh sách Session<br/><b>extension points</b><br/>Xem chi tiết Session"]
    ViewSessionDetail["Xem chi tiết Session<br/><b>extension points</b><br/>Luyện tập với Flashcard<br/>Luyện tập với Question<br/>Xem danh sách Flashcard<br/>Xem danh sách Question<br/>Trích xuất Flashcard<br/>Trích xuất Question"]
    
    PracticeFlashcard["Luyện tập với Flashcard"]
    PracticeQuestion["Luyện tập với Question"]
    
    ViewFlashcardList["Xem danh sách Flashcard<br/><b>extension points</b><br/>Thêm Flashcard<br/>Chỉnh sửa Flashcard<br/>Xóa Flashcard"]
    ViewQuestionList["Xem danh sách Question<br/><b>extension points</b><br/>Thêm Question<br/>Chỉnh sửa Question<br/>Xóa Question"]
    
    ExportFlashcard["Trích xuất Flashcard"]
    ExportQuestion["Trích xuất Question"]
    
    CreateFlashcard["Thêm Flashcard"]
    EditFlashcard["Chỉnh sửa Flashcard"]
    DeleteFlashcard["Xóa Flashcard"]
    
    CreateQuestion["Thêm Question"]
    EditQuestion["Chỉnh sửa Question"]
    DeleteQuestion["Xóa Question"]
    
    Actor -->|Truy cập| Login
    Login -.->|<<Include>>| ListSessions
    ListSessions -->|Thực hiện| Actor
    
    ListSessions -.->|<<Extend>>| ViewSessionDetail
    ViewSessionDetail -->|Thực hiện| Actor
    
    ViewSessionDetail -.->|<<Extend>>| PracticeFlashcard
    PracticeFlashcard -->|Thực hiện| Actor
    
    ViewSessionDetail -.->|<<Extend>>| PracticeQuestion
    PracticeQuestion -->|Thực hiện| Actor
    
    ViewSessionDetail -.->|<<Extend>>| ExportFlashcard
    ExportFlashcard -->|Thực hiện| Actor
    
    ViewSessionDetail -.->|<<Extend>>| ExportQuestion
    ExportQuestion -->|Thực hiện| Actor
    
    ViewSessionDetail -.->|<<Extend>>| ViewFlashcardList
    ViewFlashcardList -->|Thực hiện| Actor
    
    ViewFlashcardList -.->|<<Extend>>| CreateFlashcard
    CreateFlashcard -->|Thực hiện| Actor
    
    ViewFlashcardList -.->|<<Extend>>| EditFlashcard
    EditFlashcard -->|Thực hiện| Actor
    
    ViewFlashcardList -.->|<<Extend>>| DeleteFlashcard
    DeleteFlashcard -->|Thực hiện| Actor
    
    ViewSessionDetail -.->|<<Extend>>| ViewQuestionList
    ViewQuestionList -->|Thực hiện| Actor
    
    ViewQuestionList -.->|<<Extend>>| CreateQuestion
    CreateQuestion -->|Thực hiện| Actor
    
    ViewQuestionList -.->|<<Extend>>| EditQuestion
    EditQuestion -->|Thực hiện| Actor
    
    ViewQuestionList -.->|<<Extend>>| DeleteQuestion
    DeleteQuestion -->|Thực hiện| Actor
    
    style Actor fill:#e1f5ff
    style Login fill:#fff3e0
    style ListSessions fill:#f3e5f5
    style ViewSessionDetail fill:#fce4ec
    style PracticeFlashcard fill:#fce4ec
    style PracticeQuestion fill:#fce4ec
    style ExportFlashcard fill:#fff9c4
    style ExportQuestion fill:#fff9c4
    style ViewFlashcardList fill:#e0f2f1
    style ViewQuestionList fill:#e0f2f1
    style CreateFlashcard fill:#e8f5e9
    style EditFlashcard fill:#e8f5e9
    style DeleteFlashcard fill:#ffebee
    style CreateQuestion fill:#e8f5e9
    style EditQuestion fill:#e8f5e9
    style DeleteQuestion fill:#ffebee
```

**Ghi chú:**
- Đăng nhập là điều kiện tiên quyết để truy cập hệ thống.
- Xem danh sách Session là bước cần thiết để có thể xem chi tiết Session.
- Xem chi tiết Session có các extension points cho phép truy cập danh sách, luyện tập và trích xuất.
- Xem danh sách Question/Flashcard có các extension points cho các thao tác chỉnh sửa, tạo mới, xóa.
- Các thao tác Luyện tập được thực hiện trong giao diện xem danh sách tương ứng.
- Các thao tác Trích xuất cho phép xuất Question hoặc Flashcard ra file Excel.
- Xem danh sách Session là bước cần thiết để có thể xem chi tiết Session.
- Xem chi tiết Session có các extension points cho phép truy cập danh sách và luyện tập.
- Xem danh sách Question/Flashcard có các extension points cho các thao tác chỉnh sửa, tạo mới, xóa.
- Các thao tác Luyện tập được thực hiện trong giao diện xem danh sách tương ứng.

