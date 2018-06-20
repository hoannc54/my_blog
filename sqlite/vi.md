## Thực hiện việc ghi đa luồng trong SQLite tại xấp xỉ cùng một thời điểm / Almost row level lock performance.

Gửi các bạn,

Giống như bạn biết SQlite là cơ sở dữ liệu đơn luồng, chúng được nhúng mặc định trong hệ điều hành Linux. Có rất nhiều nghiên cứ về việc sử dụng SQLite để lưu trữ dữ liệu. Có nhiều nghiên cứ về cách làm thế nào truy cập cơ sở dữ liệu SQLite cho nhiều luồng thực hiện việc ghi. Tôi sẽ chia sẻ nghiên cứu nhỏ của tôi về việc thực hiện ghi đa luồng trên cơ sở dữ liệu SQLite.

Hãy cùng xem ưu điểm và nhược điểm về SQLite.

### Ưu điểm

* SQLite được viết bằng ngôn ngữ lập trình C thuần. Vì vậy nó truy cập nhanh nhất đến ổ đĩa hoặc bộ nhớ dữ liệu hoặc thực thi dữ liệu. Giống như bạn sử dụng ổ đĩa SSD.
* SQLite hỗ trợ bộ nhớ trong. Bộ nhớ trong SQLite nhanh gấp gần 2 lần. Nếu bạn hiểu vấn đề phân trang. Nó đủ nhanh.
* SQLite là đơn luồng. Vì vậy nó có nguy cơ thấp làm dữ liệu hỏng.
* Cơ sở dữ liệu SQLite trong 1 file đơn. Nên chúng ta có thể di chuyển hoặc truy cập bởi nền tảng khác rất dễ dàng.
* SQLite là hệ quản trị cho người dùng đầu cuối.
* Cross platform (Đa nền tảng). SQLite có thể được sử dụng trên tất cả nền tảng hệ điều hành chính.
OPENSOURCE OPENSOURCE OPENSOURCE !!!
* Và vân vân ………

### Một vài nhược điểm

* Giống như chúng tôi đã đề cập SQLite là đơn luồng. Vì vậy có nghĩa là SQLite có thể thực hiện 1 hành động ghi tại một thời điểm
* Giống như chúng tôi đã đề cập SQLite giữ dữ liệu trong 1 file. Vì vậy, có nghĩa là toàn bộ dữ liệu bị khó trong thời gian thực hiện ghi. Điều này rất không phù hợp đối với cơ sở dữ liệu có lượng truy cập lớn và thường xuyên.
* Không yêu cầu các mức xác thực.

### Tạo đa luồng trong cùng một thời điểm

Bây giờ, tôi sẽ cố gắng đưa ra mẹo nhỏ để làm cho các thao tác ghi được thực hiện gần như cùng một lúc.

Chú ý: SQLite không bao giờ cho phép bạn tác động ROW LEVEL LOCK. Vì vậy không cần phải làng phí thời gian nghiên cứu về nó.

Dĩ nhiên nó có thể tác động đến hiệu năng nếu bạn thực hiện hành động ghi trên phần lớn bảng. Nhưng Luồng thứ 2 sẽ không chờ nhiều thời gian cho đến khi kết thúc hành động đầu tiên.

Bời vì như bạn biết index B TREE là siêu nhanh. Và hành động GHI của bạn sẽ thực hiện theo ID từng dòng đã được index mặc định.

Ví dụ

delete from table where name='Fariz' // Nó sẽ khoá toàn bộ dữ liệu chính trong 10 giây.

THAY BẰNG: 

`insert into tepm.Lock select rowid,'processid' from table where name = 'Fariz'`; // Nó chỉ khoá dữ liệu tạm thời trong 10s

`delete from table where ROWID in ( select rowid from tepm.Lock where name='fariz' and processid='XYZ' ) ` // việc xoá sẽ khoá file cơ sở dữ liệu trong  0.001 s.

Tôi đã hoàn thành thử nghiệm nhỏ và kết quả tuyệt vời với tôi. Tôi đã có thể hoàn thành 2 hành động cập nhật trong 11s, đáng lẽ mỗi giao dịch mất 10s cho bảng với khoảng 40 triệu dòng.



