# baitap2
Bài tập 02: Lập trình web.
NỘI DUNG BÀI TẬP:
2.1. Cài đặt Apache web server:
Vô hiệu hoá IIS: nếu iis đang chạy thì mở cmd quyền admin để chạy lệnh: iisreset /stop
Download apache server, giải nén ra ổ D, cấu hình các file:
D:\Apache24\conf\httpd.conf
D:Apache24\conf\extra\httpd-vhosts.conf để tạo website với domain: fullname.com code web sẽ đặt tại thư mục: D:\Apache24\fullname (fullname ko dấu, liền nhau)
sử dụng file c:\WINDOWS\SYSTEM32\Drivers\etc\hosts để fake ip 127.0.0.1 cho domain này ví dụ sv tên là: Đỗ Duy Cốp thì tạo website với domain là fullname ko dấu, liền nhau: doduycop.com
thao tác dòng lệnh trên file D:\Apache24\bin\httpd.exe với các tham số -k install và -k start để cài đặt và khởi động web server apache.
2.2. Cài đặt nodejs và nodered => Dùng làm backend:
Cài đặt nodejs:
download file https://nodejs.org/dist/v20.19.5/node-v20.19.5-x64.msi (đây ko phải bản mới nhất, nhưng ổn định)
cài đặt vào thư mục D:\nodejs
Cài đặt nodered:
chạy cmd, vào thư mục D:\nodejs, chạy lệnh npm install -g --unsafe-perm node-red --prefix "D:\nodejs\nodered"
download file: https://nssm.cc/release/nssm-2.24.zip giải nén được file nssm.exe copy nssm.exe vào thư mục D:\nodejs\nodered\
tạo file "D:\nodejs\nodered\run-nodered.cmd" với nội dung (5 dòng sau):
@echo off
REM fix path
set PATH=D:\nodejs;%PATH%
REM Run Node-RED
node "D:\nodejs\nodered\node_modules\node-red\red.js" -u "D:\nodejs\nodered\work" %*
mở cmd, chuyển đến thư mục: D:\nodejs\nodered
cài đặt service a1-nodered bằng lệnh: nssm.exe install a1-nodered "D:\nodejs\nodered\run-nodered.cmd"
chạy service a1-nodered bằng lệnh: nssm start a1-nodered
2.3. Tạo csdl tuỳ ý trên mssql (sql server 2022), nhớ các thông số kết nối: ip, port, username, password, db_name, table_name
2.4. Cài đặt thư viện trên nodered:
Truy cập giao diện nodered bằng url: http://localhost:1880
Cài đặt các thư viện: node-red-contrib-mssql-plus, node-red-node-mysql, node-red-contrib-telegrambot, node-red-contrib-moment, node-red-contrib-influxdb, node-red-contrib-duckdns, node-red-contrib-cron-plus
Sửa file D:\nodejs\nodered\work\settings.js : Tìm đến chỗ adminAuth, bỏ comment # ở đầu dòng (8 dòng), thay chuỗi mã hoá mật khẩu bằng chuỗi mới adminAuth: { type: "credentials", users: [{ username: "admin", password: "chuỗi_mã_hoá_mật_khẩu", permissions: "*" }] }, với mã hoá mật khẩu có thể thiết lập bằng tool: https://tms.tnut.edu.vn/pw.php
Chạy lại nodered bằng cách: mở cmd, vào thư mục D:\nodejs\nodered và chạy lệnh nssm restart a1-nodered khi đó nodered sẽ yêu cầu nhập mật khẩu mới vào được giao diện cho admin tại: http://localhost:1880
2.5. Tạo api back-end bằng nodered:
Tại flow1 trên nodered, sử dụng node http in và http response để tạo api
Thêm node MSSQL để truy vấn tới cơ sở dữ liệu
Logic flow sẽ gồm 4 node theo thứ tự sau (thứ tự nối dây):
http in : dùng GET cho đơn giản, URL đặt tuỳ ý, ví dụ: /timkiem
function : để tiền xử lý dữ liệu gửi đến
MSSQL: để truy vấn dữ liệu tới CSDL, nhận tham số từ node tiền xử lý
http response: để phản hồi dữ liệu về client: Status Code=200, Header add : Content-Type = application/json có thể thêm node debug để quan sát giá trị trung gian.
Test api thông qua trình duyệt, ví dụ: http://localhost:1880/timkiem?q=thị
2.6. Tạo giao diện front-end:
html form gồm các file : index.html, fullname.js, fullname.css cả 3 file này đặt trong thư mục: D:\Apache24\fullname nhớ thay fullname là tên của bạn, viết liền, ko dấu, chữ thường, vd tên là Đỗ Duy Cốp thì fullname là doduycop khi đó 3 file sẽ là: index.html, doduycop.js và doduycop.css
index.html và fullname.css: trang trí tuỳ ý, có dấu ấn cá nhân, có form nhập được thông tin.
fullname.js: lấy dữ liệu trên form, gửi đến api nodered đã làm ở bước 2.5, nhận về json, dùng json trả về để tạo giao diện phù hợp với kết quả truy vấn của bạn.
2.7. Nhận xét bài làm của mình:
đã hiểu quá trình cài đặt các phần mềm và các thư viện như nào?
đã hiểu cách sử dụng nodered để tạo api back-end như nào?
đã hiểu cách frond-end tương tác với back-end ra sao?
BÀI LÀM:
1. Cài đặt Apache web server:
Bước 1: Vô hiệu hoá IIS
Nhấn Start → gõ cmd -> Chuột phải vào Command Prompt → chọn Run as administrator -> Sau đó nhập lênh: iisreset /stop
<img width="997" height="405" alt="Screenshot 2025-10-26 144557" src="https://github.com/user-attachments/assets/d7338669-2f42-4ce0-9f6b-cc2e444d4a11" />
Bước 2. Download apache server
Truy cập link: https://www.apachelounge.com/download/ để download apache
Sau khi tải xong sẽ xuất hiện tệp:
<img width="614" height="32" alt="Screenshot 2025-10-26 150457" src="https://github.com/user-attachments/assets/6c554a59-2a28-4ee1-8a55-3b5b7894b7e1" />
Bước 3: Giải nén ra ổ D:
Chuột phải vào file vừa tải -> chọn Extract All... -> Chọn nơi muốn giải nén: D:\ -> Nhấn Extract
Sau khi giải nén xong, sẽ hiện thư mục: D:\Apache24\
<img width="1434" height="1051" alt="Screenshot 2025-10-26 153924" src="https://github.com/user-attachments/assets/6bc56317-35e2-466e-b3d4-36c1d28d088b" />
Bước 4: Cấu hình file: D:\Apache24\conf\httpd.conf
+ Mở file: D:\Apache24\conf\httpd.conf
+ Sửa ServerRoot: ServerRoot "c:/Apache24" => ServerRoot "D:/Apache24"
+ Sau mở httpd.conf -> Tìm dòng: #Include conf/extra/httpd-vhosts.conf và bỏ dấu # để bật file vhosts.
Bước 5: Cấu hình file: D:Apache24\conf\extra\httpd-vhosts.conf
+ Mở file: D:Apache24\conf\extra\httpd-vhosts.conf
+ Sau khi mở file sẽ hiển thị:
  <img width="1382" height="1068" alt="Screenshot 2025-10-26 155513" src="https://github.com/user-attachments/assets/82f5302f-e61f-4685-b9b2-bc95fb9d03cc" />

