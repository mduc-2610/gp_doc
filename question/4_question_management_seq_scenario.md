# Sequence Scenario - Quản lý Question và Flashcard

## Kịch bản 1: Tạo Flashcard mới

1. User click "Thêm thẻ"
2. FlashcardEditTab.tsx gọi tới hàm addNewFlashcard()
3. Hàm addNewFlashcard() gọi tới FlashcardEditItem.tsx
4. FlashcardEditItem.tsx hiển thị dialog cho User
5. User nhập thông tin nội dung, đáp án, loại, giải thích và click "Lưu"
6. FlashcardEditItem.tsx gọi tới hàm validate()
7. Hàm validate() kiểm tra dữ liệu hợp lệ
8. FlashcardEditItem.tsx gọi tới hàm onSave()
9. Hàm onSave() gọi tới FlashcardEditTab.tsx
10. FlashcardEditTab.tsx gọi tới hàm handleCreateFlashcard()
11. Hàm handleCreateFlashcard() gọi tới lớp DocAgentService
12. Lớp DocAgentService gọi tới hàm createFlashcard()
13. Hàm createFlashcard() gửi yêu cầu với phương thức POST đến endpoint "/question/flashcard" tới question_routes.py phía backend thông qua RESTFul API
14. Lớp question_routes.py gọi hàm create_flashcard()
15. Hàm create_flashcard() gọi đến lớp QuestionService để xử lý logic nghiệp vụ
16. Lớp QuestionService sẽ gọi đến hàm create_flashcard()
17. Hàm create_flashcard() gọi tới lớp Flashcard để tạo thực thể mới
18. Lớp Flashcard đóng gói thông tin thực thể
19. Lớp Flashcard trả kết quả về cho hàm create_flashcard()
20. Hàm create_flashcard() trả kết quả về cho lớp question_routes.py
21. Lớp question_routes.py trả về dữ liệu cho hàm createFlashcard() phía Frontend
22. Hàm createFlashcard() trả kết quả cho FlashcardEditTab.tsx
23. FlashcardEditTab.tsx hiển thị thông báo thành công cho User

---

## Kịch bản 2: Xóa Flashcard

1. User click icon xóa Flashcard item
2. FlashcardEditTab.tsx gọi tới hàm handleDeleteFlashcard()
3. Hàm handleDeleteFlashcard() gọi tới lớp DocAgentService
4. Lớp DocAgentService gọi tới hàm deleteFlashcard()
5. Hàm deleteFlashcard() gửi yêu cầu với phương thức DELETE đến endpoint "/question/flashcard/{flashcard_id}" tới question_routes.py phía backend thông qua RESTFul API
6. Lớp question_routes.py gọi hàm delete_flashcard()
7. Hàm delete_flashcard() gọi đến lớp QuestionService để xử lý logic nghiệp vụ
8. Lớp QuestionService sẽ gọi đến hàm delete_flashcard()
9. Hàm delete_flashcard() gọi tới lớp MessageResponse để tạo thông báo phản hồi
10. Lớp MessageResponse đóng gói thông tin phản hồi
11. Lớp MessageResponse trả kết quả về cho hàm delete_flashcard()
12. Hàm delete_flashcard() trả kết quả về cho lớp question_routes.py
13. Lớp question_routes.py trả về dữ liệu cho hàm deleteFlashcard() phía Frontend
14. Hàm deleteFlashcard() trả kết quả cho FlashcardEditTab.tsx
15. FlashcardEditTab.tsx hiển thị thông báo xóa thành công cho User

---

## Kịch bản 3: Xem danh sách Flashcard (Edit Tab)

1. User click tab "Chỉnh sửa"
2. EditTab.tsx hiển thị giao diện cho User
3. User click tab con "Thẻ ghi nhớ"
4. EditTab.tsx gọi tới FlashcardEditTab.tsx
5. FlashcardEditTab.tsx gọi tới lớp DocAgentService
6. Lớp DocAgentService gọi tới hàm getFlashcardsBySession()
7. Hàm getFlashcardsBySession() gửi yêu cầu với phương thức GET đến endpoint "/question/flashcard/by-session/{session_id}" tới question_routes.py phía backend thông qua RESTFul API
8. Lớp question_routes.py gọi hàm get_flashcards_by_session()
9. Hàm get_flashcards_by_session() gọi đến lớp QuestionService để truy vấn trong cơ sở dữ liệu
10. Lớp QuestionService sẽ gọi đến hàm get_flashcards_by_session()
11. Hàm get_flashcards_by_session() gọi tới lớp Flashcard để đóng gói thông tin thực thể
12. Lớp Flashcard đóng gói thông tin thực thể
13. Lớp Flashcard trả kết quả về cho hàm get_flashcards_by_session()
14. Hàm get_flashcards_by_session() trả kết quả về cho lớp question_routes.py
15. Lớp question_routes.py trả về dữ liệu cho hàm getFlashcardsBySession() phía Frontend
16. Hàm getFlashcardsBySession() trả kết quả cho FlashcardEditTab.tsx
17. FlashcardEditTab.tsx hiển thị danh sách Flashcard cho User

