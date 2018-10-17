# Tại sao bạn lên học Go ?

Trong vài năm qua, có một ngôn ngữ lập trình mới đang nổi lên : Go hoặc GoLang. Không có gì làm một developer trở nên crazy hơn là một ngôn ngữ lập trình mới phải không ? Vì vậy tôi đã bắt đầu học Go trước 4-5 tháng và ở đây tôi sẽ nói cho bạn tại sao bạn cũng nên học ngôn ngữ mới này. 

Tôi sẽ không dạy bạn, làm thế nào bạn viết "Hello world!" trong bài viết này. Có rất nhiều các bài viết online cho điều đó. Tôi sẽ giải thích thời điểm hiện tại của phần cứng-phần mềm máy tính và tại sao chúng ta cần ngôn ngữ mới giống như Go ? Bởi vì nếu không có bất kỳ vấn đề gì, thì chúng ta không cần giải quyết đúng không ?

## Giới hạn phần cứng

Định luật Moore là sai

Bộ xử lý Pentium 4 đầu tiên với tốc độ 3.0 GHz được Intel giới thiệu vào năm 2004 . Ngày ngay, Macbook Pro 2016 của tôi có tốc độ 2.9 GHz. Vì vậy, gần một thập kỷ, không có quá nhiều thay đổi về hiệu năng của bộ xử lý. Bạn có thể thấy sự so sánh về sự gia tăng hiệu năng của bộ xử lý theo thời gian trong biểu đồ dưới đấy ![](https://cdn-images-1.medium.com/max/1600/1*Azz7YwzYYR6lDKFj8iIGZg.png)

Từ biểu đồ phía trên bạn có thể thấy hiệu suất của single-thread và tần số của bộ xử lý vẫn ổn định trong gần một thập kỷ. Nếu bạn đang nghĩ rằng việc thêm nhiều transistor là giải quyết được, thì bạn đã sai. Điều này là do ở quy mô nhỏ hơn một số tính chất lượng tử bắt đầu xuất hiện (giống như tunneling) và bởi vì nó thực sự tốn chi phí hơn để đưa thêm transistor ([Tại sao ](https://www.quora.com/What-is-Quantum-Tunneling-Limit-How-does-it-limit-the-size-of-a-transistor)) và số lượng các transistor bạn có thể thêm trên mỗi đô la bắt đầu giảm.

Vì vậy, để giải quyết vấn đề trên: 
* Các nhà sản xuất bắt đầu bổ sung thêm nhiều core hơn cho bộ vi xử lý. Ngày nay, chúng ta có CPU quad-core và octa-core.
* Chúng ta cũng được giới thiệu về hyper-threading
* Đã thêm bộ nhớ cache vào bộ xử lý để tăng hiệu suất.

Nhưng các giải pháp trên cũng có những hạn chế riêng. Chúng ta không thể thêm quá nhiều cache để bộ vi xử lý tăng hiệu suất vì bộ nhớ cache có giới hạn vật lý: Bộ nhớ cache càng lớn, tốc độ càng chậm. THêm nhiều core cho bộ vi xử lý cũng tốn chi phí. Ngoài ra, điều đó không thể mở rộng đến vô thời hạn. Bộ vi xử lý nhiều core có thể chạy nhiều luồng đồng thời và mang đến sự đồng thời cho toàn bộ. Chúng ta sẽ thảo luận về nó sau.

Vì vậy, nếu chúng ta không thể trả lời cho việc cải thiện phần cứng, chỉ còn cách duy nhất là cải thiện phần mềm để tăng hiệu suất. Nhưng thật đáng buồn, ngôn ngữ lập trình hiện đại lại không hiệu quả lắm. 

"Bộ vi xử lý hiện đại giống như những chiếc xe vui nhộn chạy bằng nitro, chúng vượt trội hàng quý. Không may các ngôn ngữ hiện đại giống như  Monte Carlo, chúng có nhiều vòng xoắn và quay" David Ungar

## Go has goroutines !

Giống như chúng ta đã thảo luận ở trên, các nhà sản xuất phần cứng đang thêm nhiều hơn core cho các bộ vi xử lý để tăng hiệu suất. Tất cả các trung tâm dữ liệu đang chạy trên nhiều bộ vi xử lý và chúng ta nên kỳ vọng vào việc tăng số core trong các năm tới. Thêm vào đó, ứng dụng ngày nay sử dụng nhiều micro service cho việc duy trì kết nối cơ sở dữ liệu, message queue và duy trì cache. Vì vậy, phần mềm chúng ta phát triển và ngôn ngữ lập trình nên hỗ trợ đồng thời dễ dàng và chúng nên được mở rộng với việc tăng số lượng core.

Nhưng, hầu hết các ngôn ngữ lập trình hiện đại (như Java, Python ..) là từ 90s môi trường đơn luồng. Hầu hết các ngôn ngữ lập trình hỗ trợ đa luồng. Nhưng vấn đề thực tế đi liền với việc thực thi đồng thời, luồng bị khóa, race conditions và deadlock. Những thứ đó gây khó khăn để tạo một ứng dụng đa luồng trên các ngôn ngữ đó. 

Một ví dụ, tạo một luồng trên Java không thực sự hiệu quả về bộ nhớ. Mỗi luồng tốn khoảng 1MB kích thước bộ nhớ heap và cuối nếu bạn bắt đầu xoay vòng 1 nghìn luồng, chúng sẽ tạo áp lực lớn lên heap và sẽ gây ra việc ngừng do hết bộ với. Ngoài ra, nếu bạn muốn giao tiếp giữa hai hay nhiều luồng, điều đó là rất khó.

Mặt khác, Go được release vào năm 2009 khi bộ xử lý nhiều core đã có sẵn. Đó là lý do tại sao Go được xây dựng để duy trì sự đồng thời trong suy nghĩ. Go có goroutines thay thế cho luồng. Chúng mất khoảng 2Kb bộ nhớ heap. Vì vậy, bạn có thể xoay vòng hàng triệu goroutine bất cứ lúc nào

![](https://cdn-images-1.medium.com/max/1600/1*NFojvbkdRkxz0ZDbu4ysNA.jpeg) 

Các lợi ích khác là:
* Goroutine có các đoạn ngăn xếp có thể phát triển. Điều đó nghĩa là chúng sẽ sử dụng nhiều bộ nhớ khi cần.
* Goroutine có tốc độ khởi động nhanh hơn luồng
* Goroutine đi kèm với built-in primitives để giao tiếp an toàn giữa chính chúng. (channel)
* Goroutines cho phép bạn tránh phải sử dụng khóa mutex khi chia sẻ cấu trúc dữ liệu.
* Ngoài ra, các luồng goroutines và OS không có ánh xạ 1: 1. Một goroutine đơn có thể chạy trên nhiều luồng. Goroutine được ghép thành một số lượng nhỏ các luồng hệ điều hành.

_Bạn có thể thấy sự tuyệt vời của Rob Pike không phải là sự tương đồng để hiểu sâu hơn về điều này._

Tất cả các điểm trên, làm cho Go rất mạnh mẽ để xử lý đồng thời như Java, C và C ++ trong khi vẫn giữ code thực hiện đồng thời và đẹp như Erlang.

![](https://cdn-images-1.medium.com/max/1600/1*xbsHBQJReC5l_VO4XgNSIQ.png)

## Chạy trực tiếp trên nền phần cứng

Một lợi ích đáng kể nhất của việc sử dụng C, C ++ so với các ngôn ngữ bậc cao hiện đại khác như Java / Python là hiệu năng của chúng. Bởi vì C / C ++ được biên dịch và không được thông dịch.

Bộ xử lý hiểu các tệp nhị phân. Nói chung, khi bạn xây dựng một ứng dụng bằng cách sử dụng Java hoặc các ngôn ngữ dựa trên JVM khác khi biên dịch dự án của bạn, nó biên dịch mã có thể đọc được thành mã byte có thể được JVM hoặc các máy ảo khác chạy trên hệ điều hành cơ bản. Trong khi thực hiện, VM thông dịch các bytecode đó và chuyển đổi chúng thành các tập tin nhị phân mà bộ xử lý có thể hiểu được.

![](https://cdn-images-1.medium.com/max/1600/1*TVR-VLVg68KwCOLjqQmQAw.png)

Trong khi ở phía bên kia, C / C ++ không thực hiện trên máy ảo và loại bỏ một bước từ chu kỳ thực hiện và tăng hiệu suất. Nó trực tiếp biên dịch mã có thể đọc được của con người thành mã nhị phân.

![](https://cdn-images-1.medium.com/max/1600/1*ii6xUkU_PchybiG8_GnOjA.png)

Nhưng, giải phóng và phân phối biến trong những ngôn ngữ đó là một khó khăn lớn. Trong khi hầu hết các ngôn ngữ lập trình xử lý phân phối đối tượng và loại bỏ bằng cách sử dụng Garbage Collector hoặc thuật toán đếm tham chiếu.

Go mang lại sự tốt nhất của cả thế giới. Giống như các ngôn ngữ cấp thấp hơn như C / C ++, Go là ngôn ngữ được biên dịch. Điều đó có nghĩa là hiệu suất gần như gần hơn với các ngôn ngữ cấp thấp hơn. Nó cũng sử dụng garbage collection  để phân phối và loại bỏ đối tượng. Vì vậy, không có thêm các câu lệnh malloc () và free () !!! Tuyệt!!!

## Code được viết bằng Go rất dễ bảo trì

Hãy để tôi nói với bạn một điều. Go không có cú pháp lập trình điên rồ như các ngôn ngữ khác. Nó có cú pháp rất gọn gàng và sạch sẽ.

Các nhà thiết kế của Go tại google luôn nghĩ về điều này trong tâm trí khi họ đã tạo ra ngôn ngữ. Vì google có nền tảng code lớn và hàng nghìn developer đang làm việc trên cùng một nền tảng code đó nên code đơn giản để hiểu được cho các developer khác và một đoạn code nên có ảnh hưởng ngoài tối thiểu trên một đoạn code khác. Điều đó sẽ làm cho mã dễ bảo trì và dễ sửa đổi

Go cố tình bỏ qua nhiều tính năng của ngôn ngữ OOP hiện đại
* Không có class nào. Mọi thứ chỉ được chia thành các gói. Go chỉ có cấu trúc thay vì các class.
* Không hỗ trợ kế thừa. Điều đó sẽ làm cho code dễ sửa đổi. Trong các ngôn ngữ khác như Java / Python, nếu class ABC kế thừa class XYZ và bạn thực hiện một số thay đổi trong class XYZ, thì có thể tạo ra một số tác dụng phụ trong các class khác kế thừa XYZ. Bằng cách loại bỏ thừa kế, Go làm cho nó dễ hiểu mã cũng (vì không có siêu class để xem xét trong khi nhìn vào một đoạn mã).

* Không có constructor
* Không chú thích
* Không generic
* Không exception

Những thay đổi trên làm cho Go khác với các ngôn ngữ khác và nó làm cho việc lập trình trong Go khác với các ngôn ngữ khác. Bạn có thể không thích một số điểm trên. Tuy nhiên, nó không giống như bạn không thể code ứng dụng của bạn mà không có các tính năng trên. Tất cả những gì bạn phải làm là viết 2-3 dòng nữa. Nhưng về mặt tích cực, nó sẽ làm cho code của bạn sạch hơn và rõ ràng hơn.

![](https://cdn-images-1.medium.com/max/1600/1*nlpYI256BR71xMBWd1nlfg.png)

Biểu đồ trên hiển thị rằng Go hiệu quả gần như C / C ++, trong khi vẫn giữ cú pháp mã đơn giản như Ruby, Python và các ngôn ngữ khác. Đó là một trường hợp có lợi cho cả con người và bộ vi xử lý !!!

Không giống như các ngôn ngữ mới khác như Swift, cú pháp của Go rất ổn định. Nó vẫn như cũ kể từ khi phát hành công khai ban đầu 1.0, trở lại trong năm 2012. Điều đó làm cho nó tương thích ngược.

## Go được hỗ trợ bởi Google

* Tôi biết đây không phải là một lợi thế kỹ thuật trực tiếp. Tuy nhiên, Go được thiết kế và hỗ trợ bởi Google. Google có một trong những cơ sở hạ tầng đám mây lớn nhất trên thế giới và nó được mở rộng quy mô. Go được thiết kế bởi Google để giải quyết các vấn đề của họ về hỗ trợ khả năng mở rộng và hiệu quả. Đó là những vấn đề tương tự bạn sẽ phải đối mặt trong khi tạo ra các máy chủ của riêng bạn.
* Thêm vào đó Go cũng được sử dụng bởi một số công ty lớn như Adobe, BBC, IBM, Intel và thậm chí Medium (Nguồn: https://github.com/golang/go/wiki/GoUsers)

## Phần kết luận

* Mặc dù Go rất khác với các ngôn ngữ hướng đối tượng khác, nó vẫn là cùng một loại. Go cung cấp cho bạn hiệu suất cao như C / C ++, xử lý đồng thời siêu hiệu quả như Java và thú vị với code như Python / Perl
* Nếu bạn không có bất kỳ kế hoạch học Go, tôi vẫn sẽ nói giới hạn phần cứng đặt áp lực cho chúng ta, các developer phần mềm để viết code siêu hiệu quả. Developer cần hiểu phần cứng và làm cho chương trình của họ tối ưu hóa phù hợp. Phần mềm được tối ưu hóa có thể chạy trên phần cứng rẻ hơn và chậm hơn (như thiết bị IOT) và tác động tốt hơn đến trải nghiệm người dùng cuối.









