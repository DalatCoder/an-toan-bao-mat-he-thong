# ĐỀ TÀI TẤN CÔNG SQL INJECTION

## I. Các nội dung chính
### 1. Giới thiệu
### 2. Kỹ thuật tấn công
### 3. Mội số công cụ tấn công
### 4. Demo thực tế
### 5. Cách phát hiện
### 6. Biện pháp phòng chống
### 7. Tài liệu tham khảo

### 1. Giới thiệu
- SQL viết tắt của Structural Query Language, là ngôn ngữ truy vấn có cấu trúc, được chuẩn hóa và sử dụng để định nghĩa lược đồ cơ sở dữ liệu, thao tác và truy vấn dữ liệu trong cơ sở dữ liệu quan hệ.
- Các câu lệnh SQL có thể được sử dụng để tạo các bảng, chèn và xóa dữ liệu trong các bảng, tạo các khung nhìn và truy xuất dữ liệu bằng các câu lệnh truy vấn.
- SQL Injection là 1 kĩ thuật tấn công phổ biến và phức tạp. Đối tượng được nhắm đến là các ứng dụng web, chương trình máy tính và cả cơ sở dữ liệu. Kĩ thuật này yêu cầu người thực hiện phải am hiểu sâu sắc về luồng xử lý của 1 ứng dụng web và các thành phần cấu thành nó, bao gồm cơ sở dữ liệu và SQL. Về cơ bản, kĩ thuật tấn công này tiêm nhiễm 1 đoạn mã hoặc 1 đoạn kịch bản nguy hiểm nhằm khai thác lỗ hổng bảo mật xảy ra trong tầng cơ sở dữ liệu của một ứng dụng (chẳng hạn như các truy vấn). Bằng cách tiêm nhiễm vào lệnh SQL, kẻ tấn công có thể trích xuất hoặc thao tác dữ liệu của ứng dụng web.

Một số báo cáo thực tế:
- Báo cáo tấn công vào ứng dụng web của Imperva tháng 7 năm 2013 đã khảo sát một loạt các máy chủ ứng dụng Web chuyên dụng và theo dõi tám loại tấn công phổ biến khác nhau. Báo cáo cho thấy các cuộc tấn công SQLi xếp thứ nhất hoặc thứ hai theo các tiêu chí: tổng số các sự cố bị tấn công, số lượng yêu cầu tấn công với mỗi sự cố tấn công và số ngày trung bình trên mỗi tháng mà một ứng dụng trải qua ít nhất một sự cố tấn công. Imperva đã quan sát một trang web đặc biệt nhận được 94.057 yêu cầu tấn công SQL injection chỉ trong một ngày

- Báo cáo Dự án Bảo mật Ứng dụng Web Mở 2013 về 10 rủi ro bảo mật ứng dụng Web quan trọng nhất đã liệt kê các cuộc tấn công tiêm chích, đặc biệt là các cuộc tấn công SQLi, là rủi ro hàng đầu. Bảng xếp hạng này không thay đổi so với báo cáo năm 2010.

- Báo cáo bảo mật phần mềm Veracode 2016 cho thấy tỷ lệ phần trăm các ứng dụng bị ảnh hưởng bởi các cuộc tấn công SQLi là khoảng 35%.

- Báo cáo bảo mật toàn cầu của Trustwave 2016 liệt kê các cuộc tấn công SQLi là một trong hai kỹ thuật xâm nhập hàng đầu. Báo cáo lưu ý rằng SQLi có thể gây ra mối đe dọa đáng kể cho dữ liệu nhạy cảm như thông tin nhận dạng cá nhân (Personal Identifiable Information - PII) và dữ liệu thẻ tín dụng. Dạng tấn công này tương đối dễ dàng trong việc triển khai để khai thác thông tin nhưng khó ngăn chặn

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
Bởi vì các cuộc tấn công SQLi rất phổ biến, gây tổn hại và thường xuyên thay đổi cả về phương thức lẫn kiểu tấn công, một biện pháp đối phó là không đủ. Thay vào đó, cần có một bộ kỹ thuật tích hợp. Một số biện pháp phòng chống có thể kể đến như:
- Kỹ thuật mã hóa phòng vệ thủ công
- Sử dụng các tham số trong truy vấn
- SQL DOM

#### 6.1 Kỹ thuật mã hóa phòng vệ thủ công
Một lỗ hổng phổ biến được khai thác bởi các cuộc tấn công SQLi là không xác thực đầu vào. Giải pháp đơn giản để loại bỏ các lỗ hổng này là áp dụng các kỹ thuật mã hóa phòng thủ phù hợp. Một ví dụ là kiểm tra kiểu đầu vào có phải là chuỗi chỉ chứa các ký số hay không. Loại kỹ thuật này có thể tránh các cuộc tấn công dựa trên các lỗi ràng buộc trong hệ quản trị cơ sở dữ liệu. Một kỹ thuật khác là thực hiện so khớp mẫu để cố gắng phân biệt đầu vào bình thường với đầu vào bất thường.

#### 6.2 Sử dụng các tham số trong truy vấn
Cách tiếp cận này cố gắng ngăn SQLi bằng cách cho phép nhà phát triển ứng dụng xác định chính xác hơn cấu trúc của truy vấn SQL và truyền các tham số giá trị cho nó một cách riêng biệt sao cho mọi đầu vào của người dùng không được phép sửa đổi cấu trúc truy vấn.

#### 6.3 SQL DOM
SQL DOM là một tập hợp các lớp cho phép tự động xác thực kiểu dữ liệu và thoát. Cách tiếp cận này đóng gói các truy vấn cơ sở dữ liệu nhằm cung cấp một cách an toàn và đáng tin cậy để truy cập cơ sở dữ liệu. Điều này thay đổi quy trình xây dựng truy vấn thay vì quy trình không được kiểm soát như trước đây bằng cách sử dụng một hệ thống API để kiểm tra kiểu. Trong API, các nhà phát triển có thể áp dụng một cách có hệ thống các kỹ thuật tốt nhất để lọc đầu vào và kiểm tra nghiêm ngặt kiểu dữ liệu của đầu vào của người dùng.

### 7. Tài liệu tham khảo
- Sách CEH v10 Complete Training Guide With Labs
- Computer Security Principles and Practice (Fourth edition)
