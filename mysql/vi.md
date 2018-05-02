[Source](https://dev.mysql.com/doc/refman/5.7/en/option-files.html "Permalink to MySQL :: MySQL 5.7 Reference Manual :: 4.2.6 Using Option Files")

# MySQL :: MySQL 5.7 Reference Manual :: 4.2.6 Using Option Files

### 4.2.6 Using Option Files

Hầu hết các chương trình MySQL có thể đọc các tuỳ chọn khởi động từ các file tuỳ chọn ( ~~thỉnh thoảng~~ **thường** được gọi từ các file cấu hình). Các file tuỳ chọn cung cấp một cách thuận tiện để **sử dụng** các chỉ định thường được sử dụng vì vậy chúng không cần nhập trên dòng command mỗi khi chạy chương trình.

Để biết liệu một chương trình có đọc các file tuỳ chọn, gọi nó với tuỳ chọn `\--help` . (**Đối với** [**mysqld**][1], sử dụng [`\--verbose`][2] và [`\--help`][3].) Nếu chương trình đọc các file tuỳ chọn, thông báo trợ giúp cho biết file nào nó tìm kiếm và nhóm tuỳ chọn nào nó xác nhận. 

Chú ý: 

Một chương trình MySQL được bắt đầu với tuỳ chọn `\--no-defaults` đọc không có các file tuỳ chọn nào khác ngoài  `.mylogin.cnf`. 

Nhiều các file tuỳ chọn là các file văn bản thuần tuý, được tạo bằng bất cứ trình soạn thảo nào. Ngoại lệ là file `.mylogin.cnf` chứa đường dẫn tuỳ chọn đăng nhập. Nó là một file được mã hoá bởi [**mysql_config_editor**][4] utility. Xem [Phần 4.6.6, "**mysql_config_editor** — MySQL Configuration Utility"][4]. Một "đường dẫn đăng nhập" là một nhóm tuỳ chọn chỉ cho phép một số tuỳ chọn: `host`, `user`, `password`, `port` and `socket`. Các chương trình phía Client cái mà đường dẫn đăng nhập đọc từ `.mylogin.cnf` sử dụng tuỳ chọn [`\--login-path`][5] . 

Để xác định một đường dẫn đăng nhập thay thế, thiết lập biến môi trường  `MYSQL_TEST_LOGIN_FILE` . Biến môi trường này được sử dụng bởi tiện ích testing **mysql-test-run.pl** , nhưng cũng được xác nhận bởi [**mysql_config_editor**][4] và các client MySQL như là [**mysql**][6], [**mysqladmin**][7].

MySQL tìm kiếm các file tuỳ chọn theo thứ tự mô tả cuộc thảo luận sau và đọc bất cứ ~~nội dung~~tệp nào đã tồn tại. Nếu một file tuỳ chọn muốn sử dụng là không tồn tại, tạo ra nó và tạo nó bằng phương pháp thích hợp, như thảo luận vừa rồi

Trên Windows, các chương trình MySQL đọc các tuỳ chọn khởi động từ các file như bảng, trong các thứ tự được ưu tiên) các file được liệt kê là đọc đầu tiên, và đọc sau khi được ưu tiên). 

**Table 4.1 Đọc các file tuỳ chọn trên hệ thống Windows**

| Tên File                                                                                  | Mục đích                                                       |  
| ------------------------------------------------------------------------------------------ | ------------------------------------------------------------- |  
| ``%PROGRAMDATA%`MySQLMySQL Server 5.7my.ini`, ``%PROGRAMDATA%`MySQLMySQL Server 5.7my.cnf` | Global options                                                |  
| ``%WINDIR%`my.ini`, ``%WINDIR%`my.cnf`                                                     | Global options                                                |  
| `C:my.ini`, `C:my.cnf`                                                                     | Global options                                                |  
| `_`BASEDIR`_my.ini`, `_`BASEDIR`_my.cnf`                                                   | Global options                                                |  
| `defaults-extra-file`                                                                      | The file specified with [`\--defaults-extra-file`][8], if any |  
| ``%APPDATA%`MySQL.mylogin.cnf`                                                             | Login path options (clients only)                             |  

