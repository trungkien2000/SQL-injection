# SQL-injection

Thực hiện: Trung Kiên

Ngày thực hiện: 4/4/2022

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
