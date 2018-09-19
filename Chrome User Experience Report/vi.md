#Chrome User Experience Report

Chrome User Experience Report cung cấp số liệu về trải nghiệm người dùng cách người dùng Chrome trong thực tế trải nghiệm các điểm đến phổ biến trên web

## Phương pháp luận 

Chrome User Experience Report được hỗ trợ bởi việc đo lường người dùng thực sự về các chỉ số trải nghiệm người dùng trên chính các web đã public, được tổng hợp từ những người dùng đã cho phép đồng bộ lịch sử duyệt web của họ, chưa thiết lập cụm mật khẩu đồng bộ, và đã bật [báo cáo thống kê sử dụng](https://www.google.com/chrome/browser/privacy/whitepaper.html#usagestats). Dữ liệu của kết quả được cung cấp thông qua: 
1. [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/), cung cấp các mức URL về chỉ số trải nghiệm người dùng cho các URL phổ biến được Crawler web của Google biết đến
2. [Public Google BigQuery project](https://bigquery.cloud.google.com/dataset/chrome-ux-report:all) tổng hợp các số liệu về trải nghiệm người dùng theo nguồn gốc, cho tất cả các nguồn gốc được biết đến bởi Crawler web của google và chia thành nhiều phần được nêu ở dưới

### Số liệu (Metrics)

Các số liệu được cung cấp công khai bởi Chrome User Experience Report được lưu trữ trên Google BigQuery được hỗ trợ bởi các chuẩn API trên nền tảng web đối với các trình duyệt hiện đại và được tổng hợp từ các origin-resolution. Chủ sở hữu trang web muốn các phân tích chi tiết hơn (URL level resolution) và thông tin chi tiết về hiệu suất của trang web và có thể sử dụng các API tương tự để thu thập dữ liệu đo lường người dùng thực tế (RUM) cho trang nguồn của họ

_Chú ý: Chrome User Experience Report hiện tại được vào hiệu suất tải trang. Theo thời gian, chúng tôi hi vọng sẽ thêm nhiều chỉ số và , cả hai đều cung cấp nhiều thông tin chi tiết hơn về tải trang và các yếu tố quan trọng khác ảnh hưởng nhiều nhất đến trải nghiệm người dùng._

Hướng dẫn về các số liệu nào cần theo dõi và tối ưu hoá, và các phương pháp tốt nhất về cách thể hiện các dữ liệu đo lường về người dùng thực, tham khảo tài liệu về hiệu suất ở giữa của chúng tôi.

#### First Paint

Được xác định bởi Paint Timing API và sẵn sàng trên Chrome M60+:

_"First Pain báo cáo thời gian khi trình duyệt lần đầu render sau khi điều hướng đến. Điều này loại trừ background mặc định, nhưng bao gồm background không mặc định. Đây là thời điểm quan trọng đầu tiên mà các developer cần quan tâm trong khi tải trang - khi trình duyệt bắt đầu render trang"_

#### First Contentful Paint

Được xác định bởi Pain Timing API và sẵn trên Chrome M60+:

_"First Contentful Paint báo cáo thời gian khi trình duyệt lần đầu render mọi text, ảnh (bao gồm ảnh nền), canvas có màu hoặc SVG. Điều này bao gồm text với webfont đang chờ xử lý. Đây là lần đầu tiên người dùng bắt đầu nhìn thấy nội dung trang."_

#### DOMContentLoaded

Được xác định bởi HTML specification: 

_"DOMContentLoaded báo cáo về thời gian khi khởi tạo HTML document đã được tải xong và phân tích cú pháp, mà không cần đợi các stylesheet, các ảnh và các khung để hoàn tất quá trình tải xuống"_

#### onload

Được xác định bởi HTML specification:

_"Sự kiện tải được đưa ra khi trang web và các tài nguyên phụ thuộc kết thúc tải"_

### Dimensions

Hiệu suất về nội dung của trang web có thể thay đổi đáng kể dựa trên loại thiết bị, đặc điểm của mạng và các giá trị khác. Để giúp phân loại và hiểu trải nghiệm người dùng trên các loại chính đó, Chrome User Experience Report cung cấp các dimension.

#### Effective Connection Type

Được xác định bởi Network Information API và sẵn sàng trên Chrome M62+: 

_"Cung cấp các loại kết nối hiệu quả (“chậm-2g”, “2g”, “3g”, “4g”, or “offline”) được xác định bởi các giá trị round-trip và băng thông dựa trên sự đo lường quan sát của người dùng"_

#### Device Type

Phân loại các loại thiết bị (phone, tablet hoặc desktop) được truyền qua User-Agent

#### Country

VỊ trí địa lý của người dùng ở các quốc gia, được suy ra theo địa chỉ IP của họ. Các đất nước được xác định bởi [ISO 3166-1 alpha-2 codes](https://en.wikipedia.org/wiki/ISO_3166-1#Officially_assigned_code_elements) tương ứng.

#### Data format

Báo cáo được cung cấp thông qua Google BigQuery giống như một tập các dữ liệu chứa các chỉ số trải nghiệm người dùng được tổng thợp thành origin-resolution. Mỗi tập dữ liệu đại diện cho một quốc gia, `country_rs` bắt dữ liệu người dùng cho người dùng ở Serbia (re là mã của Serbia). Ngoài ra, có một tập dữ liệu tổng hợp trên toàn cầu (all) để thu thập trải nghiệm toàn thế giới. Mỗi dòng trong tập dữ liệu chứa bản ghi lồng nhau về trải nghiệm người dùng cho một nguồn cụ thể, được chia theo các dimension.


Ví dụ, ở trên cho thấy một bản ghi mẫu từ Chrome User Experience Report, cho thấy rằng 12,3% của số lần tải trang có một "first paint time" đo trong khoảng 1000-1200 ms khi đang load "http://example.com" trên thiết bị "điện thoại" dựa trên kết nối giống như là "4G". Để có giá trị tích luỹ về trải nghiệm của người dùng ở thời gian first paint dưới 1200ms, bạn có thể thêm tất cả các bản ghi có giá trị "kết thúc" của biểu đồ nhỏ hơn hoặc bằng 1200.

_Chú ý: The Chrome User Experience Report không cung cấp giá trị định lượng (ví dụ: trung bình). Các giá trị như vậy có thể xấp xỉ từ dữ liệu được cung cấp, nhưng không được đưa ra trực tiếp từ báo cáo_

## Bắt đầu

Chrome User Experience Report được cung cấp như là một project public trên Google BigQuery. Để truy cập vào project, bạn cần một tài khoản Google và một Google Cloud Project: Tham khảo hướng dẫn theo từng bước của chúng tôi và hướng dẫn về cách query project


### Mẹo phân tích và thực hành tốt nhất

#### Xem xét sự khác nhau phổ biến giữa các nguồn

Các số liệu được cung cấp bởi Chrome User Experience Report cung cấp bởi dữ liệu đo lường người dùng thực tế. Kết quả là, dữ liệu phản ánh cách ường dùng thực tế trải nghiệm trang nguồn và không giống như kiểm thử tổng hợp hoặc nội bộ nơi thử nghiệm được thực hiện trong điều kiện cố định và mô phỏng, bắt đầy đủ các yếu tố bên ngoài tạo ra và đưa đến trải nghiệm cho người dùng đầu cuối

Ví dụ, sự khác biệt lượng người dùng truy cập trang gốc cụ thể có thể đóng góp những sự khác biệt ý nghĩa cho trải nghiệm người dùng. Nếu trang web thường xuyên được nhiều khách truy cập với nhiều thiết bị hiện đại hoặc thông qua một mạng nhanh hơn, kết qur có thể xuất hiện "nhanh" ngay cả khi trang wev không được tối ưu hoá. Ngược lại, một trang web được tối ưu hoá tốt sẽ thu hút lượng người dùng lớn hoặc lượng người dùng lớn hơn trên các thiết bị hoặc mạng chậm, có thể xuất hiện "chậm".

Khi thực hiện so sánh trực tiếp giữa các trang nguồn, điều quan trọng là phải tính toán và kiểm soát sự khác biệt lượng người truy cập: phân loại theo các dimension được cung cấp, chẳng hạn như loại thiết bị và loại kết nối, và xem xét các yếu tố bên ngoài giống như dân số, quốc gia nơi trang nguồn được truy cập, ....

#### Xem xét sự khác biệt về lượng người trên trang gốc

Chrome User Experience Report tổng hợp dữ liệu cho mỗi trang gốc với các giá trị "mật độ" trên tất cả các biểu đồ chỉ số dimension tổng hợp thành giá trị của "1.0". Điều này cung cấp thông tin chi tiết về sự phân bố trải nghiệm trên các dimension cho một trang gốc duy nhất. 

Tuy nhiên, khi tổng hợp dữ liệu từ nhiều nguồn, ví dụ trong một ngành dọc hoặc khu vực địa lý, hãy cẩn thận với kết quả được rút ra: việc tăng mật độ cho cùng một chỉ số trên nhiều nguồn không tính đến sự khác biệt về dân số tương đối giữa các nguồn.

Ví dụ: Trang web A có thể có 10 triệu khách truy cập, trong khi trang web B có 10 nghìn. Trong cả 2 trường hợp, mật độ biểu đồ cho mỗi nguồn gốc là "1.0", và tập dữ liệu không cung cấp bất kỳ số liệu tuyệt đối nào về quy mô lượng người dùng của nguồn riêng lẻ hoặc sự khác biệt về kích thước dân số tương đối trên nguồn. Kết quả là, nếu bạn cộng các mật độ từ A và B và trung bình kết qur, bạn sẽ coi chúng là bằng nhau mặc dù A có 3 đơn vị lưu lượng truy cập lớn hơn.

#### Xem xét sự khác biệt về dân số của Chrome

Báo cáo của Chrome User Experience được hỗ trợ bởi việc đo lường người dùng thực được tổng hợp từ những người dùng Chrome đã chọn đồng ý đồng bộ hoá lịch sử web của họ, chưa thiết lập cụm mật khẩu đồng bộ và đã bật báo cáo thống kê sử dụng. Lượng người này có thể khong đại diện cho người dùng cơ bản rộng lớn cho một nguồn cụ thể và nhiều nguồn có thể có sự khác biệt về người truy cập. Hơn nữa, dữ liệu này không tính đến người dùng có trình duyệt khác nhau và sự khác biệt trong việc chấp nhận trình duyệt ở khu vực địa lý khác nhau. 

Do đó, hãy cẩn thận với các loại kết luận được rút ra khi nhìn vào mặt cắt ngang của nguồn và khi so sánh  nguồn cụ thể: tránh sử dụng so sánh tuyệt đối và xem xét các yếu tối ngừoi dùng khác nhau nêu trong các phần ở trên

### Phản hồi và đề xuất

Chúng tôi muốn nghe phản hồi, câu hỏi và đề xuất của bạn để giúp chúng tôi cải thiện Chrome User Experience Report. Vui lòng tham gia cuộc trò chuyện trên nhóm Google công khai














