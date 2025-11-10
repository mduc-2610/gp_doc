# Sequence Scenario - Quản lý Question và Flashcard

## Kịch bản 1: Tạo Question mới

1. Tại giao diện Session cụ thể, User click vào tab "Edit"
2. EditTab.js khởi tạo và hiển thị giao diện
3. User click vào tab con "Question" trong EditTab
4. QuestionEditTab.js gọi `DocAgentService.getQuestionsBySession()` để lấy danh sách Question
5. DocAgentService gửi request GET tới endpoint `/question/questions/by-session/{sessionId}`
6. Backend route nhận request và QuestionService được gọi hàm `get_questions_by_session()`
7. QuestionService truy vấn cơ sở dữ liệu trả danh sách Question objects
8. question_routes.py trả về danh sách QuestionResponse JSON
9. DocAgentService nhận danh sách từ response và đóng gói với ServiceResult<Question[]>
10. QuestionEditTab.js nhận ServiceResult<Question[]> và hiển thị danh sách Question
11. User click nút "Thêm Question mới"
12. QuestionEditTab.js tạo một object Question tạm thời và hiển thị ở chế độ edit
13. User nhập thông tin và click nút "Lưu"
14. QuestionEditTab.js gọi `DocAgentService.createQuestion()`
15. DocAgentService gửi POST request tới `/question/questions` endpoint
16. Backend route nhận request và QuestionService được gọi `create_question()`
17. QuestionService tạo Question model mới với QuestionAnswers và lưu vào database
18. QuestionService trả QuestionResponse
19. question_routes.py trả về QuestionResponse JSON
20. DocAgentService nhận response và đóng gói với ServiceResult<Question>
21. QuestionEditTab.js cập nhật danh sách Question bằng dữ liệu mới từ response
22. Component re-render và hiển thị danh sách Question được cập nhật
23. Toast success notification được hiển thị: "Tạo Question thành công"

---

## Kịch bản 2: Chỉnh sửa Question

1. Tại giao diện Session cụ thể, User click vào tab "Edit"
2. EditTab.js khởi tạo và hiển thị giao diện
3. User click vào tab con "Question"
4. QuestionEditTab.js gọi `DocAgentService.getQuestionsBySession()` để lấy danh sách Question
5. DocAgentService gửi request GET tới endpoint `/question/questions/by-session/{sessionId}`
6. Backend route nhận request và QuestionService được gọi hàm `get_questions_by_session()`
7. QuestionService truy vấn cơ sở dữ liệu trả danh sách Question objects
8. question_routes.py trả về danh sách QuestionResponse JSON
9. DocAgentService nhận danh sách từ response và đóng gói với ServiceResult<Question[]>
10. QuestionEditTab.js nhận ServiceResult<Question[]> và hiển thị danh sách Question
11. User click icon chỉnh sửa trên một Question
12. QuestionEditTab.js hiển thị QuestionEditItem ở chế độ edit với dữ liệu Question đó
13. User nhập thông tin và click nút "Lưu"
14. QuestionEditTab.js gọi `DocAgentService.updateQuestion()`
15. DocAgentService gửi PUT request tới `/question/questions/{questionId}`
16. Backend route nhận request và QuestionService được gọi `update_question()`
17. QuestionService lấy question hiện tại từ database, cập nhật các field và commit transaction
18. QuestionService trả QuestionResponse
19. question_routes.py trả về QuestionResponse JSON
20. DocAgentService nhận response và đóng gói với ServiceResult<Question>
21. QuestionEditTab.js cập nhật item trong danh sách Question
22. Component re-render và hiển thị danh sách Question được cập nhật
23. Toast success notification được hiển thị: "Cập nhật Question thành công"

---

## Kịch bản 3: Xóa Question

1. Tại giao diện Session cụ thể, User click vào tab "Edit"
2. EditTab.js khởi tạo và hiển thị giao diện
3. User click vào tab con "Question"
4. QuestionEditTab.js gọi `DocAgentService.getQuestionsBySession()` để lấy danh sách Question
5. DocAgentService gửi request GET tới endpoint `/question/questions/by-session/{sessionId}`
6. Backend route nhận request và QuestionService được gọi hàm `get_questions_by_session()`
7. QuestionService truy vấn cơ sở dữ liệu trả danh sách Question objects
8. question_routes.py trả về danh sách QuestionResponse JSON
9. DocAgentService nhận danh sách từ response và đóng gói với ServiceResult<Question[]>
10. QuestionEditTab.js nhận ServiceResult<Question[]> và hiển thị danh sách Question
11. User click icon xóa trên một Question
12. QuestionEditTab.js yêu cầu xác nhận xóa
13. User click "Xác nhận" để xác nhận xóa
14. QuestionEditTab.js gọi `DocAgentService.deleteQuestion()`
15. DocAgentService gửi DELETE request tới `/question/questions/{questionId}`
16. Backend route nhận request và QuestionService được gọi `delete_question()`
17. QuestionService xóa question khỏi database
18. QuestionService trả MessageResponse
19. question_routes.py trả về MessageResponse JSON
20. DocAgentService nhận response và đóng gói với ServiceResult<MessageResponse>
21. QuestionEditTab.js loại bỏ item khỏi danh sách Question
22. Component re-render, danh sách Question được cập nhật
23. Toast success notification được hiển thị: "Xóa Question thành công"