Trong bảng ~~trước~~ **trên**, `%PROGRAMDATA%` đại diện trực tiếp file hệ thống chứa dữ liệu ứng dụng cho tất cả các user trên host. Đường dẫn mặc định trỏ đến `C:ProgramData` trên Microsoft Windows Vista và phiên bản lớn hơn và `C:Documents and SettingsAll UsersApplication Data` trên phiên bản cũ của Microsoft Windows. 

`%WINDIR%` đại diện cho thư mục cục bộ của Windows. ~~This is commonly~~ **Nó thường là** `C:WINDOWS`. Sử dụng lệnh sau đây để xác định vị trí đưa ra từ giá trị của biến môi trường `WINDIR` : 
    
    
    C:> echo %WINDIR%

`%APPDATA%` đại diện cho giá trị của thư mục chứa dữ liệu ứng dụng Windows. Sử dụng câu lệnh sau đây để xác định vị trí đưa ra ra từ giá trị của biến môi trường `APPDATA` : 
    
    
    C:> echo %APPDATA%

_`BASEDIR`_ đại diện cho thư mục cài đặt cơ bản của MySQL. Khi MySQL 5.7 được cài đặt sử dụng MySQL Installer, nó thường là `C:_`PROGRAMDIR`_MySQLMySQL 5.7 Server` nơi mà _`PROGRAMDIR`_ đại diện cho thư mục của các chương trình (luôn luôn là `Program Files` phiên bản ngôn ngữ Tiếng Anh của Windows), Xem ở [Section 2.3.3, "MySQL Installer for Windows"][9]. 

Trên hệ thống Unix và tương tự Unix, chương trình MySQL đọc các tuỳ chọn khởi động từ các file được hiển thị theo bảng dưới đây, theo thứ tự được chỉ định ( các file được liệt kê đầu tiên thì được đọc đầu tiên, các file sau được đọc theo thứ tự ưu tiên). 

Chú ý: 

Trên nền tảng Unix, MySQL bỏ qua các file cấu hình có thể được ghi **toàn cục** **that are world-writable**. Đây là chủ ý như là một biện pháp an ninh. 

**Table 4.2 Các file tuỳ chọn đọc trên các hệ thống Unix và giống Unix**

|  Tên file               | Mục đích                                                       |  
| ----------------------- | ------------------------------------------------------------- |  
| `/etc/my.cnf`           | Global options                                                |  
| `/etc/mysql/my.cnf`     | Global options                                                |  
| `_`SYSCONFDIR`_/my.cnf` | Global options                                                |  
| `$MYSQL_HOME/my.cnf`    | Server-specific options (server only)                         |  
| `defaults-extra-file`   | The file specified with [`\--defaults-extra-file`][8], if any |  
| `~/.my.cnf`             | User-specific options                                         |  
| `~/.mylogin.cnf`        | User-specific login path options (clients only)               |  

Trong bảng trước, `~` đại diện cho thư mục home của user hiện tại (gía trị của  `$HOME`). 

_`SYSCONFDIR`_ đại diện cho thư mục được xác định với tuỳ chọn [`SYSCONFDIR`][10] tới **CMake** khi MySQL được xây dựng. Theo mặc định, nó là thư mục `etc` được đặt trong thư mục cài đặt khi biên dịch. 

`MYSQL_HOME` là một biến môi trường chứa đường dẫn đến thư mục đặt file `my.cnf` **chỉ định** của server. Nếu `MYSQL_HOME` không được thiết lập và bạn ~~bắt đầu~~ **khởi động** server sử dụng chương trình [**mysqld_safe**][11], [**mysqld_safe**][11] được thiết lập đến _`BASEDIR`_, thư mục cài đặt cơ sở của MySQL. 

_`DATADIR`_ thường là  `/usr/local/mysql/data`, mặc dù điều này có thể thay đổi theo từng nền tảng hoặc cách cài đặt. Giá trị là vị trí thư mục dữ liệu được built khi MySQL được biên dịch, không phải là vị trí được xác định với tuỳ chọn [`\--datadir`][12] khi [**mysqld**][1] bắt đầu. Sử dụng [`\--datadir`][12] lúc chạy không có hiệu quả trên server tìm kiến tuỳ chọn của các file nó đọc trước tiến trình thực thi bất cứ tuỳ chọn nào.