---

## Kịch bản 4: Xem danh sách Flashcard (Practice Tab)

1. User click tab "Thẻ ghi nhớ"
2. DocAgentPage.tsx gọi tới FlashcardTab.tsx
3. FlashcardTab.tsx gọi tới lớp DocAgentService
4. Lớp DocAgentService gọi tới hàm getFlashcardsBySession()
5. Hàm getFlashcardsBySession() gửi yêu cầu với phương thức GET đến endpoint "/question/flashcard/by-session/{session_id}" tới question_routes.py phía backend thông qua RESTFul API
6. Lớp question_routes.py gọi hàm get_flashcards_by_session()
7. Hàm get_flashcards_by_session() gọi đến lớp QuestionService để truy vấn trong cơ sở dữ liệu
8. Lớp QuestionService sẽ gọi đến hàm get_flashcards_by_session()
9. Hàm get_flashcards_by_session() gọi tới lớp Flashcard để đóng gói thông tin thực thể
10. Lớp Flashcard đóng gói thông tin thực thể
11. Lớp Flashcard trả kết quả về cho hàm get_flashcards_by_session()
12. Hàm get_flashcards_by_session() trả kết quả về cho lớp question_routes.py
13. Lớp question_routes.py trả về dữ liệu cho hàm getFlashcardsBySession() phía Frontend
14. Hàm getFlashcardsBySession() trả kết quả cho FlashcardTab.tsx
15. FlashcardTab.tsx hiển thị Flashcard đầu tiên cho User
16. User nhấn key left, right, enter
17. FlashcardTab.tsx gọi tới hàm handleKeyPress()
18. Nếu key == "ArrowLeft" thì FlashcardTab.tsx gọi tới hàm goToPrevious()
19. Nếu key == "ArrowRight" thì FlashcardTab.tsx gọi tới hàm goToNext()
20. Nếu key == "Enter" thì FlashcardTab.tsx gọi tới hàm flipCard() và lật Flashcard để hiển thị đáp án
21. FlashcardTab.tsx hiển thị kết quả cho User

---

## Kịch bản 5: Chỉnh sửa Flashcard

1. User click vào 1 Flashcard item
2. FlashcardEditTab.tsx gọi tới hàm handleEdit()
3. Hàm handleEdit() gọi tới FlashcardEditItem.tsx
4. FlashcardEditItem.tsx hiển thị dialog với thông tin hiện tại cho User
5. User nhập thông tin cập nhật và click "Lưu"
6. FlashcardEditItem.tsx gọi tới hàm validate()
7. Hàm validate() kiểm tra dữ liệu hợp lệ
8. FlashcardEditItem.tsx gọi tới hàm onSave()
9. Hàm onSave() gọi tới FlashcardEditTab.tsx
10. FlashcardEditTab.tsx gọi tới hàm handleUpdateFlashcard()
11. Hàm handleUpdateFlashcard() gọi tới lớp DocAgentService
12. Lớp DocAgentService gọi tới hàm updateFlashcard()
13. Hàm updateFlashcard() gửi yêu cầu với phương thức PUT đến endpoint "/question/flashcard/{flashcard_id}" tới question_routes.py phía backend thông qua RESTFul API
14. Lớp question_routes.py gọi hàm update_flashcard()
15. Hàm update_flashcard() gọi đến lớp QuestionService để xử lý logic nghiệp vụ
16. Lớp QuestionService sẽ gọi đến hàm update_flashcard()
17. Hàm update_flashcard() gọi tới lớp Flashcard để cập nhật thực thể
18. Lớp Flashcard đóng gói thông tin thực thể đã cập nhật
19. Lớp Flashcard trả kết quả về cho hàm update_flashcard()
20. Hàm update_flashcard() trả kết quả về cho lớp question_routes.py
21. Lớp question_routes.py trả về dữ liệu cho hàm updateFlashcard() phía Frontend
22. Hàm updateFlashcard() trả kết quả cho FlashcardEditTab.tsx
23. FlashcardEditTab.tsx hiển thị thông báo cập nhật thành công cho User

