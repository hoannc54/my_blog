[Source](https://medium.com/@noufel.gouirhate/create-your-own-mvc-framework-in-php-af7bd1f0ca19 "Permalink to Create your own MVC framework in PHP – Noufel Gouirhate – Medium")

# Tạo MVC framework cho chính bạn trong PHP – Noufel Gouirhate – Medium

Trước khi học MVC, T đã phát triển các website theo cách tuần tự. Dĩ nhiên, tôi tạo một file php cho mỗi trang. Và mỗi file là sự pha trộn giữa php và html ... một sự pha trộn thực sự khó chịu. CHúng tôi càng đi vào các dự án, chúng tôi càng gặp khó khăn trong việc bảo trì nó: rất nhiều sự dư thừa trong cấu trúc HTML, khó đọc code vì php được chèn trực tiếp trong view. Ngay cả khi dự án kết thúc trong điều kiện tốt, chúng tôi thực sự bắt đầu lẫn lộng với logic của chúng tôi.

### Giải thích về MVC

Để tránh một số tình huống đó, các developer bắt đầu nghĩ về một cách tổ chức code mới của một trang web. Một cách để làm điều đó là dùng _MVC design pattern_. Mục tiêu là chia dự án thành 3 phần lớn: 

* **Model**: tương tác với database. Nó nhận, lưu trữ và lấy dữ liệu cho người dùng.
* **View**: Hiển thị thông tin cho người dùng và tương tác với dữ liệu từ controller.
* **Controller**: gửi và nhận dữ liệu từ model và đưa qua view.

![][1]

### Cấu trúc

Để cài đặt design pattern này, chúng ta sẽ cấu trúc dự án của chúng ta. Chúng ta có thể tìm thấy nhiều loại cấu trúc khác trên internet. Nhưng đây là những gì tôi gợi ý:

![][2]

Như bạn thấy, chúng ta lấy ra xương sống của MVC framework với 3 thư mục (Models, Views, Controllers) và một số nội dung khác:

* Thư mục Webroot là thư mục duy nhất có thể truy cập bởi ngừoi dùng.
* Router.php, dispatcher.php, request.php, .htaccess là một phần của hệ thống routing
* Config : tất cả cấu hình cần cho website. Chúng ta sẽ truy xuất đến file db.ph là điểm truy cập duy nhất vào cơ sở dữ liệu của chúng ta (singleton class).

### Kiến trúc toàn bộ

![][3]

Khi truy cập vào website của chúng ta, người dùng sẽ tự động được chuyến hướng đến Webroot/index.php thông qua 2 file .htaccess.

Cái đầu tiên sẽ chuyển hướng người dùng đến thư mục Webroot.

![][4]

Và cái thứ hai sẽ chuyến hướng người dùng đến index.php. Lưu ý chúng ta lưu trữ tham số (p=$1).

![][5]

index.php yêu cầu tất cả file chúng ta cần để khởi tạo bộ dispatcher. Sau khi khởi tạo một thể hiện của Dispatch class, chúng ta sẵn sàng thiết lập logic định tuyến (routing).

![][6]

### Hệ thống Routing

#### _request.php_

Mục tiêu của file này là lấy url theo request của người dùng.

![][7]

#### router.php

router lấy url được bắt bởi _request.php_ và tác url thành 3 phần khác nhau dựa trên ký tự "/":

![][8]

Những đầu vào này sẽ được xử lý bởi dispatcher. Dispatcher đang làm công việc tương tự một người kiểm soát không lưu. Khi một request mới được load, nó sẽ chọn controller và hành động theo tham số. Vì vậy với chỉ một phương thức (dispatch()), chúng ta có thể khởi chạy tất cả logic của router này.

![][9]

### Database

model của chúng ta sẽ xử lý request đến cơ sở dữ liệu của chúng ta. Vì vậy chúng ta sẽ gọi cơ sở dữ liệu của chúng ta nhiều lần. Nói một cách đơn giản, tại mỗi liên kết, chúng ta có thể tạo ra một thể hiện của cơ sở dữ liệu. Giải pháp này không thực sự hiệu quả. Tôi khuyên chúng ta nên tạo một singleton để xử lý kết nối đến cơ sở dữ liệu của chúng ta:

![][10]

### MVC

Bây giờ chúng ta thiết lập dispatcher, website của chúng ta có thể tải một hành động từ controller.

Ở đây chúng ta muốn tạo một ứng dụng todo, vì vậy chúng ta phải tạo một _tasksController.php_. Controller này sẽ yêu cầu dữ liệu từmodel Task.php và sau đó chuyển dữ liệu đến một view. Để làm cho quá trình này đơn giản hơn, chúng ta sẽ tạo một class cha Controller để xử lý việc này.

![][11]

Phương thức _set()_ sẽ merge tất cả dữ liệu mà chúng ta muốn chuyển qua view.

Phương thức _render()_ sẽ import dữ liệu với phương thức trích xuất và sau đó tải layout được request trong thư Views. Hơn nữa, điều này cho phép chúng ta có một layout để tránh lặp lại sự ngu ngốc của HTML trong view của chúng ta.

Chúng ta đã sẵn sàng làm việc trên tasksController.php. Chỉ cần kiểm tra code của chúng ta. Tôi sẽ tạo một hành động index:

![][12]

Và một cái view nhanh với một message "Hello".

Và đây là kết quả :

![][13]

MVC framework của chúng ta đã được thiết lập! Bây giờ chúng ta chỉ cần thực hiện các hành động trong CRUD cho task resource. Nếu bạn muốn biết thêm chi tiết về điều này và lấy website với các task CRUD, bạn có thể check out repo của tôi trên Github.

Và từ giờ, bạn đã phát triển một cấu trúc MVC bền vững hơn nhiều so với các trang web PHP truyền thống của chúng tôi. Nhưng vẫn còn rất nhiều việc phải làm (bảo mật, xử lý lỗi…). Các chủ đề này đã được xử lý bởi các framework như Laravel hoặc Symfony.

[1]: https://cdn-images-1.medium.com/max/1600/1*xnuMvzXzmAxYXcRrd1Wj5Q.png
[2]: https://cdn-images-1.medium.com/max/1600/1*IA0nHOylfQYxjnGwi1XGaQ.png
[3]: https://cdn-images-1.medium.com/max/1600/1*gRErOZyn7ptn373U9fv0Yg.png
[4]: https://cdn-images-1.medium.com/max/1600/1*_agMehf9fNamnUtWqnv4kg.png
[5]: https://cdn-images-1.medium.com/max/1600/1*I67GugEBv0ONYruFet_wbA.png
[6]: https://cdn-images-1.medium.com/max/1600/1*tPlzi7umbyf6JJ9WSkfx8w.png
[7]: https://cdn-images-1.medium.com/max/1600/1*3m5NfXYUAoDAllbVdS8N1w.png
[8]: https://cdn-images-1.medium.com/max/1600/1*EVNESudstEyfXwvx6b5f1Q.png
[9]: https://cdn-images-1.medium.com/max/1600/1*I9mpgAX_OpaJa35jiQfUVg.png
[10]: https://cdn-images-1.medium.com/max/1600/1*EBlYRwirAwcywwTg0T1waw.png
[11]: https://cdn-images-1.medium.com/max/1600/1*Dmg_0gOYlq5ONFlKRkfbGw.png
[12]: https://cdn-images-1.medium.com/max/1600/1*n6l3kSUruZfOxpUNmKgTHA.png
[13]: https://cdn-images-1.medium.com/max/1600/1*MSUdTGHL_ozUGdBVeixirQ.png