<img width="978" height="1074" alt="Screenshot 2025-10-26 162121" src="https://github.com/user-attachments/assets/2a18163e-111c-40fa-887e-30bb170fe10c" />
Thêm vào cuối file nội dung sau:
<VirtualHost *:80>
    ServerAdmin admin@nguyenthuthao.com
    DocumentRoot "D:/Apache24/nguyenthuthao"
    ServerName nguyenthuthao.com
    ServerAlias www.nguyenthuthao.com
    ErrorLog "logs/nguyenthuthao-error.log"
    CustomLog "logs/nguyenthuthao-access.log" common
</VirtualHost>
Trong file D:\Apache24\conf\httpd.conf sửa: DocumentRoot "D:/Apache24/nguyenthuthao" và <Directory "D:/Apache24/nguyenthuthao">
Bước 6: Tạo thư mục D:\Apache24\nguyenthuthao
Trong đó tạo file index.html:

<!DOCTYPE html>
<html>
  <head><title>Trang web của Thảo</title></head>
  <body><h1>Xin chào, đây là nguyenthuthao.com!</h1></body>
</html>

Bước 7: Cấu hình file hosts để fake domain
Mở file: C:\Windows\System32\drivers\etc\hosts (Mở bằng Notepad quyền Admin (Run as Administrator))
Sau khi mở, thêm dòng: 127.0.0.1 nguyenthuthao.com
<img width="1295" height="1066" alt="image" src="https://github.com/user-attachments/assets/e58e0a61-957b-47b7-b7ac-d7f3089e4e36" />
Bước 8: Cài đặt và khởi động Apache
Mở cmd -> chạy quyền admin (Run as Administrator), rồi chạy:
cd /d D:\Apache24\bin
httpd.exe -k install
httpd.exe -k start
<img width="975" height="503" alt="Screenshot 2025-10-26 175907" src="https://github.com/user-attachments/assets/ae206a1e-b7b8-490c-bfd1-640665c780e0" />
Bước 9: Kiểm tra kết quả
Mở trình duyệt, gõ: http://nguyenthuthao.com:8080
<img width="1906" height="1072" alt="Screenshot 2025-10-26 183207" src="https://github.com/user-attachments/assets/24c3f90b-bcd9-46ef-9915-96713c7d1c4f" />
2. Cài đặt nodejs và nodered => Dùng làm backend
2.1. Cài đặt nodejs
Tải file: https://nodejs.org/dist/v20.19.5/node-v20.19.5-x64.msi
Cài đặt bằng giao diện (GUI):
Nhấn file: node-v20.19.5-x64.msi
Chọn Next → I Agree → Custom.
Ở phần chọn đường dẫn, đổi thành: D:\nodejs
Bấm Next → Install.
Khi hoàn tất, Node.js sẽ được cài vào D:\nodejs và npm đi kèm.
Kiểm tra: Mở cmd(admin) và chạy:
cd \nodejs
node -v
npm -v
<img width="980" height="506" alt="Screenshot 2025-10-26 190425" src="https://github.com/user-attachments/assets/0b4bca41-7f48-4b11-a586-032b639756d5" />
2.2. Cài đặt nodered
Chạy cmd (Admin), vào thư mục D:\nodejs, chạy lệnh npm install -g --unsafe-perm node-red --prefix "D:\nodejs\nodered"
Sau khi chạy cmd, kết quả nodered hiển thị trong thư mục D:\nodejs
<img width="1442" height="1071" alt="Screenshot 2025-10-26 191413" src="https://github.com/user-attachments/assets/fba78db2-e235-4bd5-881a-b21f9eb6782f" />
Cài nssm: https://nssm.cc/release/nssm-2.24.zip.
Tạo file "D:\nodejs\nodered\run-nodered.cmd"
<img width="1227" height="1079" alt="Screenshot 2025-10-26 191551" src="https://github.com/user-attachments/assets/8ac6bdea-e253-4be6-a717-05164f342681" />
Cài service a1-nodered bằng nssm

