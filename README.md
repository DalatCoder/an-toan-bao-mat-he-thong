# ĐỀ TÀI TẤN CÔNG SQL INJECTION

## I. Các nội dung chính
### 1. Giới thiệu
### 2. Kỹ thuật tấn công
### 3. Mội số công cụ tấn công
### 4. Demo thực tế
### 5. Cách phát hiện
### 6. Biện pháp phòng chống

### 1. Giới thiệu
- SQL viết tắt của Structural Query Language, là ngôn ngữ truy vấn có cấu trúc, được chuẩn hóa và sử dụng để định nghĩa lược đồ cơ sở dữ liệu, thao tác và truy vấn dữ liệu trong cơ sở dữ liệu quan hệ.
- Các câu lệnh SQL có thể được sử dụng để tạo các bảng, chèn và xóa dữ liệu trong các bảng, tạo các khung nhìn và truy xuất dữ liệu bằng các câu lệnh truy vấn.
- SQL Injection là 1 kĩ thuật tấn công phổ biến và phức tạp. Đối tượng được nhắm đến là các ứng dụng web, chương trình máy tính và cả cơ sở dữ liệu. Kĩ thuật này yêu cầu người thực hiện phải am hiểu sâu sắc về luồng xử lý của 1 ứng dụng web và các thành phần cấu thành nó, bao gồm cơ sở dữ liệu và SQL. Về cơ bản, kĩ thuật tấn công này tiêm nhiễm 1 đoạn mã hoặc 1 đoạn kịch bản nguy hiểm nhằm khai thác lỗ hổng bảo mật xảy ra trong tầng cơ sở dữ liệu của một ứng dụng (chẳng hạn như các truy vấn). Bằng cách tiêm nhiễm vào lệnh SQL, kẻ tấn công có thể trích xuất hoặc thao tác dữ liệu của ứng dụng web.

### 2. Các kỹ thuật tấn công
1. In-band SQLi
2. Inferential SQLi
3. Out-of-band SQLi

#### 2.1 In-band SQLi
Tiêm nhiễm SQL trong băng là 1 nhóm bao gồm nhiều phương thức tấn công. Trong đó, các kĩ thuật tiêm nhiễm SQL sử dụng cùng kênh giao tiếp để phát động tấn công và thu thập thông tin qua các phản hồi. Kĩ thuật tiêm nhiễm trong băng bao gồm các phương thức tấn công:
- Tiêm nhiễm SQL dựa trên thông báo lỗi
- Tiêm nhiễm SQL dựa trên câu lệnh UNION

##### 2.1.1 Tiêm nhiễm SQL dựa trên thông báo lỗi
Kẻ tấn công dựa vào thông báo lỗi phản hồi từ máy chủ cơ sở dữ liệu, từ đó thu thập thông tin và định hình nên cấu trúc của cơ sử dữ liệu lưu trữ. Trong quá trình phát triển sản phẩm, những thông báo lỗi giúp ích rất nhiều cho các nhà phát triển trong việc tìm và xử lý lỗi. Tuy nhiên, khi sản phẩm đã được hoàn thiện và đưa vào sử dụng thực tế, những thông báo lỗi này lại tiềm ẩn nhiều nguy cơ an toàn bảo mật.
Kĩ thuật tiêm nhiễm SQL dựa trên thông báo lỗi lại bao gồm rất nhiều kĩ thuật con, có thể kể đến như:
- System Store Procedure
- End of line comment
- Tautology

##### 2.1.2 Tiêm nhiễm SQL dựa trên câu lệnh UNION
Kỹ thuật này sử dụng toán tử UNION trong SQL để gộp kết quả chung từ 2 hoặc nhiều câu lệnh truy vấn `SELECT`.

```sql
SELECT <column_name(s)> FROM <table_1>
UNION
SELECT <column_name(s)> FROM <table_2>;
```

#### 2.2 Kĩ thuật tấn công suy luận
Với kĩ thuật tấn công này, thực tế không có việc truyền dữ liệu từ ứng dụng web. Do đó, hacker không thể xem kết quả trực tiếp từ cuộc tấn công, chính vì thế mà nó được gọi với tên khác là "Kĩ thuật tiêm nhiễm mù". Tuy nhiên, kẻ tấn công vẫn có thể xây dựng lại thông tin bằng cách gửi các yêu cầu cụ thể và quan sát hành vi phản hồi của máy chủ trang web/cơ sở dữ liệu. Các kiểu tấn công suy diễn có thể kể đến như:
- Truy vấn bất hợp pháp/không đúng logic
- Tiêm SQL mù

##### 2.2.1 Truy vấn bất hợp pháp/không đúng logic
Cuộc tấn công này cho phép kẻ tấn công thu thập thông tin quan trọng về loại và cấu trúc của cơ sở dữ liệu đằng sau ứng dụng Web. Cuộc tấn công được coi là bước đầu tiên, thu thập thông tin cho các cuộc tấn công khác. Lỗ hổng được khai thác bởi cuộc tấn công này là trang lỗi mặc định được trả về bởi các máy chủ ứng dụng thường được mô tả quá chi tiết. Trong thực tế, một thông báo lỗi được tạo ra thường có thể tiết lộ các thông số dễ bị tổn thương, có thể bị lợi dụng bởi kẻ tấn công.

##### 2.2.2 Tiêm SQL mù
Tiêm SQL mù cho phép kẻ tấn công suy ra dữ liệu có trong hệ thống cơ sở dữ liệu ngay cả khi hệ thống đủ an toàn để không hiển thị bất kỳ thông tin sai sót nào cho kẻ tấn công. Kẻ tấn công hỏi máy chủ câu hỏi dạng đúng/sai. Nếu câu lệnh được đánh giá là đúng, trang web sẽ tiếp tục hoạt động bình thường. Nếu câu lệnh là sai, mặc dù không có thông báo lỗi mô tả, trang web sẽ khác đáng kể so với trang hoạt động bình thường.

#### 2.3 Kĩ thuật tấn công ngoài băng
Đây là kĩ thuật sử dụng các kênh khác nhau để tiến hành cuộc tấn công và thu thập các phản hồi. Để có thể tấn công, kĩ thuật này yêu cầu 1 số tính năng phải được kích hoạt sẵn, ví dụ như DNS hoặc yêu cầu HTTP đến máy chủ cơ sở dữ liệu. Chính vì phải thõa mãn các yêu cầu trước khi tấn công nên kĩ thuật này không được phổ biến.

### 3. Một số công cụ
Có 1 số công cụ dùng cho việc tấn công SQL Injection, có thể kể đến như:
- BSQL Hacker
- Marathon Tool
- SQL Power Injector
- Havij

### 4. Demo thực tế

### 5. Cách phát hiện

### 6. Biện pháp phòng chống
