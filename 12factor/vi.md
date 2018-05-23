## Giới thiệu
Trong thời đại ngày nay, phần mềm thông thường được phân phối như một dịch vụ, được gọi _web apps_ or _software-as-a-service_. 12 chuẩn ứng dụng là một phương pháp cho xây dựng các ứng dụng phần mềm như một dịch vụ:

* Sử dụng các định dạng khai báo cho cài đặt tự động, để tối thiểu hoá thời gian và chi phí cho các developer mới tham gia dự án.
* Có một **quy định rõ ràng** với hệ điều hành cơ bản, cung cấp khả năng di động tối đã giữa các môi trường thực thi.
* Thích hợp cho việc triển khai trên **nền tảng đám mây** hiện đại, giảm bớt sự phụ thuộc vào các máy chủ và hệ thống quản trị.
* **Giảm thiểu sự khác nhau** giữa quá trình phát triển và sản phẩm, cho phép sự phát triển liên tục một các nhanh chóng nhất.
* Và có thể mở rộng quy mô mà không thay đổi nhiều các công cụ, kiến trúc hoặc phương pháp phát triển.

12 phương pháp này có thể áp dụng vào ứng dụng được viết trên mọi ngôn ngữ lập trình, và nó được sử dụng bất kỳ với sự kết hợp nào của các dịch vụ sao lưu (cơ sở dữ liệu, hàng đợi, bộ nhớ cache, ...)


## Background

Những người đóng góp cho tài liệu này đã được trực tiếp tham gia vào việc phát triển và triển khai hàng trăm ứng dụng, và gián tiếp thấy sự phát triển, vận hành và mở rộng của hàng trăm nghìn ứng dụng thông qua công việc của chúng tôi trên nền tảng Heroku

Tài liệu này tổng hợp tất cả các kinh nghiệm và sự theo dõi của chúng tôi về nhiều ứng dụng phần mềm giống như dịch vụ trong thực tế. Nó là 3 khía cạnh chính về ý tưởng thực hiện cho phát triển ứng dụng, đặc biệt tập trung vào sự linh hoạt của sự phát triển liên tục của ứng dụng theo thời gian, sự linh hoạt của sự cộng tác giữa các developer làm việc trên codebase của ứng dụng và tránh lãng phí chi phí phần mềm.

