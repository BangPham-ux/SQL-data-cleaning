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
#Kết quả
```
| marital_status |
|----------------|
| [NULL]         |
| divorced       |
| married        |
| single         |
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
  street = TRIM(SUBSTR(full_address, 1, INSTR(full_address, ',') - 1))
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
#Cập nhật giá trị NULL cho cột job_title nếu giá trị rỗng
```
UPDATE club_member_info_cleaned
SET job_title = NULL
WHERE job_title = ''
```
#Kết quả cuối cùng
```
| full_name             | age | marital_status | email                    | phone        | full_address                                 | job_title                    | membership_date | state          | street                | city          |
|-----------------------|-----|----------------|--------------------------|--------------|----------------------------------------------|------------------------------|-----------------|----------------|-----------------------|---------------|
| ADDIE LUSH            | 40  | married        | alush0@shutterfly.com    | 254-389-8708 | 3226 Eastlawn Pass,Temple,Texas              | Assistant Professor          | 7/31/2013       | Texas          | 3226 Eastlawn Pass    | Temple        |
| ROCK CRADICK          | 46  | married        | rcradick1@newsvine.com   | 910-566-2007 | 4 Harbort Avenue,Fayetteville,North Carolina | Programmer III               | 5/27/2018       | North Carolina | 4 Harbort Avenue      | Fayetteville  |
| SYDEL SHARVELL        | 46  | divorced       | ssharvell2@amazon.co.jp  | 702-187-8715 | 4 School Place,Las Vegas,Nevada              | Budget/Accounting Analyst I  | 10/6/2017       | Nevada         | 4 School Place        | Las Vegas     |
| CONSTANTIN DE LA CRUZ | 35  | [NULL]         | co3@bloglines.com        | 402-688-7162 | 6 Monument Crossing,Omaha,Nebraska           | Desktop Support Technician   | 10/20/2015      | Nebraska       | 6 Monument Crossing   | Omaha         |
| GAYLOR REDHOLE        | 38  | married        | gredhole4@japanpost.jp   | 917-394-6001 | 88 Cherokee Pass,New York City,New York      | Legal Assistant              | 5/29/2019       | New York       | 88 Cherokee Pass      | New York City |
| WANDA DEL MAR         | 44  | single         | wkunzel5@slideshare.net  | 937-467-6942 | 10864 Buhler Plaza,Hamilton,Ohio             | Human Resources Assistant IV | 3/24/2015       | Ohio           | 10864 Buhler Plaza    | Hamilton      |
| JOANN KENEALY         | 41  | married        | jkenealy6@bloomberg.com  | 513-726-9885 | 733 Hagan Parkway,Cincinnati,Ohio            | Accountant IV                | 4/17/2013       | Ohio           | 733 Hagan Parkway     | Cincinnati    |
| JOETE CUDIFF          | 51  | divorced       | jcudiff7@ycombinator.com | 616-617-0965 | 975 Dwight Plaza,Grand Rapids,Michigan       | Research Nurse               | 11/16/2014      | Michigan       | 975 Dwight Plaza      | Grand Rapids  |
| MENDIE ALEXANDRESCU   | 46  | single         | malexandrescu8@state.gov | 504-918-4753 | 34 Delladonna Terrace,New Orleans,Louisiana  | Systems Administrator III    | 3/12/1921       | Louisiana      | 34 Delladonna Terrace | New Orleans   |
| FEY KLOSS             | 52  | married        | fkloss9@godaddy.com      | 808-177-0318 | 8976 Jackson Park,Honolulu,Hawaii            | Chemical Engineer            | 11/5/2014       | Hawaii         | 8976 Jackson Park     | Honolulu      |
```