Nếu nhiều phiên bản của một tuỳ chọn được tìm thấy, phiên bản cuối cùng sẽ được ưu tiên, với một exception: ~~For~~ đối với **[mysqld**][1], phiên bản _đầu tiên_ của tuỳ chọn `[\--user`][13] được sử dụng như một biện pháp an ninh, để đại diện một user xác định trong một tuỳ chọn file khỏi bị ghi đề trên command. 

Mô tả sau đây của cú pháp file **tùy chọn** áp dụng đến các file mà bạn chỉnh sửa bằng tay. Không bao gồm `.mylogin.cnf`, được tạo bằng **[mysql_config_editor**][4] và được mã hoá. 

Bất kỳ tuỳ chọn dài nào có thể được đưa ra bởi dòng lệnh khi đang chạy một chương trình MySQL có thể đưa ra trong một file **tùy chọn**. Để lấy danh sách các tuỳ chọn sẵn sàng cho một chương trình, chạy với tuỳ chọn `\--help` . (Cho **[mysqld**][1], sử dụng `[\--verbose`][2] và `[\--help`][3].) 

Cú pháp cho các tuỳ chọn xác định trong một tuỳ chọn file là tương tự cú pháp dòng lệnh (xem [Section 4.2.4, "Using Options on the Command Line"][14]). Tuy nhiên, trong một file tuỳ chọn, bạn bỏ hai dấu gạch ngang đầu trong tên tuỳ chọn và bạn xác định chỉ một tuỳ chọn cho mỗi dòng. Ví dụ, `\--quick` và `\--host=localhost` trên dòng lệnh nên được xác định như là `quick` và `host=localhost` trên các dòng riêng rẽ trong file tuỳ chọn. Đểu xác định một mẫu tuỳ chọn `\--loose-_`opt_name`_` trong một file tuỳ chọn, ghi nó giống như `loose-_`opt_name`_`. 

Các dòng trống trong các file tuỳ chọn được bỏ qua. Các dòng không trống có thể theo bất kỳ mẫu nào sau đây:

* `#_`comment`_`, `;_`comment`_`

Các dòng comment bắt đầu với `#` hoặc `;`. Một comment `#` có thể bắt đầu trong giữa dòng .

* `[_`group`_]`

_`group`_ là tên của chương trình hoặc nhóm cho cái mà ban muốn thiết lập các tuỳ chọn. Sau một nhóm dòng mọi dòng tuỳ chọn cài đặt được áp dụng với tên nhóm trước khi kết thúc file tuỳ chọn hoặc nhóm các dòng khác được đưa vào. Tên nhóm tuỳ chọn không phân biệt hoa thường.

* `_`opt_name`_`

Cái này tương đương `\--_`opt_name`_` trên dòng lệnh. 

* `_`opt_name`_=_`value`_`

Cái này tương đương với `\--_`opt_name`_=_`value`_` trên dòng lệnh. Trong một file tuỳ chọn, bạn có thể có các khoảng trắng xung quanh ký tự `=` , thỉnh thoảng điều đấy không đúng trên dòng lệnh. Giá trị tuỳ chọn có thể được bao quanh bởi dấu nháy đơn hoặc nháy kép, rất hữu ích nếu gía trị chứ một ký tự comment `#`. 

Các khoảng trắng ở đầu và cuối được tự động xoá từ tên và giá trị tuỳ chọn. 