---

## Kịch bản 6: Tạo Question mới

1. User click "Thêm câu hỏi"
2. QuestionEditTab.tsx gọi tới hàm addNewQuestion()
3. Hàm addNewQuestion() gọi tới QuestionEditItem.tsx
4. QuestionEditItem.tsx hiển thị dialog cho User
5. User nhập thông tin nội dung, loại, tùy chọn trả lời, đáp án đúng, mức độ nhận thức, giải thích và click "Lưu"
6. QuestionEditItem.tsx gọi tới hàm validate()
7. Hàm validate() kiểm tra dữ liệu hợp lệ
8. QuestionEditItem.tsx gọi tới hàm onSave()
9. Hàm onSave() gọi tới QuestionEditTab.tsx
10. QuestionEditTab.tsx gọi tới hàm handleCreateQuestion()
11. Hàm handleCreateQuestion() gọi tới lớp DocAgentService
12. Lớp DocAgentService gọi tới hàm createQuestion()
13. Hàm createQuestion() gửi yêu cầu với phương thức POST đến endpoint "/question/question" tới question_routes.py phía backend thông qua RESTFul API
14. Lớp question_routes.py gọi hàm create_question()
15. Hàm create_question() gọi đến lớp QuestionService để xử lý logic nghiệp vụ
16. Lớp QuestionService sẽ gọi đến hàm create_question()
17. Hàm create_question() gọi tới lớp Question để tạo thực thể mới
18. Lớp Question đóng gói thông tin thực thể
19. Lớp Question trả kết quả về cho hàm create_question()
20. Hàm create_question() trả kết quả về cho lớp question_routes.py
21. Lớp question_routes.py trả về dữ liệu cho hàm createQuestion() phía Frontend
22. Hàm createQuestion() trả kết quả cho QuestionEditTab.tsx
23. QuestionEditTab.tsx hiển thị thông báo thành công cho User

---

## Kịch bản 7: Xóa Question

1. User click icon xóa Question item
2. QuestionEditTab.tsx gọi tới hàm handleDeleteQuestion()
3. Hàm handleDeleteQuestion() gọi tới lớp DocAgentService
4. Lớp DocAgentService gọi tới hàm deleteQuestion()
5. Hàm deleteQuestion() gửi yêu cầu với phương thức DELETE đến endpoint "/question/question/{question_id}" tới question_routes.py phía backend thông qua RESTFul API
6. Lớp question_routes.py gọi hàm delete_question()
7. Hàm delete_question() gọi đến lớp QuestionService để xử lý logic nghiệp vụ
8. Lớp QuestionService sẽ gọi đến hàm delete_question()
9. Hàm delete_question() gọi tới lớp MessageResponse để tạo thông báo phản hồi
10. Lớp MessageResponse đóng gói thông tin phản hồi
11. Lớp MessageResponse trả kết quả về cho hàm delete_question()
12. Hàm delete_question() trả kết quả về cho lớp question_routes.py
13. Lớp question_routes.py trả về dữ liệu cho hàm deleteQuestion() phía Frontend
14. Hàm deleteQuestion() trả kết quả cho QuestionEditTab.tsx
15. QuestionEditTab.tsx hiển thị thông báo xóa thành công cho User

---

## Kịch bản 8: Xem danh sách Question (Edit Tab)

1. User click tab "Chỉnh sửa"
2. EditTab.tsx hiển thị giao diện cho User
3. User click tab con "Câu hỏi"
4. EditTab.tsx gọi tới QuestionEditTab.tsx
5. QuestionEditTab.tsx gọi tới lớp DocAgentService
6. Lớp DocAgentService gọi tới hàm getQuestionsBySession()
7. Hàm getQuestionsBySession() gửi yêu cầu với phương thức GET đến endpoint "/question/question/by-session/{session_id}" tới question_routes.py phía backend thông qua RESTFul API
8. Lớp question_routes.py gọi hàm get_questions_by_session()
9. Hàm get_questions_by_session() gọi đến lớp QuestionService để truy vấn trong cơ sở dữ liệu
10. Lớp QuestionService sẽ gọi đến hàm get_questions_by_session()
11. Hàm get_questions_by_session() gọi tới lớp Question để đóng gói thông tin thực thể
12. Lớp Question đóng gói thông tin thực thể
13. Lớp Question trả kết quả về cho hàm get_questions_by_session()
14. Hàm get_questions_by_session() trả kết quả về cho lớp question_routes.py
15. Lớp question_routes.py trả về dữ liệu cho hàm getQuestionsBySession() phía Frontend
16. Hàm getQuestionsBySession() trả kết quả cho QuestionEditTab.tsx
17. QuestionEditTab.tsx hiển thị danh sách Question cho User

