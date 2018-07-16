##Giới thiệu
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
12 yếu tố của ứng dụng luôn luôn được theo dõi bởi hệ thống quản lý phiên bản, giống như Git, Mercurial hoặc Subversion. Một bản sao chép sửa đổi của cơ sở dữ liệu theo dõi được biết đến như một _code repository_, thường được gọi ngắn gọn là _code repo_ hay _repo_

Một codebase là bất cứ repo đơn lẻ nào (trong một hệ thống quản lý sửa đổi tập trung giống như Subversion) hoặc bất cứ repos nào được chia sẻ trong một commit gốc (trong một hệ thống quản lý thay đổi phân cấp giống như Git)

![](https://12factor.net/images/codebase-deploys.png)

Luôn luôn có sự tương đồng một - một giữa codebase và ứng dụng :

* Nếu có nhiều codebase, nó không là một ứng dụng - nó là một hệ thống phân tán. Môi thành phần trong hệ thống phân tán là một ứng dụng và mỗi thành phần đó có thể tuân thủe 12-yếu tố
* Nhiều ứng dụng dùng chung code giống nhau là vi phạm 12-yếu tố. Giải pháp ở đây là các đoạn code được dùng chung đưa vào các thư viện, chúng có thể được include thông qua trình quản lý các phụ thuộc.

Chỉ có một codebase cho mỗi ứng dụng, nhưng có nhiều phiên bản triển khai cho ứng dụng. Một phiên bản _deploy_ là một thể hiện đang chạy của ứng dụng. Đây thường là một site production và một hoặc nhiều giai đoạn của site. Thêm vào đó, mỗi developer có một bản sao chép của của ứng dụng đang chạy trên môi trường phát triển local của họ, mỗi cái đó cũng đủ điều kiện để triển khai.

Codebase là như nhau trên tất cả các bản deploy, mặc dù các phiên bản khác nhau có thể hoạt động trong mỗi deploy. Ví dụg, một developer có một vài commit không được deploy trong staging; staging có một vài commit không được deploy trong production. Nhưng tất cả chúng đều dùng chung một codebase, do đó làm cho chúng có thể xác định giống như các phiên bản deploy khác nhau của cùng một ứng dụng.

## Dependencies (các phụ thuộc)
#### Khai báo rõ ràng và độc lập các phụ thuộc

Hầu hết các ngôn ngữ lập trình đều cung cấp hệ thống gói để phân phối các thư viện hỗ trợ, giống như CPAN của Perl hoặc Rubygems cho Ruby. Các thư viện được cài đặt thông qua hệ thống gói có thể được cài đặt trên toàn hệ thống (được gọi là "gói site") hoặc đựa đưa vào thư mục chứa ứng dụng (được gọi là "vendoring" hoặc "bundling").

**Một ứng dụng 12-yếu tố không bao giờ phụ thuộc trên sự tồn tại không rõ ràng của các gói của hệ thống.** Nó khai báo tất cả các phụ thuộc một cách hoàn thiện và chính xác thông qua một khai báo phụ thuộc. Hơn nữa, nó sử dụng một công cụ _cô lập các phụ thuộc_ trong quá trình thực thi để đảm bảo rằng không có phụ thuộc ngầm nào "rò rỉ" từ hệ thống xung quanh. Đặc tả phụ thuộc đầy đủ và rõ ràng được áp dụng thống nhất cho cả bản production và development.

Ví dụ, Bundler trong Ruby cung cấp định dạng khai báo `Gemfile` cho các khai báo phụ thuộc và `bundle exec` cho việc cô lập phụ thuộc. Trong Python có 2 công cụ riêng rẽ cho những bước này - Pip được sử dụng cho khai báo và Virtualenv cho cô lập. Mỗi C có Autoconf cho khai báo phụ thuộc và liên kết tĩnh có thể cung cấp phụ thuộc cô lập. Bất kể công cụ gì, khai báo và cô lập phụ thuộc phải luôn luôn được sử dụng cùng nhau - chỉ có một cái này hoặc một cái kia là không đủ đáp ứng 12-yếu tố.

Một lợi ích của việc khai báo phụ thuộc rõ ràng là nó đơn giản hoá việc cài đặt cho developer cho ứng dụng. Developer mới có thể kiểm tra codebase của ứng dụng trên máy của môi trường development của họ, chỉ yêu cầu quản lý phụ thuộc và thời gian chạy của ngôn ngữ được cài đặt là điều kiện kiên quyết. Họ sẽ có thể thiết lập mọi thứ cần thiết để chạy code của ứng dụng bằng một lệnh đã xác định _build command_ . Ví dụ, build command cho Ruby/Bundler là `bundle install` trong khi cho Clojure/Leiningen là `lein deps`.

12 yếu tố ứng dụng cũng không phụ thuộc vào sự tồn tài ngầm của bất cứ công cụ hệ thống nào. Ví dụ bao gồm ImageMagick hoặc `curl`. Trong khi những công cụ này có thể tồn tại trên nhiều hoặc hầu hết các hệ thống, không có gì đảm bảo chúng sẽ tồn tại trên tất cả các hệ thống mà ứng dụng có thể chạy trong tương lai, hoặc phiên bản được tìm thấy trên một phiên bản trong tương lai sẽ tương thích với ứng dụng. Nếu ứng dụng cần shell công cụ hệ thống, công cụ đó sẽ được đưa vào ứng dụng.


## III. Config
#### Lưu trữ cấu hình trong môi trường

Một cấu hình của ứng dụng là mọi thứ có khả năng thay đổi giữa các bản deploy ( staging, production, môi trường developer, ...). Nó bao gồm: 

* Tài nguyên xử lý cơ sở dữ liệu, Memcached, dịch vụ sao lưu khác
* Thông tin đăng nhập cho các dịch vụ bên ngoài như Amazon S3 hoặc Twitter
* Mỗi giá trị deploy như hostname cho việc deploy

Đôi khi, ứng dụng lưu trữ cấu hình của các hằng số trong code. Nó là một vi phạm của 12-yếu tố, đồi hỏi phải tách biệt nghiêm ngặt ra khỏi cấu hình code. Cấu hình khác nhau đáng kể trên các deploy , code là không.

Một bộ test xem liệu một ứng dụng đã có tất cả các cấu hình mà được tách biệt với code được cài đặt chính xác hay chưa là xác định xem liệu codebase có thể trở thành mã nguồn mở tại bất cứ thời điển nào, mà không ảnh hưởng tới bất kỳ thông tin xác thực nào không.

Lưu ý rằng định nghĩa của "cấu hình" không bao gồm cấu hình nội bộ của ứng dụng, giống như `config/routes.rb` trong Rails hoặc làm thế nào các module được kết nối trong Spring. Đây là loại cấu hình không thay đổi giữa các deploy và do đó được thực hiện tốt nhất trong code.

Một cách tiếp cận khác để cấu hình sử dụng các file cấu hình mà không được kiểm tra trong trình quản lý thay đổi, giống như `config/database.yml` trong Rails. Đây là một cải tiến lớn so với việc sủ dụng các hằng số được kiểm tra trong code repo, nhưng vẫn có điểm yếu: dễ dàng nhầm lẫn đăng ký một file cấu hình cho repo; có nhiều hướng cho các file cấu hình được phân tán ở những nơi khác nhau và định dạng khác nhau, làm khó khăn trong việc xem và quản lý tất cả các cấu hình một nơi. Hơn nữa, các định dạng này có xu hướng mang tính đặc biệt cho một ngôn ngữ hoặc một framework.

12 yếu tố lưu trữ cấu hình trong biến môi trường ( thường được gọi ngắn là env vars hoặc env). Env vars dễ dàng thay đổi giữa các phiên bản deploy không làm thay đổi code; không giống như các file cấu hình, rất ít khả năng chúng được vô tình thêm vào các repo code; và không giống như các tệp cấu hình tuỳ chỉnh, hoặc cơ chế cấu hình khác như Java System Properties, chúng là một tiêu chuẩn về các ngôn ngữ và hệ điều hành.

Mộ khía canh khác của quản lý cấu hình là theo nhóm. ĐÔi khi ứng dụng cấu hình hàng loạt thành các nhóm được đặt tên ( thường gọi là "environments") sau khi triển khai cụ thể giống như `development`, `test` và `production` trong Rails. Phương pháp này không rõ ràng quy mô: vì càng nhiều deploy của ứng dụng được tạo ra, tên môi trường mới cần thiết, giống như `staging` hoặc `qa` . Khi dự án phát triển hơn nữa, các nhà phát triển có thể thêm các môi trường đặc biệt của riêng họ như `joes-staging`, dẫn đến tạo ra tổ hợp của cấu hình mà làm cho việc quản lý deploy của ứng dụng rất dễ hỏng.

Trong 12 yếu tố ứng dụng, env vars đóng vai trò điều khiển các env vars khác. Chúng không bao giờ được nhóm lại với nhau thành "môi trường", mà thay vào đó được quản lý độc lập cho từng deploy. Đây là một mô hình tạo nên sự thuận lợi khi nâng cấp sang nhiều deploy trong suốt vòng đời ứng dụng.

## IV. Dịch vụ nền

### Điều chỉnh dich vụ nền dưới dạng tài nguyên đính kèm

Một _dịch vụ nền_ là bất kỳ dịch vụ nào mà ứng dụng sử dụng trên mạng như một phần hoạt động của nó. Ví dụ bao gồm các kho lưu trữ dữ liệu (như MySQL hoặc CouchDB), hệ thống messaging/queueing (như  RabbitMQ hoặc Beanstalkd), dịch vụ SMTP cho đầu ra email (như Postfix), và hệ thống cache (như Memcached).

Backing service như cơ sở dữ liệu được quản lý theo truyền thống bởi cùng một hệ quản trị như một ứng dụng chạy. Ngoài các dịch vụ được quản lý cục bộ này, ứng dụng cũng có thể có các dịch vụ được cung cấp và quản lý bởi bên thứ 3. Ví dụ như bao gồm dịch vụ SMTP (giống như Postmark), dịch vụ thu thập số liệu (như New Relic hoặc Loggly), dịch vụ binary asset (như Amazon S3) và ngay cả các dịch vụ truy cập API( như Twitter, Google Map, hoặc Last.fm).

Việc code cho một ứng dụng theo 12 chuẩn không phân biệt giữa dịch vụ cục bộ hay bên thứ ba. Đối với ứng dụng, cả hai đều là tài nguyên được đính kèm, được truy cập thông qua một URL hoặc locator/thông tin xác thực khác được lưu trong cấu hình. Một deploy của ứng dụng theo 12 chuẩn nên có thể hoán đổi giữa cơ sở dữ liệu MySQL cụ bộ với một dịch vụ do bên thứ 3 quản lý ( như Amazon RDS) mà không làm thay đổi bất kỳ đoạn code nào của ứng dụng. Tương tự như vậy, một server SMTP cục bộ có thể được thay đổi sang một dịch vụ SMTP của bên thứ 3 (như Postmark) mà không thay đổi code. Trong cả 2 trường hợp, chỉ xử lý tài nguyên trong cấu hình cần thay đổi.

Mỗi dịch dụ backing riêng biệt là một nguồn tài nguyên. Ví dụ, một cơ sở dữ liệu MySQL là một nguồn tài nguyên , 2 cơ sở dữ liệu MySQL( được sử dụng giữa các tầng ứng dụng) đủ điều kiện như là 2 nguồn tài nguyên riêng biệt. Ứng dụng theo 12 chuẩn xử lý các cơ sở dữ liệu giống như tài nguyên đính kèm, điều này thể hiện sự phụ thuộc không chặt chẽ chúng với deploy được đính kèm.
Tài nguyên có thể được đính kèm và tách ra từ deploy theo ý muốn. Ví dụ nếu cơ sở dữ liệu của ứng dụng họt động sai do có vấn đề phần cứng, quản trị viên của ứng dụng có thể tạo lên một máy chủ cơ sở dữ liệu mới được khôi phục từ bản sao lưu gần đây. Cơ sở dữ liệu của bản production hiện tại nên được tách ra và cơ sở dữ liệu mới được đính kèm tất cả đều không làm thay đổi bất kỳ đoạn code nào.

## V. Build, release, run

Một codebase được chuyển thành một deploy (không phải deployment) thông qua 3 giai đoạn: 

* Giai đoạn _build_ là một sự chuyển đổi một repo code vào gói thực thi đã biết như là một _build_ . Sử dụng một phiên bản của code tại một commit cụ thể bởi tiến trình deploymet, giai đoạn build sẽ lấy các phụ thuộc vendor và các biên dịch các file nhị phân và các tài nguyên.
* Giai đoạn _release_ sử dụng các file được tạo ra bởi giai đoạn build và kết hợp cới cấu hình hiện tại của deploy.
* Giai đoạn _run_ (cũng giống như "runtime") chạy ứng dụng trong môi trường thực thi, bằng cách chạy một tiến trình đã được lựa chọn của release

![](https://12factor.net/images/release.png)

**12 chuẩn ứng dụng sử dụng tách rời giữa các giai đoạn build, release và run**. Ví dụ, nó không thể thay đổi code khi chạy, vì nó không có cách nào để truyền những thay đổi trở lại gai đoạn build.

Các công cụ deployment thường cung cấp các công cụ quản lý release, đáng chú ý nhất là khả năng rollback lại phiên bản release trước. Ví dụ, công cụ deployment [Capistrano](https://github.com/capistrano/capistrano/wiki)lưu trữ các bản release trong một thư mục con với tên `releases`, nơi bản release hiện tại là một symlink đến thư mục release hiện tại. Lệnh `rollback` giúp bạn dễ dàng quay trở lại bản release trước.

Mỗi bản release nên luôn luôn có một ID duy nhất cho release, như là một timestamp cho release (như `2011-04-06-20:32:17` )hoặc một số tăng dần (như `v100`). Các bản release là một bản ghi chỉ thêm vào cuối và một bản release không thể biến đổi khi nó được tạo ra. Mọi thay đổi phải tạo mới một phiên bản release.

Các bản build được khởi tạo bởi developer của ứng dụng bất cứ khi nào code mới được deploy. Thực thi theo thời gian chạy, bởi contrast, có thể xảy ra tự động trong trường hợp như là khởi động lại server, hoặc một tiến trình bị lỗi được khởi động lại bởi tiến trình quản lý. Vì thế, giai đoạn chạy nên giữ càng ít chi tiết càng tốt, vì các vấn đề ngăn chặn ứng dụng hoạt động bất kỳ lúc nào và làm hỏng hệ thống khi không có developer sẵn sàng. Giai đoạn build có thể phức tạp hơn vì các lỗi luôn luôn xảy ra với developer người mà đang thực hiện deploy

## VI. Process

Ứng dụng đươc thực thi trong môi trường thực thi như là một hoặc nhiều tiến trình

Trong trường hợp đơn giản nhất, code là một một script độc lập, môi trường thực thi là máy tính cá nhân của developer với một ngôn ngữ được cài đặt chạy, và tiến trình được khởi động thông qua câu lệnh (ví dụ `python my_script.py`). Mặt khác , một bản production được deploy cho một ứng dụng phức tạp có thể sử dụng nhiều loại tiến trình, được khởi tạo từ 0 hoặc có thể có nhiều tiến trình đang chạy.

Các tiến trình tuân theo 12 chuẩn có dạng không trạng thái và không chia sẻ. Bất kỳ dữ liệu nào cần lưu trữ lâu dài phải được lưu trữ ở dịch vụ backing có trạng thái, thông thường là cơ sở dữ liệu.

BỘ nhớ hoặc hệ thống file của tiến trình có thể được sử dụng một cache nhỏ gọn, giao dịch đơn. Ví dụ tải xuống một file lớn, thao tác trên đó và lưu trữ kết quả thực hiện vào cơ sở dữ liệu. 12 chuẩn ứng dụng không bao giờ giả định bất kỳ cái gì đã được cache trong bộ nhớ hoặc trên ổ đĩa sẽ tiện lợi cho yêu cầu hoặc công việc sau này -- với nhiều tiến trình của mỗi loại đang chạy, khả năng cao lf một yêu cầu tương lai sẽ được phục vụ bởi một tiến trình khác. Ngay cả khi đang chỉ chạy một tiến trình, một khởi động lại (được kích hoặc bởi deploy code, thay đổi cấu hình hoặc thay đổi vị trí của môi trường thực thi tiến trình sang một vị trí vật lý khác) sẽ luôn luôn xoá sạch tất cả trạng thái của cục bộ (ví dụ như bộ nhớ và các filesystem).

Các đóng gói asset giống như **django-assetpackager** sử dụng filesystem như một bộ nhớ cache cho biên dịch các asset. Một ứng dụng tuân theo 12 chuẩn có xu hướng thực hiện biên dịch này trong cả giai đoạn build. Các bộ đóng gói asset giống như **Jammit** và **Rails asset pipeline** có thể được cấu hình thành các gói asset trong giai đoạn build.

Một số hệ thống web dựa vào "sticky sessions" đó là việc cache dữ liệu phiên người dùng trong bộ nhớ của tiến trình ứng dụng và đợi các yêu cầu tiếp từ khách như thế được điều hướng đến cùng một tiến trình. "Sticky session" là một sự vi phạm của 12 chuẩn và không nên sử dụng hoặc phụ thuộc vào nó. Các dữ liệu trạng thái là giải pháp tốt cho một nơi lưu trữ liệu có quy định về thời gian hết hạn như Memcached hoặc Redis

## VII. Port binding 
#### Cung cấp các dịch vụ thông qua cổng

Các ứng dụng web đôi khi được thực thi bên trong một webserver container. Ví dụ, ứng dụng php có thể chạy giống như là một module bên trong Apache HTTPD hoặc các ứng dụng Java chạy bên trong Tomcat.

Ứng dụng tuân theo 12 chuẩn hoàn toàn được độc lập và không phụ thuộc và injection chạy của webserver trong môi trường thực thi để tạo ra một dịch vụ web-facing. Ứng dụng web cung cấp HTTP như làm một dịch vụ gắn vào một cổng và lắng nghe yêu cầu thông qua cổng đó.

Trong môi trường phát triển cục bộ, deveploper truy cập vào URL giống như `http://localhost:5000/` để truy cập vào dịch vụ được cung cấp bởi ứng dụng của họ. Trong triển khai, một lớp điều hướng xử lý các yêu cầu điều hướng từ hostname public đến cổng web xử lý.

Điều này thông thường được thực hiện bằng cách sử dụng các khai báo phụ thuộc để thêm vào một thư viện webserver vào ứng dụng, giống như Tornado cho Python, Thin cho Ruby, hoặc Jetty cho Java và ngôn ngữ chuẩn JVM khác. Điều này hoàn toàn xảy ra trong phạm vi người dùng, đó là trong code ứng dụng. Quy định với môi trường thực thi là gắn với một cổng để phục vụ các yêu cầu.

HTTP không chỉ là dịch vụ duy nhất được cung cấp bằng cách binding qua cổng. Gần đây bất kỳ loại phần mềm server nào cũng có thể chạy thông qua tiến trình kết nối với một cổng và chờ các yêu cầu đến. Ví dụ bao gồm ejabberd (XMPP), và Redis (giao thức Redis).

Cũng cần chú ý rằng phương pháp kết nối qua cổng cũng có nghĩa là một ứng dụng có thể trở thành một dịch vụ backing cho một ứng dụng khác, bằng cách cung cấp URL cho ứng dụng backing như một tài nguyên để xử lý trong cấu hình cho ứng dụng sử dụng.

## VIII. Concurrency
### Mở rộng thông qua mô hình tiến trình

Mọi chương trình máy tính, mỗi khi chạy, được đại diện bởi một hoặc nhiều tiến trình. Các ứng dụng web đã thực hiện đa dạng các mẫu thực thi tiến trình. Ví dụ, tiến trình chạy PHP là tiến trình con của Apache, được bắt đầu dựa trên nhu cầu, được yêu cầu bởi số lượng request. Các tiến trình Java thực hiện ngược lại, với JVM cung cấp một tiến trình uber lớn để lưu trữ một lượng lớn block của các tài nguyên hệ thống (CPU và bộ nhớ) khi khởi động, với concurrency được quản lý nội bộ thông qua các luồng. Trong cả 2 trường hợp, các tiến trình chạy chỉ hiển thị một cách tối thiểu cho các developer của ứng dụng.

![Scale is expressed as running processes, workload diversity is expressed as process types.](https://12factor.net/images/process-types.png)

**Trong ứng dụng theo 12 chuẩn, các tiến trình là class đầu tiên.**  Tiến trình trong ứng dụng theo 12 chuẩn lấy mạnh từ [the unix process model for running service daemons](https://adam.herokuapp.com/past/2011/5/9/applying_the_unix_process_model_to_web_apps/).  Sử dụng mô hình này, developer có thể kiến trúc ứng dụng của họ để xử lý một khối lượng các công việc khác nhau bằng cách chỉ định từng loại công việc cho một *loại tiến trình*. Ví dụ, các request HTTP có thể được xử lý bởi một tiến trình web và các task nền chạy dài được xử lý bởi một tiến trình worker.

Điều này không loại trừ các tiến trình riêng lẻ từ các tiến trình xử lý nội bộ của chính chúng, thông qua các luồng bên trong bộ runtime VM, hoặc mô hình async/envented được tìm thấy trong các công cụ như [EventMachine](http://rubyeventmachine.com/), [Twisted](http://twistedmatrix.com/trac/), hoặc [Node.js](http://nodejs.org/).  Nhưng một máy ảo(VM) cá nhân chỉ có thể phát triển quá lớn (theo chiều dọc), nên ứng dụng cũng phải có khả năng mở rộng nhiều tiến trình cùng chạy trên nhiều máy chủ vật lý.

Mô hình tiến trình thực sự hiệu quả khi nó đến thời điểm mở rộng.  [share-nothing, horizontally partitionable nature of twelve-factor app processes](./processes)nghĩa là thêm nhiều concurrency là một hành động đơn giản và tin cậy.  Danh sách các loại tiến trình và số của các tiến trình của mỗi loại được biết đến như là *process formation*.

Các tiến trình ứng dụng theo 12 chuẩn [không nên chạy ở chế độ nềnshould never daemonize](http://dustin.github.com/2010/02/28/running-processes.html) hoặc ghi vào các file PID.  thay vào đõ, dựa vào hệ thống quản lý tiến trình của hệ điều hành (giống như [systemd](https://www.freedesktop.org/wiki/Software/systemd/), một trình quản lý tiến trình phân tán trên nền tảng đám mây, hoặc một công cụ giống như [Foreman](http://blog.daviddollar.org/2011/05/06/introducing-foreman.html) trong phát triển) để quản lý [output streams](./logs), phản hồi của tiến trình bị crash, và xử lý  quá trình khởi động và tắt do người dùng khởi tạo.

## IX. Disposability
### Tối đa hoá sức mạnh với việc khởi động nhanh và tắt nhẹ nhàng

**Các tiến trình ứng dụng theo 12 chuẩn là *disposable*, nghĩa là chúng có thể được bắt đầu hoặc dùng lại tại một thời điểm thông báo**. Điều này tạo điều kiện mở rộng mềm dẻo nhanh, triển khai nhanh chóng [code](./codebase) hoặc thay đổi [cấu hình](./config), và ổn định của các bản triển khai sản phẩm.

Các tiến trình nên cố gắng để **giảm thiểu tối đa thời gian khởi động**.  Lý tưởng nhất, một tiến trình mất 1 vài giây từ lúc khởi động command để thực thi cho đến khi tiến trình được thực hiện và sẵn sàng nhận các request hoặc các job.  Thời gian khởi động ngắn được cung cấp sự nhanh chong cho tiến trình bản [release](./build-release-run) và mở rộng;và nó hỗ trợ mạnh mẽ, bởi vì trình quản lý tiến trình có thể dễ dàng di chuyển các tiến trình đến các máy vật lý mới khi được bảo hành.

Các tiến trình **được tắt nhẹ nhàng khi chúng nhận một tín hiệu [SIGTERM](http://en.wikipedia.org/wiki/SIGTERM)** từ trình quản lý tiến trình.  Ví dụ một tiến trình web, For a web process, một quá trình tắt nhẹ nhàng bằng cách ngừng lắng nghe trên các cổng (do đó từ chối bất kỳ request mới nào), cho phép bất kỳ tiến trình hiện tại nào kết thúc và sau đó kết thúc.  Ngụ ý trong mô hình này là các request HTTP là ngắn ( không quá một vài giây), hoặc trong trường hợp đợi lâu, client không nên cố gắng kết nối lại khi kết nối bị mất.

Với một tiến trình worker, quá trình tắt nhẹ nhàng(graceful shutdown ) đạt được bằng cách return lại job hiện tại cho hàng đợi work.  Ví dụ, trên woker [RabbitMQ](http://www.rabbitmq.com/) có thể gửi một [`NACK`](http://www.rabbitmq.com/amqp-0-9-1-quickref.html#basic.nack);trên [Beanstalkd](http://kr.github.com/beanstalkd/), job được trả về queue tự động bất cứ khi nào một worker ngắt kết nối. Khóa hệ thống nền giống như [tạm dừng Job](https://github.com/collectiveidea/delayed_job#readme) cần phải chắc chắn để bản release của chúng khóa bản ghi job.  Ngụ ý trong mô hình này là tất cả các job đều là [reentrant](http://en.wikipedia.org/wiki/Reentrant_%28subroutine%29), thường đạt được bằng cách bao gói lại các kết quả trong một giao dịch, hoặc thực hiện thao tác [idempotent](http://en.wikipedia.org/wiki/Idempotence).

Các tiến trình cũng nên **có sự ổn định trước sự cố bất ngờ**, trong trường hợp phần cứng lỗi.  Mặc dù đây là sự xuất hiện ít phổ biến hơn việc graceful shutdown với `SIGTERM`, nó vẫn có thể xảy ra. Một cách tiếp cần được đề xuất sử dụng hệ thống queueing backend mạnh mẽ, giống như Beanstalkd, điều đó trả về các job cho queue khi các client mất kế nối hoặc bị timeout.  Mỗi cách, ứng dụng theo 12 chuẩn được thiết kế để xử lý các kết thúc bất ngờ hoặc không nhẹ nhàng.  [Các thiết kế bị crash](http://lwn.net/Articles/191059/) đưa khái niệm này vào [kết quả logic](http://docs.couchdb.org/en/latest/intro/overview.html).

## X. Dev/prod parity
### Giữ các bản development, staging, và production càng giống nhau càng tốt

Trong lịch sử, có những khoảng cách giữa bản development (một developer thực hiện chỉnh sửa trực tiếp trên bản [deploy](./codebase) local của ứng dụng) và bản production (một bản deploy đang chạy của ứng dụng đầu cuối).  Những khoảng cách này được biểu hiện trong 3 mặt:

* **Khoảng cách thời gian:** một developer có thể làm việc trên code mất vài ngày, vài tuần, thậm chí hàng tháng để ra được bản production.
* **Khoảng cách về nhân sự**: Các developer viết code, Các ops engineers triển khai nó.
* **Khoảng cách về công cụ**: Các developers có thể sử dụng một nhóm như là Nginx, SQLite, and OS X, trong khi bản production được triển khai sử dụng Apache, MySQL, và Linux.

**Ứng dụng tuân theo 12 chuẩn được thiết kế cho [việc triển khai liên tục ](http://avc.com/2011/02/continuous-deployment/) bằng cách giữ cho khoảng cách giữa bản development và production nhỏ.**  Nhìn vào 3 khoảng cách được mô tả ở trên:

* Làm khoảng cách thời giản nhỏ: một developer có thể viết code và nó được triển khai sau vài giờ hoặc vài phút.
* Làm cho khoảng cách nhân sự nhỏ: các developer người mà viết code có liên hệ chặt chẽ trong việc triển khai và xem xét các hoạt động trong production.
* Làm cho khoảng cách công cụ nhỏ: Giữ cho bản development và production càng giống nhau càng tốt.

Tổng kết trong bảng dưới:

<table>
  <tr>
    <th></th>
    <th>Ứng dụng truyền thống</th>
    <th>Ứng dụng theo 12 chuẩn</th>
  </tr>
  <tr>
    <th>Thời gian giữa các bản deploy</th>
    <td>tuần</td>
    <td>giờ</td>
  </tr>
  <tr>
    <th>Code của tác giả và code của developẻ</th>
    <td>Khác nhau về con người</td>
    <td>Same people</td>
  </tr>
  <tr>
    <th>Môi trường Dev và production</th>
    <td>khác nhau </td>
    <td>Càng giống nhau càng tốt</td>
  </tr>
</table>

[Backing services](./backing-services), giống như là cơ sở dữ liệu, hệ thống queueing , hoặc cache, là môt nơi mà dev/prod được coi là rất quan trọng.  Nhiều ngôn ngữ cung cấp các thư viện giúp đơn giản hoá quyền truy cập vào các dịch vụ nền, bảo gồm *adapters* để khác nhau về loại dịch vụ.  Một vài ví dụ được thể hiện bảng dưới

<table>
  <tr>
    <th>Loại</th>
    <th>Ngôn ngữ</th>
    <th>Thư viện</th>
    <th>Adapters</th>
  </tr>
  <tr>
    <td>Database</td>
    <td>Ruby/Rails</td>
    <td>ActiveRecord</td>
    <td>MySQL, PostgreSQL, SQLite</td>
  </tr>
  <tr>
    <td>Queue</td>
    <td>Python/Django</td>
    <td>Celery</td>
    <td>RabbitMQ, Beanstalkd, Redis</td>
  </tr>
  <tr>
    <td>Cache</td>
    <td>Ruby/Rails</td>
    <td>ActiveSupport::Cache</td>
    <td>Memory, filesystem, Memcached</td>
  </tr>
</table>

Các developer đôi khi thấy rất tuyệt khi sử dụng các dịch vụ nền nhẹ trong môi trường local, trong khi các dịch vụ nền chuẩn được sử dụng ở bản production.  Ví dụ, sử dụng SQLite locally và PostgreSQL trong bản production; hoặc bộ nhớ tiến trình local cho quá trình cache trong bản development và Memcached trong production.

**Developer tuân theo 12 chuẩn chống lại việc sử dụng các dịch vụ nền khác nhau giữa môi trường development và production**, ngày cả khi các adapter thích nghi với bất cứ sự khác nhau nào trong dịch vụ nền.  Sự khac nhau giữa các dịch vụ nền có nghĩa là các tính không tương thích nhỏ sẽ tăng nên, làm cho code hoạt động và vượt quá việc test trong môi trường development hoặc staging nhưng sai trong môi trường production.  Các loại lỗi này tạo ra rào cản làm mất sự thống nhất trong quá trình phát triển.  Chi phí của rào cản này và sự giảm nhẹ tiếp theo của việc tiếp tục triển khai là lớn khi được xem xét tổng thể trong suốt vòng đời ứng dụng.

Các dịch vụ nhẹ trên local ít hấp dẫn hơn trước.  Các dịch vụ nền hiện đại như Memcached, PostgreSQL, và RabbitMQ không khó trong việc cài đặt và chạy nhờ các hệ thống đóng gói hiện đại, như [Homebrew](http://mxcl.github.com/homebrew/) và [apt-get](https://help.ubuntu.com/community/AptGet/Howto).  Ngoài ra, các công cụ cho phép khai báo như [Chef](http://www.opscode.com/chef/) và [Puppet](http://docs.puppetlabs.com/) kết hợp với môi trường ảo nhẹ như [Docker](https://www.docker.com/) và [Vagrant](http://vagrantup.com/) cho phép các developer chạy trên môi trường local gần đúng với môi trường production. Chi phí cài đặt và sử dụng của các hệ thống này là thấp so với lợi ích dev/prod và việc triển khai liên tục.

Các Adapter cho các dịch vụ nền khác nhau vẫn hữu dụng, vì nó làm cho việc chuyển sang các dịch vụ nền mới tương đối không vất vả.  Nhưng tất cả các bản deploy của ứng dụng (developer environments, staging, production) nên sử dụng loại và phiên bản giống nhau cho mỗi dịch vụ nền.

## XI. Logs
### Coi các log như là các luồng sự kiện

*Logs* cung cấp khả năng hiển thị hoạt động của ứng dụng đang chạy.  Trong môi trường trên máy chủ, chúng thường được ghi vào một file trên ổ đĩa (một "logfile"); nhưng đây chỉ là định dạng đầu ra.

Logs là [luồng](https://adam.herokuapp.com/past/2011/4/1/logs_are_streams_not_files/) của sự kiện tổng hợp, các sự kiện được sắp xếp theo thời gian được thu thập từ luồng đầu ra của tất cả các tiến trìn và dịch vụ nền đang chạy.  Các Log dạng thô thường là định dạng văn bản với mỗi sự kiện trên mỗi dòng ( mặc dù backtraces từ các  exception có thể kéo dài nhiều dòng).  Các log không có bắt đầu và kết thúc cố định, nhưng luồng là liên tục miễn là ứng dụng đang hoạt động.

**Một ứng dụng theo 12 chuẩn không bao giờ quan tâm đến việc định tuyến hoặc lưu trữ luồng đầu ra của nó.**  Nó không nên cố gắng ghi hoặc quản lý file log.  Thay vào đó, mỗi tiến trình ghi chúng vào luồng sự kiện, không bị chặn vào `stdout`.  Trong quá trình phát triển local, developer sẽ xem luồng này ở termianl để quan sát hoạt động của ứng dụng.

Trong các bản deploy của  staging hoặc production , mỗi luồng tiến trình sẽ được bắt bởi môi trường thực thi, được xem xét cùng với tất cả các luồng khác từ ứng dụng và được định tuyến một hoặc nhiều điểm cuối để xem và lưu trữ lâu dài.  Những điểm lưu trữ này không hiển thị hoặc cấu hình bởi ứng dụng và thay vào đó được quản lý hoàn toàn bởi môi trường thực thi. Log của router Open-source (giống như [Logplex](https://github.com/heroku/logplex) và [Fluentd](https://github.com/fluent/fluentd)) sẵn sàng cho mục đích này.

Luồng sự kiện của ứng dụng có thể được định tuyến đến một file và được xem trực tiếp qua terminal.  Quan trọng nhất, luồng có thể được gửi tới hệ thống phân tích và index  giống như [Splunk](http://www.splunk.com/), hoặc một hệ thống kho dữ liệu đa năng như[Hadoop/Hive](http://hive.apache.org/).  Những hệ thống này cho phép sức mạnh và sự linh hoạt tuyệt vời cho việc phân tích hoạt động ứng dụng theo thời gian, bao gồm: 

* Tìm sự kiện của thể trong quá khứ.
* Tạo đồ thị có quy mô lớn theo xu hướng (như là các request trêm phút).
* Cảnh báo hoạt động theo chuẩn đoán do người dùng xác định( chẳng hạn thông báo khi số lượng lỗi mỗi phút vượt ngưỡng nhất định).

## XII. Các tiến trình Admin
### Chạy các task admin/management như là tiến trình 1 lần

[Thông tin tiến trình](./concurrency) là một danh sách các tiến trình được sự dụng để thực hiện công việc business thông thường ( chẳng hạn như xử lý các request web) khi nó chạy.  Đặc biệt, các developer sẽ muốn thực hiện các task quản trị hoặc bảo trì một lần cho ứng dụng, chẳng hạn như:

* Running database migrations (e.g. `manage.py migrate` in Django, `rake db:migrate` in Rails).
* Running a console (also known as a [REPL](http://en.wikipedia.org/wiki/Read-eval-print_loop) shell) to run arbitrary code or inspect the app's models against the live database.  Most languages provide a REPL by running the interpreter without any arguments (e.g. `python` or `perl`) or in some cases have a separate command (e.g. `irb` for Ruby, `rails console` for Rails).
* Running one-time scripts committed into the app's repo (e.g. `php scripts/fix_bad_records.php`).

One-off admin processes should be run in an identical environment as the regular [long-running processes](./processes) of the app.  They run against a [release](./build-release-run), using the same [codebase](./codebase) and [config](./config) as any process run against that release.  Admin code must ship with application code to avoid synchronization issues.

The same [dependency isolation](./dependencies) techniques should be used on all process types.  For example, if the Ruby web process uses the command `bundle exec thin start`, then a database migration should use `bundle exec rake db:migrate`.  Likewise, a Python program using Virtualenv should use the vendored `bin/python` for running both the Tornado webserver and any `manage.py` admin processes.

Twelve-factor strongly favors languages which provide a REPL shell out of the box, and which make it easy to run one-off scripts.  In a local deploy, developers invoke one-off admin processes by a direct shell command inside the app's checkout directory.  In a production deploy, developers can use ssh or other remote command execution mechanism provided by that deploy's execution environment to run such a process.