---

## Kịch bản 4: Luyện tập với Question

1. Tại giao diện Session cụ thể, User click vào tab "Question"
2. QuestionTab.js gọi `DocAgentService.getQuestionsBySession()` để lấy danh sách Question
3. DocAgentService gửi request GET tới endpoint `/question/questions/by-session/{sessionId}`
4. Backend route nhận request và QuestionService được gọi hàm `get_questions_by_session()`
5. QuestionService truy vấn cơ sở dữ liệu trả danh sách Question objects
6. question_routes.py trả về danh sách QuestionResponse JSON
7. DocAgentService nhận danh sách từ response và đóng gói với ServiceResult<Question[]>
8. QuestionTab.js nhận ServiceResult<Question[]> và hiển thị Question đầu tiên
9. User xem Question và các tùy chọn trả lời
10. User chọn tùy chọn hoặc nhập đáp án
11. User click nút "Tiếp theo"
12. QuestionTab.js lưu lựa chọn của User
13. QuestionTab.js cập nhật `currentIndex` tăng lên 1
14. Component re-render và hiển thị Question tiếp theo
15. User tiếp tục trả lời cho đến Question cuối cùng
16. Sau Question cuối cùng, User click nút "Hoàn thành"
17. QuestionTab.js tính điểm so sánh đáp án
18. Component re-render và hiển thị kết quả

---

## Kịch bản 5: Tạo Flashcard mới

1. Tại giao diện Session cụ thể, User click vào tab "Edit"
2. EditTab.js khởi tạo và hiển thị giao diện
3. User click vào tab con "Flashcard" trong EditTab
4. FlashcardEditTab.js gọi `DocAgentService.getFlashcardsBySession()` để lấy danh sách Flashcard
5. DocAgentService gửi request GET tới endpoint `/flashcard/flashcards/by-session/{sessionId}`
6. Backend route nhận request và FlashcardService được gọi hàm `get_flashcards_by_session()`
7. FlashcardService truy vấn cơ sở dữ liệu trả danh sách Flashcard objects
8. flashcard_routes.py trả về danh sách FlashcardResponse JSON
9. DocAgentService nhận danh sách từ response và đóng gói với ServiceResult<Flashcard[]>
10. FlashcardEditTab.js nhận ServiceResult<Flashcard[]> và hiển thị danh sách Flashcard
11. User click nút "Thêm flashcard mới"
12. FlashcardEditTab.js tạo một object Flashcard tạm thời và hiển thị ở chế độ edit
13. User nhập thông tin và click nút "Lưu"
14. FlashcardEditTab.js gọi `DocAgentService.createFlashcard()`
15. DocAgentService gửi POST request tới `/flashcard/flashcards`
16. Backend route nhận request và FlashcardService được gọi `create_flashcard()`
17. FlashcardService tạo Flashcard model mới và lưu vào database
18. FlashcardService trả FlashcardResponse
19. flashcard_routes.py trả về FlashcardResponse JSON
20. DocAgentService nhận response và đóng gói với ServiceResult<Flashcard>
21. FlashcardEditTab.js cập nhật danh sách Flashcard bằng dữ liệu mới từ response
22. Component re-render và hiển thị danh sách Flashcard được cập nhật
23. Toast success notification được hiển thị: "Tạo flashcard thành công"

---

## Kịch bản 6: Chỉnh sửa Flashcard