---

## Kịch bản 9: Luyện tập với Question (Practice Tab)

1. User click tab "Câu hỏi"
2. EditTab.tsx gọi tới QuestionEditTab.tsx
3. QuestionEditTab.tsx gọi tới lớp DocAgentService
4. Lớp DocAgentService gọi tới hàm getQuestionsBySession()
5. Hàm getQuestionsBySession() gửi yêu cầu với phương thức GET đến endpoint "/question/question/by-session/{session_id}" tới question_routes.py phía backend thông qua RESTFul API
6. Lớp question_routes.py gọi hàm get_questions_by_session()
7. Hàm get_questions_by_session() gọi đến lớp QuestionService để truy vấn trong cơ sở dữ liệu
8. Lớp QuestionService sẽ gọi đến hàm get_questions_by_session()
9. Hàm get_questions_by_session() gọi tới lớp Question để đóng gói thông tin thực thể
10. Lớp Question đóng gói thông tin thực thể
11. Lớp Question trả kết quả về cho hàm get_questions_by_session()
12. Hàm get_questions_by_session() trả kết quả về cho lớp question_routes.py
13. Lớp question_routes.py trả về dữ liệu cho hàm getQuestionsBySession() phía Frontend
14. Hàm getQuestionsBySession() trả kết quả cho QuestionEditTab.tsx
15. QuestionEditTab.tsx hiển thị Question đầu tiên cho User
16. User nhấn key left, right, enter
17. QuestionEditTab.tsx gọi tới hàm handleKeyPress()
18. Nếu key == "ArrowLeft" thì QuestionEditTab.tsx gọi tới hàm goToPrevious()
19. Nếu key == "ArrowRight" thì QuestionEditTab.tsx gọi tới hàm goToNext()
20. Nếu key == "Enter" và needResults == true thì QuestionEditTab.tsx gọi tới hàm redoPractice() sau đó gọi tới hàm goToNext()
21. QuestionEditTab.tsx hiển thị kết quả cho User
22. User click "Hoàn thành" hoặc nhấn key enter
23. QuestionEditTab.tsx gọi tới hàm calculateScore()
24. Hàm calculateScore() tính toán điểm số
25. QuestionEditTab.tsx hiển thị kết quả điểm số cho User

---

## Kịch bản 10: Chỉnh sửa Question

1. User click vào 1 Question item
2. EditTab.tsx gọi tới hàm handleEdit()
3. Hàm handleEdit() gọi tới QuestionEditTab.tsx
4. QuestionEditTab.tsx gọi tới QuestionEditItem.tsx
5. QuestionEditItem.tsx hiển thị dialog với thông tin hiện tại cho User
6. User nhập thông tin cập nhật và click "Lưu"
7. QuestionEditItem.tsx gọi tới hàm validate()
8. Hàm validate() kiểm tra dữ liệu hợp lệ
9. QuestionEditItem.tsx gọi tới hàm onSave()
10. Hàm onSave() gọi tới QuestionEditTab.tsx
11. QuestionEditTab.tsx gọi tới hàm handleUpdateQuestion()
12. Hàm handleUpdateQuestion() gọi tới lớp DocAgentService
13. Lớp DocAgentService gọi tới hàm updateQuestion()
14. Hàm updateQuestion() gửi yêu cầu với phương thức PUT đến endpoint "/question/question/{question_id}" tới question_routes.py phía backend thông qua RESTFul API
15. Lớp question_routes.py gọi hàm update_question()
16. Hàm update_question() gọi đến lớp QuestionService để xử lý logic nghiệp vụ
17. Lớp QuestionService sẽ gọi đến hàm update_question()
18. Hàm update_question() gọi tới lớp Question để cập nhật thực thể
19. Lớp Question đóng gói thông tin thực thể đã cập nhật
20. Lớp Question trả kết quả về cho hàm update_question()
21. Hàm update_question() trả kết quả về cho lớp question_routes.py
22. Lớp question_routes.py trả về dữ liệu cho hàm updateQuestion() phía Frontend
23. Hàm updateQuestion() trả kết quả cho EditTab.tsx
24. EditTab.tsx hiển thị thông báo cập nhật thành công cho User