Mục đích của chúng tôi là nâng cao nhận thức về một số vấn đề hệ thống mà chúng tôi đã thấy trong quá trình phát triển các ứng dụng hiện đại, để cung cấp từ vựng chung cho việc thảo luận những vấn đề này, và cung cấp một tập hợp các khái niệm giải pháp cho những vấn đề với các thuật ngữ kèm theo. Định dạng này là được lấy từ cuốn sách của Marlin Fowler [Patterns of Enterprise Application Architecture and Refactoring](https://books.google.com/books/about/Patterns_of_enterprise_application_archi.html?id=FyWZt5DdvFkC)

## Ai nên đọc tài liệu này ?

Mọi developer xây dựng các ứng dụng mà chạy như một dịch vụ. Các kỹ sư người triển khai hoặc quản lý các ứng dụng như vậy

## 12 phương pháp luận
### I. Codebase
Một codebase được theo dõi trong mọi mọi sửa đổi, và các lần triển khai
### II. Dependencies
Khai báo rõng ràng và độc lập giữa các phụ thuộc
### III. Config
Lưu trữ cấu hình trong môi trường
### IV. Backing services
Treat backing services as attached resources
### V. Build, release, run
Các giai đoạn xây dựng và chạy hoàn toàn riêng biệt
### VI. Processes
Thực thi ứng dụng dưới một hoặc nhiều tiến trình
### VII. Port binding
Export services via port binding
### VIII. Concurrency
Mở rộng quy mô thông qua mô hình process
### IX. Disposability
Maximize robustness with fast startup and graceful shutdown
Tối đa hoá sức mạnh với việc khởi động nhanh và tắt dễ dàng.
### X. Dev/prod parity
Giữ các bản phát triển, staging và sản phẩn càng giống nhau càng tốt
### XI. Logs
Coi các log như luồng sự kiện
### XII. Admin processes
Run admin/management tasks as one-off processes
Chạy tác vụ admin/quản lý giống như các tiến trình một lần

## I. Codebase
#### Một codebase được theo dõi trong bộ quản lý thay đổi, một số triển khai
Ứng dụng 12-quy chuẩn luôn luôn được theo dõi bởi hệ thống quản lý phiên bản, giống như Git, Mercurial hoặc Subversion. Một bản sao chép sửa đổi của cơ sở dữ liệu theo dõi được biết đến như một _code repository_, thường được gọi ngắn gọn là _code repo_ hay _repo_

Một codebase là bất cứ repo đơn lẻ nào (trong một hệ thống quản lý sửa đổi tập trung giống như Subversion) hoặc bất cứ repos nào được chia sẻ trong một commit gốc (trong một hệ thống quản lý thay đổi phân cấp giống như Git)

![](https://12factor.net/images/codebase-deploys.png)

Luôn luôn có sự tương đồng một - một giữa codebase và ứng dụng :

* Nếu có nhiều codebase, nó không là một ứng dụng - nó là một hệ thống phân tán. Môi thành phần trong hệ thống phân tán là một ứng dụng và mỗi thành phần đó có thể tuân thủ 12-quy chuẩn
* Nhiều ứng dụng dùng chung code giống nhau là vi phạm 12-quy chuẩn. Giải pháp ở đây là các đoạn code được dùng chung đưa vào các thư viện, chúng có thể được include thông qua trình quản lý các phụ thuộc.

Chỉ có một codebase cho mỗi ứng dụng, nhưng có nhiều phiên bản triển khai cho ứng dụng. Một phiên bản _deploy_ là một thể hiện đang chạy của ứng dụng. Đây thường là một site production và một hoặc nhiều giai đoạn của site. Thêm vào đó, mỗi developer có một bản sao chép của của ứng dụng đang chạy trên môi trường phát triển local của họ, mỗi cái đó cũng đủ điều kiện để triển khai.

Codebase là như nhau trên tất cả các bản deploy, mặc dù các phiên bản khác nhau có thể hoạt động trong mỗi deploy. Ví dụg, một developer có một vài commit không được deploy trong staging; staging có một vài commit không được deploy trong production. Nhưng tất cả chúng đều dùng chung một codebase, do đó làm cho chúng có thể xác định giống như các phiên bản deploy khác nhau của cùng một ứng dụng.

## Dependencies (các phụ thuộc)
#### Khai báo rõ ràng và độc lập các phụ thuộc

Hầu hết các ngôn ngữ lập trình đều cung cấp hệ thống gói để phân phối các thư viện hỗ trợ, giống như CPAN cho Perl hoặc Rubygems cho Ruby. Các thư viện được cài đặt thông qua hệ thống gói có thể được cài đặt trên toàn hệ thống (được gọi là "site package") hoặc đựa đưa vào thư mục chứa ứng dụng (được gọi là "vendoring" hoặc "bundling").

**Một ứng dụng 12-quy chuẩn không bao giờ phụ thuộc trên sự tồn tại không rõ ràng của các gói hệ thống.** Nó khai báo tất cả các phụ thuộc một cách hoàn thiện và chính xác thông qua một khai báo phụ thuộc. Hơn nữa, nó sử dụng một công cụ _cô lập các phụ thuộc_ trong quá trình thực thi để đảm bảo rằng không có phụ thuộc ngầm nào "rò rỉ" từ hệ thống xung quanh. Đặc tả phụ thuộc đầy đủ và rõ ràng được áp dụng thống nhất cho cả bản production và development.

Ví dụ, Bundler trong Ruby cung cấp định dạng khai báo `Gemfile` cho các khai báo phụ thuộc và `bundle exec` cho việc cô lập phụ thuộc. Trong Python có 2 công cụ riêng rẽ cho những bước này - Pip được sử dụng cho khai báo và Virtualenv cho cô lập. Mỗi C có Autoconf cho khai báo phụ thuộc và liên kết tĩnh có thể cung cấp phụ thuộc cô lập. Bất kể công cụ gì, khai báo và cô lập phụ thuộc phải luôn luôn được sử dụng cùng nhau - chỉ có một cái này hoặc một cái kia là không đủ đáp ứng 12-quy chuẩn.

Một lợi ích của việc khai báo phụ thuộc rõ ràng là nó đơn giản hoá việc cài đặt cho developer cho ứng dụng. Developer mới có thể kiểm tra codebase của ứng dụng trên máy của môi trường development của họ, chỉ yêu cầu quản lý phụ thuộc và thời gian chạy của ngôn ngữ được cài đặt là điều kiện kiên quyết. Họ sẽ có thể thiết lập mọi thứ cần thiết để chạy code của ứng dụng bằng một lệnh đã xác định _build command_ . Ví dụ, build command cho Ruby/Bundler là `bundle install` trong khi cho Clojure/Leiningen là `lein deps`.

Ứng dụng 12-quy chuẩn cũng không phụ thuộc vào sự tồn tài ngầm của bất cứ công cụ hệ thống nào. Ví dụ bao gồm ImageMagick hoặc `curl`. Trong khi những công cụ này có thể tồn tại trên nhiều hoặc hầu hết các hệ thống, không có gì đảm bảo chúng sẽ tồn tại trên tất cả các hệ thống mà ứng dụng có thể chạy trong tương lai, hoặc phiên bản được tìm thấy trên một phiên bản trong tương lai sẽ tương thích với ứng dụng. Nếu ứng dụng cần shell công cụ hệ thống, công cụ đó sẽ được đưa vào ứng dụng.


## III. Config
#### Lưu trữ cấu hình trong môi trường

Một cấu hình của ứng dụng là mọi thứ có khả năng thay đổi giữa các bản deploy ( staging, production, môi trường developer, ...). Nó bao gồm: 

* Tài nguyên xử lý cơ sở dữ liệu, Memcached, dịch vụ sao lưu khác
* Thông tin đăng nhập cho các dịch vụ bên ngoài như Amazon S3 hoặc Twitter
* Mỗi giá trị deploy như hostname cho việc deploy

Đôi khi, ứng dụng lưu trữ cấu hình của các hằng số trong code. Nó là một vi phạm của 12-quy chuẩn, đồi hỏi phải tách biệt nghiêm ngặt ra khỏi cấu hình code. Cấu hình khác nhau đáng kể trên các deploy , code là không.

Một bộ test xem liệu một ứng dụng đã có tất cả các cấu hình mà được tách biệt với code được cài đặt chính xác hay chưa là xác định xem liệu codebase có thể trở thành mã nguồn mở tại bất cứ thời điển nào, mà không ảnh hưởng tới bất kỳ thông tin xác thực nào không.

Lưu ý rằng định nghĩa của "cấu hình" không bao gồm cấu hình nội bộ của ứng dụng, giống như `config/routes.rb` trong Rails hoặc làm thế nào các module được kết nối trong Spring. Đây là loại cấu hình không thay đổi giữa các deploy và do đó được thực hiện tốt nhất trong code.

Một cách tiếp cận khác để cấu hình sử dụng các file cấu hình mà không được kiểm tra trong trình quản lý thay đổi, giống như `config/database.yml` trong Rails. Đây là một cải tiến lớn so với việc sủ dụng các hằng số được kiểm tra trong code repo, nhưng vẫn có điểm yếu: dễ dàng nhầm lẫn đăng ký một file cấu hình cho repo; có nhiều hướng cho các file cấu hình được phân tán ở những nơi khác nhau và định dạng khác nhau, làm khó khăn trong việc xem và quản lý tất cả các cấu hình một nơi. Hơn nữa, các định dạng này có xu hướng mang tính đặc biệt cho một ngôn ngữ hoặc một framework.

Ứng dụng 12-quy chuẩn lưu trữ cấu hình trong biến môi trường ( thường được gọi ngắn là env vars hoặc env). Env vars dễ dàng thay đổi giữa các phiên bản deploy không làm thay đổi code; không giống như các file cấu hình, rất ít khả năng chúng được vô tình thêm vào các repo code; và không giống như các tệp cấu hình tuỳ chỉnh, hoặc cơ chế cấu hình khác như Java System Properties, chúng là một tiêu chuẩn về các ngôn ngữ và hệ điều hành.

Mộ khía canh khác của quản lý cấu hình là theo nhóm. ĐÔi khi ứng dụng cấu hình hàng loạt thành các nhóm được đặt tên ( thường gọi là "environments") sau khi triển khai cụ thể giống như `development`, `test` và `production` trong Rails. Phương pháp này không rõ ràng quy mô: vì càng nhiều deploy của ứng dụng được tạo ra, tên môi trường mới cần thiết, giống như `staging` hoặc `qa` . Khi dự án phát triển hơn nữa, các nhà phát triển có thể thêm các môi trường đặc biệt của riêng họ như `joes-staging`, dẫn đến tạo ra tổ hợp của cấu hình mà làm cho việc quản lý deploy của ứng dụng rất dễ hỏng.

Trong 12 yếu tố ứng dụng, env vars đóng vai trò điều khiển các env vars khác. Chúng không bao giờ được nhóm lại với nhau thành "môi trường", mà thay vào đó được quản lý độc lập cho từng deploy. Đây là một mô hình tạo nên sự thuận lợi khi nâng cấp sang nhiều deploy trong suốt vòng đời ứng dụng.
