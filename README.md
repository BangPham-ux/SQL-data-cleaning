#Tạo bảng mới
```
CREATE TABLE club_member_info_cleaned (
  full_name VARCHAR(50),
  age INTEGER,
  marital_status VARCHAR(50),
  email VARCHAR(50),
  phone VARCHAR(50),
  full_address VARCHAR(50),
  job_title VARCHAR(50),
  membership_date VARCHAR(50)
);
```
---- 
#Thêm dữ liệu vào bảng mới
```
INSERT INTO club_member_info_cleaned
SELECT * FROM club_member_info;
```
