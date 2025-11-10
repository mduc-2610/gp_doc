Kịch bản: 
1. Tại giao diện chính của người quản lí, sau khi đăng nhập, người quản lí 
click chọn chức năng “Thống kê nhân viên” 
2. App.js sẽ gọi tới EmployeeStatistics.js 
3. Trang EmployeeStatistics.js hiển thị giao diện cho người quản lí 
4. Người quản lí chọn ngày bắt đầu và ngày kết thúc rồi nhấn nút “Tải thống 
kê” 
5. Trang EmployeeStatistics.js gọi tới hàm loadEmployeeStatistics() 
6. Hàm loadEmployeeStatistics() gọi tới lớp ApiService 
7. Lớp ApiService gọi tới hàm getEmployeeStatistics() 
8. Hàm getEmployeeStatistics() gửi yêu cầu với phương thức GET đến 
endpoint “/api/employee/statistics” tới EmployeeController phía backend 
thông qua RESTFul API. 
9. Lớp EmployeeController gọi hàm getEmployeeStatistics() 
10. Hàm getEmployeeStatistics() gọi đến lớp EmployeeRepository để truy 
vấn trong cơ sở dữ liệu 
11.  Lớp EmployeeRepository sẽ gọi đến hàm 
getEmployeeStatisticsByDateRange() 
12. Hàm getEmployeeStatisticsByDateRange() gọi tới lớp EmployeeStatistics 
để đóng gói thông tin thực thể 
13.  Lớ p EmployeeStatistics đo ng go i tho ng tin thự c the  
14.  Lớp EmployeeStatistics trả kết quả về cho hàm 
getEmployeeStatisticsByDateRange() 
15.  Hàm getEmployeeStatisticsByDateRange() trả kết quả về cho hàm 
getEmployeeStatistics() 
16.  Hàm getEmployeeStatistics() trả về dữ liệu cho hàm 
getEmployeeStatistics() phía Frontend 
17.  Hàm getEmployeeStatistics() trả kết quả hàm loadEmployeeStatistics() 
18.  Hàm loadEmployeeStatistics() trả kết quả cho trang EmployeeStatistics.js 
25 
19.  Trang EmployeeStatistics.js hiển thị giao diện cho người quản lý 
20.  Người quản lí chọn một nhân viên bất kỳ 
21.  Trang EmployeeStatistics.js gọi tới hàm handleEmployeeSelect() 
22.  Hàm handleEmployeeSelect() gọi tới lớp ApiService 
23.  Lớp ApiService gọi tới hàm getEmployeeAccessLogs() 
24.  Hàm getEmployeeAccessLogs() gửi yêu cầu với phương thức GET đến 
endpoint “/api/access-log/by-employee/{employeeId}” tới lớp 
AccessLogController phía backend thông qua RESTFul API. 
25.  Lớp AccessLogController gọi hàm getAccessLogsByEmployee() 
26.  Hàm getAccessLogsByEmployee() gọi đến lớp AccessLogRepository để 
truy vấn trong cơ sở dữ liệu 
27.  Lớp AccessLogRepository sẽ gọi đến hàm 
findByEmployeeIdAndTimestampBetween()  
28.  Hàm findByEmployeeIdAndTimestampBetween() gọi tới lớp AccessLog 
để đóng gói thông tin thực thể 
29.  Lớ p AccessLog đo ng go i tho ng tin thự c the  
30.  Lớp AccessLog trả kết quả về cho hàm 
findByEmployeeIdAndTimestampBetween() 
31.  Hàm findByEmployeeIdAndTimestampBetween() trả kết quả về cho hàm 
getAccessLogsByEmployee() 
32.  Hàm getAccessLogsByEmployee() trả về dữ liệu cho hàm 
getEmployeeAccessLogs() phía Frontend 
33.  Hàm getEmployeeAccessLogs() trả kết quả cho hàm 
handleEmployeeSelect() 
34.  Hàm handleEmployeeSelect() trả kết quả cho trang EmployeeStatistics.js 
35.  Trang EmployeeStatistics.js hiển thị giao diện cho người quản lý 
36.  Người quản lí chọn lọc theo khu vực và vào ra. 
37. Trang EmployeeStatistics.js gọi tới hàm handleFilterLogs() 
38.  Hàm handleFilterLogs() gọi tới lớp ApiService 
39.  Lớp ApiService gọi tới hàm getEmployeeAccessLogs() 
40.  Hàm getEmployeeAccessLogs() gửi yêu cầu với phương thức GET đến 
endpoint “/api/access-log/by-employee/{employeeId}” tới lớp 
26 
AccessLogController phía backend thông qua RESTFul API. 
41.  Lớp AccessLogController gọi hàm getAccessLogsByEmployee() 
42.  Hàm getAccessLogsByEmployee() gọi đến lớp AccessLogRepository để 
truy vấn trong cơ sở dữ liệu 
43.  Lớp AccessLogRepository sẽ gọi đến hàm 
findByEmployeeIdAndTimestampBetween()  
44.  Hàm findByEmployeeIdAndTimestampBetween() gọi tới lớp AccessLog 
để đóng gói thông tin thực thể 
45.  Lớ p AccessLog đo ng go i tho ng tin thự c the  
46.  Lớp AccessLog trả kết quả về cho hàm 
findByEmployeeIdAndTimestampBetween() 
47.  Hàm findByEmployeeIdAndTimestampBetween() trả kết quả về cho hàm 
getAccessLogsByEmployee() 
48.  Hàm getAccessLogsByEmployee() trả về dữ liệu cho hàm 
getEmployeeAccessLogs() phía Frontend 
49.  Hàm getEmployeeAccessLogs() trả kết quả cho hàm handleFilterLogs() 
50.  Hàm handleFilterLogs trả kết quả cho trang EmployeeStatistics.js 
51.  Trang EmployeeStatistics.js hiển thị giao diện cho người quản lý