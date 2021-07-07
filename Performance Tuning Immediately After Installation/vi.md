[Source](https://www.percona.com/blog/2016/10/12/mysql-5-7-performance-tuning-immediately-after-installation/ "Permalink to MySQL 5.7 Performance Tuning Immediately After Installation")

# Điều chỉnh hiệu năng MySQL 5.7 sau khi cài đặt

Đây là blog cập nhật [blog của Stephane Combaudon's blog trong Điều chỉnh hiệu năng MySQL][1], và bao gồm việc điều chỉnh hiệu năng MySQL 5.7 sau cài đặt.
Một vài năm trước, Stephane Combaudon đã viết một bài viết blog về [10 cài đặt điều chỉnh hiệu năng MySQL sau khi cài đặt][1] nó bao gồm cho các phiên bản cũ hơn MySQL: 5.1, 5.5 và 5.6. Trong bài viết này, tôi sẽ xem xét điều chỉnh gì trong MySQL 5.7 (tập trung vào InnoDB).

Tin tốt là MySQL 5.7 có giá trị mặc định tốt hơn. Morgan Tocker đã tạo một [trang với một danh sách tính năng đã hoàn thành trong MySQL 5.7][2], và là một nơi tham khảo tốt. Ví dụ, các biên su là được thiết lập _theo mặc định_:
* innodb_file_per_table=ON
* innodb_stats_on_metadata = OFF
* innodb_buffer_pool_instances = 8 (or 1 if innodb_buffer_pool_size < 1GB)
* query_cache_type = 0; query_cache_size = 0; (disabling mutex)

Trong MySQL 5.7, chỉ có 4 biến thực sự quan trọng, chúng cần được thay đổi. Tuy nhiên, có những biến khác InnoDB và toàn cục MySQL có thể cần được điều chỉnh cho lượng công việc và phần cứng cụ thể.

Để bắt đầu, thêm các cài đặt sau vào file  my.cnf dưới phần [mysqld]. Bạn sẽ cần phải khởi động lại MySQL:

```
[mysqld] 
# các biến khác ở đây
innodb_buffer_pool_size = 1G # (điều chỉnh giá trị ngay tại đây, 50%-70% của tổng dung lượng RAM)
innodb_log_file_size = 256M 
innodb_flush_log_at_trx_commit = 1 # có thể thay đổi 2 hoặc 0 
innodb_flush_method = O_DIRECT
```


Mô tả 


| **Variable** |  **Value** |  
|--------------|:-----------:|
| innodb\_buffer\_pool\_size |  Bắt đầu với 50% 70% của tổng số RAM. Không cần kích thước đủ lớn so với kích thước của database |  
| innodb\_flush\_log\_at\_trx\_commit | * 1   (Mặc định)|
||* 0/2 (hiệu năng cao hơn, độ tin cậy kém hơn)|  
| innodb\_log\_file\_size |  128M – 2G (Không cần kích thước lớn hơn so với vùng nhớ đệm) |  
| innodb\_flush\_method |  O_DIRECT (tránh nhân đôi bộ nhớ đệm) | 

 

_**Tiếp theo là gì ?**_

Đó là một điểm khởi đầu tốt cho bất cứ cài đặt nào mới. Có một số biến khác có thể tăng hiệu năng MySQL cho một lượng công việc. Thông thường, tôi sẽ cài đặt một công cụ MySQL monitoring/graphing (ví dụ, đó là [Percona Monitoring and Management platform][3]) và sau đó kiểm tra bảng điều khiển MySQL để thực hiện điều chỉnh thêm.

_**Chúng ta có thể điều chỉnh thêm cái gì dựa trên đồ thị?**_

_InnoDB buffer pool size_. Nhìn vào các đồ thị:

![MySQL 5.7 Performance Tuning][4]

![MySQL 5.7 Performance Tuning][5]

Giống như chúng ta có thể nhìn thấy, chúng ta có thể lợi ích từ việc tăng kích thước bộ nhớ đệm InnoDB một chút lên ~10G, vì chúng ta có RAM khả dụng và số trang trống là nhỏ so với tổng số  bộ nhớ đệm.

_InnoDB log file size._ Nhìn vào đồ thị:

![MySQL 5.7 Performance Tuning][6]

Giống như chúng ta có thể nhìn thấy ở đây, InnoDB thường ghi 2.26 GB dữ liệu trên một giờ, nó thực thi tổng kích thước của các file log (2G). Chúng ta bây giờ có thể tăng biến innodb\_log\_file\_size và khởi động lại MySQL. Cách khác, sử dụng "hiển thị trạng thái engine InnoDB" để [tính toán kích thước file log tốt InnoDB][7].

_**Các biến khác**_

Đây là một số biến khác của InnoDB mà có thể điều chỉnh thêm:

innodb\_autoinc\_lock_mode

Cài đặt [innodb\_autoinc\_lock\_mode][8] =2 (chế độ xen kẽ)có thể loại bỏ sự cần thiết cho table-level AUTO-INC lock (và có thể tăng hiệu năng khi câu lệnh insert nhiều dòng được sử dụng để insert các giá trị vào table với khóa chính tự tăng *auto\_increment primary key*). Điều này yêu cầu binlog\_format=ROW hoặc MIXED  (và ROW là mặc định trong MySQL 5.7).

innodb\_io\_capacity và innodb\_io\_capacity\_max

Đây là một điều chỉnh nâng cao hơn, và chỉ có ý nghĩa khi bạn bạn đang thực hiện nhiều lệnh ghi nhiều lần (nó không áp dụng với lệnh đọc,như SELECTs). Nếu bạn thực sự cần điều chỉnh nó, các tốt nhất là biết có bao nhiêu IOPS hệ thống có thể thực hiện. Ví dụ, nếu server có một ổ SSD, chúng ta có thể thiết lập innodb_io_capacity_max=6000 và innodb_io_capacity=3000 (tối đa 50%). Nó là một ý tưởng tốt để chạy sysbench hoặc mọi công cụ benchmark khác để chuẩn hoá thông qua ổ đĩa.

Nhưng chúng ta có cần phải lo lắng về cài đặt này không? Nhìn vào đồ thị của bộ nhớ đệm "[dirty pages][9]":

![screen-shot-2016-10-03-at-7-19-47-pm][10]

Trong trường hợp này, tổng số trang xấu là cao, và nó giống như InnoDB không thể theo kịp việc đẩy chúng ra. Nếu chúng ta có một ổ đĩa phụ nhanh (ví dụ SSD), chúng ta có thể hưởng lợi từ việc tăng innodb_io_capacity and innodb_io_capacity_max.

_**Phần kết luận or TL;DR version**_

Mặc định trong MySQL 5.7 mới thì tốt hơn nhiều cho lượng công việc chung. Đồng thời chúng ta vẫn cần cấu hình các biến InnoDB để tận dụng số RAM trong máy. Sau khi cài đặt, theo dõi các bước sau:

1. Thêm các biến InnoDB vào my.cnf (như mô tả ở trên) và khởi động lại MySQL
2. Cài đặt một hệ thống monitoring, (ví dụ Percona Monitoring and Management platform)
3. Nhìn vào đồ thị và xác định MySQL cần điều chỉnh thêm không.

### Xem thêm các nguồn:

#### Posts

#### Webinars

#### Presentations

#### Free eBooks

#### Tools

### _Related_

![][11]

[1]: https://www.percona.com/blog/2014/01/28/10-mysql-performance-tuning-settings-after-installation/
[2]: http://www.thecompletelistoffeatures.com/
[3]: http://pmmdemo.percona.com
[4]: https://www.percona.com/blog/wp-content/uploads/2016/10/Screen-Shot-2016-10-03-at-12.49.22-PM.png
[5]: https://www.percona.com/blog/wp-content/uploads/2016/10/Screen-Shot-2016-10-03-at-12.48.13-PM.png
[6]: https://www.percona.com/blog/wp-content/uploads/2016/10/Screen-Shot-2016-10-03-at-12.43.52-PM.png
[7]: https://www.percona.com/blog/2008/11/21/how-to-calculate-a-good-innodb-log-file-size/
[8]: http://dev.mysql.com/doc/refman/5.7/en/innodb-auto-increment-handling.html
[9]: http://dev.mysql.com/doc/refman/5.7/en/glossary.html#glos_dirty_page
[10]: https://www.percona.com/blog/wp-content/uploads/2016/10/Screen-Shot-2016-10-03-at-7.19.47-PM.png
[11]: https://secure.gravatar.com/avatar/79877aeedbd68531a30468cd771d5d07?s=84&d=mm&r=g
[12]: https://www.percona.com/blog/author/alexanderrubin/

  
