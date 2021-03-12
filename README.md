# ĐỀ TÀI TẤN CÔNG SQL INJECTION

## I. Các nội dung chính
### 1. Giới thiệu
### 2. Kỹ thuật tấn công
### 3. Mội số công cụ tấn công
### 4. Demo thực tế
### 5. Cách phát hiện và phòng chống

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
- Illegal / Logically incorrect query
- Tautology

##### 2.1.2 Tiêm nhiễm SQL dựa trên câu lệnh UNION
Kỹ thuật này sử dụng toán tử UNION trong SQL để gộp kết quả chung từ 2 hoặc nhiều câu lệnh truy vấn `SELECT`.

```sql
SELECT <column_name(s)> FROM <table_1>
UNION
SELECT <column_name(s)> FROM <table_2>;
```

### 3. Một số công cụ
Có 1 số công cụ dùng cho việc tấn công SQL Injection, có thể kể đến như:
- BSQL Hacker
- Marathon Tool
- SQL Power Injector
- Havij