Mở cmd (Admin), chuyển đến thư mục nodered: cd /d D:\nodejs\nodered
Cài đặt service "a1-nodered" bằng lệnh: nssm.exe install a1-nodered "D:\nodejs\nodered\run-nodered.cmd"
<img width="970" height="498" alt="Screenshot 2025-10-26 202822" src="https://github.com/user-attachments/assets/0271740e-9b94-4fc7-b09e-a55f45dccec2" />
chạy service "a1-nodered" bằng lệnh: nssm start a1-nodered
<img width="966" height="505" alt="Screenshot 2025-10-26 202937" src="https://github.com/user-attachments/assets/0ff232bd-d504-44be-b0cb-5c8aff599eca" />
3. Tạo csdl tuỳ ý trên mssql (sql server 2022), nhớ các thông số kết nối: ip, port, username, password, db_name, table_name
Tạo username, password:
-- 1. Tạo database
IF DB_ID('QLSV') IS NULL
    CREATE DATABASE QLSV;
GO

USE QLSV;
GO

-- 2. Tạo bảng SinhVien
IF OBJECT_ID('dbo.SinhVien','U') IS NULL
BEGIN
CREATE TABLE dbo.SinhVien (
    MaSV INT IDENTITY(1,1) PRIMARY KEY,
    HoTen NVARCHAR(200) NOT NULL,
    Lop NVARCHAR(50) NULL,
    QueQuan NVARCHAR(200) NULL
);
END
GO