Bạn có thể sử dụng cá ký tự  `b`, `t`, `n`, `r`, `\`, và `s` trong giá trị của tuỳ chọn để đại diện các ký tự backspace, tab, dòng mới, carriage return, backslash, và khoảng trắng. Trong các file tuỳ chọn, những luật được áp dụng;

* Một dấu backslash được theo bởi ký tự escape sequence hợp lệ được chuyển thành ký tự đại diện cho sequence. Ví dụ, `s` được chuyển thành một khoảng trắng. 
* Một dấu backslash không được theo bởi một ký tự escape sequence hợp lệ vẫn không thay đổi. Ví dụ, `S` được giữ nguyên. 

Các luật trên có nghĩa là dấu gạch chéo thực sự được đưa viết thành  `\`, hoặc  `` nếu theo sau không phải là một ký tự kết thúc hợp lệ. 

Các quy tắc cho chuỗi kết thúc trong file tuỳ chọn có thể khác một chút so với các quy tắc về chuỗi kết thúc trong câu lệnh SQL. Trong ngữ cảnh sau, nếu "_`x`_" không là ký tự kết thúc chuỗi hợp lệ, `_`x`_` trở thành "_`x`_" chứ không phải là `_`x`_`. Xem [Section 9.1.1, "String Literals"][15]. 

Các quy tắc kết thúc cho giá trị file tuỳ chọn là đặc biệt thích hợp cho tên đường dẫn Windows, khi sử dụng `` làm ký tự phân cách cho tênd dường dẫn. Một ký tự phân cách trong một tên đường dẫn  Windows phải được ghi `\` nếu nó theo sau bởi một ký tự kết thúc. Nó được ghi thành `\` or `` nếu nó không phải. Tuỳ nhiên, `/` có thể được sử dụng trong tên đường dẫn Windows và sẽ được coi như là ``. Giả sử bạn muốn xác định một thư mục gốc của `C:Program FilesMySQLMySQL Server 5.7` trong file tuỳ chọn. Điều đó có thể hoàn thành theo nhiều các. Một vài ví dụ: 
    
    
    basedir="C:Program FilesMySQLMySQL Server 5.7"
    basedir="C:\Program Files\MySQL\MySQL Server 5.7"
    basedir="C:/Program Files/MySQL/MySQL Server 5.7"
    basedir=C:\ProgramsFiles\MySQL\MySQLsServers5.7

Nếu một tên nhóm tuỳ chọn là giống như một tên chương trình, các tuỳ chọn trong nhóm có thể áp dụng riêng cho chương trình đấy. Ví dụ,nhóm `[mysqld]` và `[mysql]` áp dụng cho chương trình **[mysqld**][1] server và **[mysql**][6] client, tương ứng. 

Nhóm tuỳ chọn `[client]` được đọc bởi tất cả các chương trình client được cung cấp trong bản phân phối MySQL (ngoại trừ **[mysqld**][1]). Để hiểu làm thế nào chương trình bên thứ ba sử dụng C API có thể sử dụng được các file tuỳ chọn, xem tài liệu C API tại [Section 27.8.7.50, "mysql_options()"][16]. 

Nhóm `[client]` cho phép bạn xác định tuỳ chọn được áp dụng cho tất cả client. Ví dụ, `[client]` là nhóm phù hợp để sử dụng xác định mật khẩu kết nối đến server. (Nhưng chắc chắn file tuỳ chọn là được truy cập bởi chính bạn, điều đó làm người khác không thể tìm ra mật khẩu của bạn) . Đảm bảo không chèn được tuỳ chọn vào nhóm `[client]` trừ khi nó được nhận biết bởi tất cả chương trình client mà bạn sử dụng. Các chương trình không thể hiểu tuỳ chọn kết thúc sau khi hiển thị lỗi nếu bạn chạy chúng.

Liệt kê các nhóm tuỳ chọn chung trước các nhóm cụ thể sau. Ví dụ, một nhóm `[client]` chung hơn bởi vì được đọc bởi tất cả chương trình client, trong khi đó nhóm `[mysqldump]` chỉ được đọc bởi **[mysqldump**][17]. Các tuỳ chọn cụ thể này được ghi đè lên tuỳ chọn trước đó, vì thể đặt các nhóm tuỳ chọn theo thứ tự `[client]`, `[mysqldump]` cho phép các tuỳ chọn cụ thể **[mysqldump**][17]-ghi đè các tuỳ chọn `[client]` . 

Đây là file tuỳ chọn toàn cục: 
    
    
    [client]
    port=3306
    socket=/tmp/mysql.sock
    
    [mysqld]
    port=3306
    socket=/tmp/mysql.sock
    key_buffer_size=16M
    max_allowed_packet=8M
    
    [mysqldump]
    quick

Đây là file tuỳ chọn của người dùng: 
    
    
    [client]
    # The following password will be sent to all standard MySQL clients
    password="my password"
    
    [mysql]
    no-auto-rehash
    connect_timeout=2

Để tạo nhóm cấu hình để chỉ đọc bởi server **[mysqld**][1] từ bản phát hành cụ thể nào đó MySQL, sử dụng nhóm với tên  `[mysqld-5.6]`, `[mysqld-5.7]`, .... Nhóm sau thể hiện cài đặt  `[sql_mode`][18] nên chỉ được sử dụng bởi MySQL servers với phiên bản 5.7.x : 
    
    
    [mysqld-5.7]
    sql_mode=TRADITIONAL

Nó có thể sử dụng `!include` trong file cấu hình để bao gồm các file cấu hình khác và  `!includedir` để tìm kiếm các thư mục cụ thể cho các file cấu hình. Ví dụg, để inclue file `/home/mydir/myopt.cnf` , sủ dụng:
    
    
    !include /home/mydir/myopt.cnf

Để tìm kiếm thư mục  `/home/mydir` và đọc các file cấu hình tìm thấy, sử dụng: 
    
    
    !includedir /home/mydir

MySQL không đảm bảo về thứ tự các file cấu hình trong thư mục được đọc.

Chú ý: 

Bất kỳ các file được tìm thấy và thêm vào sử dụng  `!includedir` trên hệ điều hành  Unix _phải_ có các tên kết thúc  `.cnf`. Trên Windows, chỉ thị này kiểm tra các file với đuôi `.ini` hoặc `.cnf` . 

Ghi nội dung của một file cấu hình được include giống như file cấu hình khác. Đó là, nó nên chưa các nhóm của các cấu hình, mỗi nhóm có dòng `[_`group`_]` đặt trước thể hiện chương trình nào áp dụng cấu hình.

Trong khi một file được include đang được xử lý, chỉ có những tuỳ cấu hình trong nhóm mà chương trình hiện tại đang tìm kiếm được sử dụng. Các nhóm khác được bỏ qua. Giả sử một file `my.cnf` chưa dòng này:
    
    
    !include /home/mydir/myopt.cnf

Và giả sử `/home/mydir/myopt.cnf` nhìn giống như: 
    
    
    [mysqladmin]
    force
    
    [mysqld]
    key_buffer_size=16M

Nếu  `my.cnf` được xử lý bởi **[mysqld**][1], chỉ nhóm  `[mysqld]` trong `/home/mydir/myopt.cnf` được sử dụng . Nếu file được xử lý bởi **[mysqladmin**][7], chỉ nhóm `[mysqladmin]` được sử dụng.. Nếu file được xử lý bởi chương trình khác, không có cấu hình trong `/home/mydir/myopt.cnf` được sử dụng.

`!includedir` được xử lý tương tự ngoại trừ tất cả các file trong thư mục được đọc.

Nếu một file cấu hình chứa `!include` hoặc `!includedir` , tên các file được đặt bởi chỉ thị được xử lý bất cứ khi nào file cấu hình được xử lý, không kể chúng xuất hiện ở đâu trong file.

[1]: https://dev.mysql.com/mysqld.html "4.3.1 mysqld — The MySQL Server"
[2]: https://dev.mysql.com/server-options.html#option_mysqld_verbose
[3]: https://dev.mysql.com/server-options.html#option_mysqld_help
[4]: https://dev.mysql.com/mysql-config-editor.html "4.6.6 mysql_config_editor — MySQL Configuration Utility"
[5]: https://dev.mysql.com/option-file-options.html#option_general_login-path
[6]: https://dev.mysql.com/mysql.html "4.5.1 mysql — The MySQL Command-Line Tool"
[7]: https://dev.mysql.com/mysqladmin.html "4.5.2 mysqladmin — Client for Administering a MySQL Server"
[8]: https://dev.mysql.com/option-file-options.html#option_general_defaults-extra-file
[9]: https://dev.mysql.com/mysql-installer.html "2.3.3 MySQL Installer for Windows"
[10]: https://dev.mysql.com/source-configuration-options.html#option_cmake_sysconfdir
[11]: https://dev.mysql.com/mysqld-safe.html "4.3.2 mysqld_safe — MySQL Server Startup Script"
[12]: https://dev.mysql.com/server-options.html#option_mysqld_datadir
[13]: https://dev.mysql.com/server-options.html#option_mysqld_user
[14]: https://dev.mysql.com/command-line-options.html "4.2.4 Using Options on the Command Line"
[15]: https://dev.mysql.com/string-literals.html "9.1.1 String Literals"
[16]: https://dev.mysql.com/mysql-options.html "27.8.7.50 mysql_options()"
[17]: https://dev.mysql.com/mysqldump.html "4.5.4 mysqldump — A Database Backup Program"
[18]: https://dev.mysql.com/server-system-variables.html#sysvar_sql_mode

  