1. Tại giao diện Session cụ thể, User click vào tab "Edit"
2. EditTab.js khởi tạo và hiển thị giao diện
3. User click vào tab con "Flashcard" trong EditTab
4. FlashcardEditTab.js gọi `DocAgentService.getFlashcardsBySession()` để lấy danh sách Flashcard
5. DocAgentService gửi request GET tới endpoint `/flashcard/flashcards/by-session/{sessionId}`
6. Backend route nhận request và FlashcardService được gọi hàm `get_flashcards_by_session()`
7. FlashcardService truy vấn cơ sở dữ liệu trả danh sách Flashcard objects
8. flashcard_routes.py trả về danh sách FlashcardResponse JSON
9. DocAgentService nhận danh sách từ response và đóng gói với ServiceResult<Flashcard[]>
10. FlashcardEditTab.js nhận ServiceResult<Flashcard[]> và hiển thị danh sách Flashcard
11. User click icon chỉnh sửa trên một flashcard
12. FlashcardEditTab.js hiển thị FlashcardEditItem ở chế độ edit với dữ liệu flashcard đó
13. User nhập thông tin và click nút "Lưu"
14. FlashcardEditTab.js gọi `DocAgentService.updateFlashcard()`
15. DocAgentService gửi PUT request tới `/flashcard/flashcards/{flashcardId}`
16. Backend route nhận request và FlashcardService được gọi `update_flashcard()`
17. FlashcardService lấy flashcard hiện tại từ database, cập nhật các field và commit transaction
18. FlashcardService trả FlashcardResponse
19. flashcard_routes.py trả về FlashcardResponse JSON
20. DocAgentService nhận response và đóng gói với ServiceResult<Flashcard>
21. FlashcardEditTab.js cập nhật item trong danh sách Flashcard
22. Component re-render và hiển thị danh sách Flashcard được cập nhật
23. Toast success notification được hiển thị: "Cập nhật flashcard thành công"

---

## Kịch bản 7: Xóa Flashcard

1. Tại giao diện Session cụ thể, User click vào tab "Edit"
2. EditTab.js khởi tạo và hiển thị giao diện
3. User click vào tab con "Flashcard" trong EditTab
4. FlashcardEditTab.js gọi `DocAgentService.getFlashcardsBySession()` để lấy danh sách Flashcard
5. DocAgentService gửi request GET tới endpoint `/flashcard/flashcards/by-session/{sessionId}`
6. Backend route nhận request và FlashcardService được gọi hàm `get_flashcards_by_session()`
7. FlashcardService truy vấn cơ sở dữ liệu trả danh sách Flashcard objects
8. flashcard_routes.py trả về danh sách FlashcardResponse JSON
9. DocAgentService nhận danh sách từ response và đóng gói với ServiceResult<Flashcard[]>
10. FlashcardEditTab.js nhận ServiceResult<Flashcard[]> và hiển thị danh sách Flashcard
11. User click icon xóa trên một flashcard
12. FlashcardEditTab.js yêu cầu xác nhận xóa
13. User click "Xác nhận" để xác nhận xóa
14. FlashcardEditTab.js gọi `DocAgentService.deleteFlashcard()`
15. DocAgentService gửi DELETE request tới `/flashcard/flashcards/{flashcardId}`
16. Backend route nhận request và FlashcardService được gọi `delete_flashcard()`
17. FlashcardService xóa flashcard khỏi database
18. FlashcardService trả MessageResponse
19. flashcard_routes.py trả về MessageResponse JSON
20. DocAgentService nhận response và đóng gói với ServiceResult<MessageResponse>
21. FlashcardEditTab.js loại bỏ item khỏi danh sách Flashcard
22. Component re-render, danh sách Flashcard được cập nhật
23. Toast success notification được hiển thị: "Xóa flashcard thành công"

---

## Kịch bản 8: Luyện tập với Flashcard

1. Tại giao diện Session cụ thể, User click vào tab "Flashcard"
2. FlashcardTab.js gọi `DocAgentService.getFlashcardsBySession()` để lấy danh sách Flashcard
3. DocAgentService gửi request GET tới endpoint `/flashcard/flashcards/by-session/{sessionId}`
4. Backend route nhận request và FlashcardService được gọi hàm `get_flashcards_by_session()`
5. FlashcardService truy vấn cơ sở dữ liệu trả danh sách Flashcard objects
6. flashcard_routes.py trả về danh sách FlashcardResponse JSON
7. DocAgentService nhận danh sách từ response và đóng gói với ServiceResult<Flashcard[]>
8. FlashcardTab.js nhận ServiceResult<Flashcard[]> và hiển thị Flashcard đầu tiên
9. User xem câu hỏi trên Flashcard
10. User click vào Flashcard để lật sang xem đáp án
11. FlashcardTab.js cập nhật state `isFlipped = true`
12. Component re-render và hiển thị mặt đáp án
13. User click icon mũi tên trái/phải để chuyển sang Flashcard trước/sau
14. FlashcardTab.js cập nhật `currentIndex` tăng hoặc giảm 1
15. FlashcardTab.js reset `isFlipped = false`
16. Component re-render và hiển thị Flashcard mới
17. User tiếp tục luyện tập lặp đi lặp lại