-- 3. Thêm dữ liệu mẫu (chạy mỗi khi cần)
INSERT INTO dbo.SinhVien (HoTen, Lop, QueQuan)
VALUES
(N'Đỗ Duy Cốp', N'CNTT1', N'Thái Nguyên'),
(N'Nguyễn Thị Thảo', N'CNTT2', N'Hà Nội'),
(N'Trần Văn B', N'CNTT1', N'Bắc Ninh');
GO

IP = 127.0.0.1
PORT = 1433
USERNAME = user_api
PASSWORD = Pass@12345
DB = QLSV
TABLE = SinhVien

4. Cài đặt thư viện trên nodered:
Truy cập giao diện nodered bằng url: http://nguyenthuthao.com:1880
<img width="1912" height="1067" alt="Screenshot 2025-10-26 211006" src="https://github.com/user-attachments/assets/25a5c07f-eeb2-40f8-95f0-9da873064848" />
Trong giao diện Nodered: Vào Menu (≡ góc phải trên) -> chọn Manage palette -> Chọn tab Install -> Gõ lần lượt các thư viện và nhấn Install cho từng cái

Cài đặt thư viện:

node-red-contrib-mssql-plus
<img width="718" height="306" alt="Screenshot 2025-10-26 211527" src="https://github.com/user-attachments/assets/93a6e036-2c81-4d21-b39b-95e07d9c10de" />
node-red-node-mysql
<img width="696" height="313" alt="Screenshot 2025-10-26 211721" src="https://github.com/user-attachments/assets/7dbe9b52-b830-4841-8e1e-ae12115b29a5" />
node-red-contrib-telegrambot
<img width="716" height="317" alt="Screenshot 2025-10-26 211904" src="https://github.com/user-attachments/assets/243fdcf4-849d-4905-882b-bf0f5a708d97" />
node-red-contrib-moment
<img width="714" height="339" alt="Screenshot 2025-10-26 211958" src="https://github.com/user-attachments/assets/01dc4e8c-a05a-4fe0-b0c3-94abde9a68cd" />
node-red-contrib-influxdb
<img width="716" height="302" alt="Screenshot 2025-10-26 212049" src="https://github.com/user-attachments/assets/2f10c280-c2e7-45c4-81b6-879764fa2b7e" />
node-red-contrib-duckdns
![Uploading Screenshot 2025-10-26 212136.png…]()
node-red-contrib-cron-plus
![Uploading Screenshot 2025-10-26 212156.png…]()
Sửa file "D:\nodejs\nodered\work\settings.js":
adminAuth: {
       type: "credentials",
        users: [{
            username: "admin",
            password: "$2a$08$zZWtXTja0fB1pzD4sHCMyOCMYz2Z6dNbM6tl8sJogENOMcxWV9DN.",
            permissions: "*"
        }]
    },

Mã hoá mật khẩu có thể thiết lập bằng tool: https://tms.tnut.edu.vn/pw.php

Chạy lại nodered bằng cách: mở cmd (quyền Admin), vào thư mục 'D:\nodejs\nodered' và chạy lệnh 'nssm restart a1-nodered'
<img width="987" height="517" alt="Screenshot 2025-10-26 213217" src="https://github.com/user-attachments/assets/aaa66115-957c-4be2-8fc8-4e2cd0daf259" />
Nodered sẽ yêu cầu nhập mật khẩu mới vào được giao diện cho admin tại: http://nguyenthuthao.com:1880

7. Nhận xét bài làm:
7.1. Hiểu quá trình cài đặt phần mềm và thư viện
Biết cách cài Apache trên Windows từ bản ZIP (giải nén) và chỉnh httpd.conf/httpd-vhosts.conf.
Hiểu ServerRoot / DocumentRoot và cách ánh xạ domain nội bộ qua file hosts.
Biết cách cài Node.js và Node-RED, cách cài Node-RED global vào thư mục tùy chỉnh.
Biết dùng nssm để chạy Node-RED như một Windows Service (start/stop/restart).
Biết cài thêm package vào Node-RED qua Manage Palette.
