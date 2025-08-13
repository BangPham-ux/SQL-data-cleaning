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
---- 
#Vấn đề: Dữ liệu cột full_name không nhất quán về trường hợp chữ (hoa/thường), có khoảng trống dầu dòng

#Xóa khoảng trống đầu dòng
```
UPDATE club_member_info_cleaned
SET full_name = TRIM(full_name)
```
---- 
#Cập nhật dữ liệu bằng chữ hoa
```
UPDATE club_member_info_cleaned
SET full_name = UPPER(full_name);
```
---- 
#Vấn đề: Dữ liệu cột Age có giá trị bị thiếu và giá trị không hợp lệ

---- 
#Thay đổi tên cột
```
ALTER TABLE club_member_info_cleaned
RENAME COLUMN martial_status TO marital_status;
```
#Sửa lỗi chính tả 
```
UPDATE club_member_info_cleaned
SET marital_status = 'divorced'
WHERE marital_status = 'divored'
```
#Cột full_street tách thành 3 cột (street, city, state)
```
ALTER TABLE club_member_info_cleaned ADD COLUMN street TEXT;
ALTER TABLE club_member_info_cleaned ADD COLUMN city   TEXT;
ALTER TABLE club_member_info_cleaned ADD COLUMN state  TEXT;
```
#Cập nhật dữ liệu cho cột street
```
UPDATE club_member_info_cleaned
SET
  street = TRIM(substr(full_address, 1, instr(full_address, ',') - 1))
```
#Cập nhật dữ liệu cho cột city
```
UPDATE club_member_info_cleaned
SET city = SUBSTR(full_address, INSTR(full_address, ',') + 1,INSTR(SUBSTR(full_address, INSTR(full_address, ',') + 1), ',')-1)
```
#Cập nhật dữ liệu cho cột state
```
UPDATE club_member_info_cleaned
SET state = SUBSTR(full_address,INSTR(full_address, ',') + INSTR(SUBSTR(full_address, INSTR(full_address, ',') + 1), ',') + 1)
```

