
## Giới thiệu

Token based authentication là phương thức phổ biến trên nền tảng web ngày nay. Với hầu hết các công ty web sử dụng API, token là cách tốt nhất để xác thực cho nhiều user

Có một số yếu tố quan trọng khi chọn token dựa trên xác thực cho ứng dụng của bạn. Lý do chính cho token là : 
Các máy chủ không trạng thái và có thể mở rộng
Ứng dụng mobile sẵn sàng
Chuyển xác thực cho cá ứng dụng khác
Thêm bảo mật

## Ai sử dụng Token Based Authentication ?

Bất kỳ API hoặc ứng dụng web nào mà bạn thấy đều có khả năng sử dụng token. Các ứng dụng như Facebook, Twitter, Google+, GitHub và nhiều hơn nữa sử dụng token

Hãy xem chính xác cách thức hoạt động của nó. 

Tại sao Token Came Around

Trước khi chúng ta có thể thấy cách token based authentication hoạt động và lợi ích của nó, chúng ta phải xem xét các xác thực đã được thực hiện trong quá khứ

### Server Based Authentication (phương thức truyền thống)

Từ khi giao thức HTTP là phi trạng thái (stateless), điều này có nghĩa là nếu chúng ta xác thực một người dùng với một username và password, sau đó đến request tiếp theo, ứng dụng của chúng ta sẽ không biết chúng ta là ai. Chúng ta sẽ phải xác thực lại.

Đó là cách truyền thống để ứng dụng của chúng ta nhớ chúng ta là ai để lưu trữ thông tin người dùng đã đăng nhập trên server. Điều này có thể được thực hiện theo một vài cách khác trong phiên, luôn luôn nhớ hoặc lưu trữ trên ổ đĩa.

Đây là một sơ đồ mô tả một server dựa trên luồng xác thực sẽ được thấy 

![anh](https://cask.scotch.io/2014/11/tokens-traditional.png)

Do các web, ứng dụng và sự nổ lên của ứng dụng di động đã tác động đến, phương thức xác thực này đã cho thấy các vấn đề, đặc biệt trong khả năng mở rộng.

### Các vấn đề với Server dựa trên xác thực
Một vài vấn đề chính phát sinh với phương thức xác thực này: 

**Sessions** Mỗi lần một user được xác thực, server sẽ cần tạo một bản ghi ở một vài nơi trên server. Điều này thường được thực hiện trên bộ nhớ và khi có nhiều người xác thực, chi phí máy chủ của bạn sẽ tăng lên.

**Khả năng mở rộng** Từ khi các session được lưu trữ trên bộ nhớ, điều này đưa đến vấn đề về khả năng mở rộng. Khi các nhàu cung cấp Cloud của chúng ta bắt đầu sao chép các server để xử lý việc chịu tải ứng dụng, có các thông tin quan trọng trong bộ nhớ session sẽ hạn chế khả năng mửo rộng quy mô của chúng ta.

**CORS** Khi chúng ta muốn mở rộng ứng dụng của mình để cho phép dữ liệu của chúng ta được sử dụng trên nhiều thiết bị di động, chúng ta phải lo lắng về việc chia sẻ tài nguyên gốc (CORS - cross-origin resource sharing). Khi sử dụng AJAX gọi để lấy tài nguyên từ một tên miền khác (mobile đến API server), chúng ta có thể gặp sự cố với các request bị cấm

**CSRF** Chúng ta cũng sẽ có sự bảo vệ chống lại [cross-site request forgery](https://en.wikipedia.org/wiki/Cross-site_request_forgery) (CSRF). Người dùng dễ bị CSRF tấn công khi họ có thể đã xác thực với trang web của ngân hàng và điều này có thể được tận dụng để truy cập vào trang web khác.

Với những vấn đề này, khả năng mở rộng là một trong những vấn đề chính, nó hiểu rõ phải thử cách tiếp cận khác.

## Token Based được hoạt động thế nào. 

Token based authentication là phi trạng thái (stateless). Chúng ta không lưu bất kỳ thông tin nào về người dùng trên server hoặc session

Chỉ với khái niệm này giải quyết được nhiều vấn đề về việc phải lưu trữ thông tin trên máy chủ 

Không có thông tin nào về session nghĩa là ứng dụng của bạn có thể mở rộng và thêm nhiều máy chủ nếu cần mà không phải lo lắng về nơi lưu người dùng đã đăng nhập

Mặc dù việc triển khai này có thể thay đổi, nhưng nó vẫn theo như sau: 

1. Quyền truy cập của người dùng với username/password
2. Ứng dụng xác minh các thông tin xác thực 
3. Ứng dụng cung cấp một token đã đăng ký cho client
4. Client lưu trữ token và gửi nó cũng với mỗi request
5. Server xác minh token và trả về dữ liệu

Mỗi request riêng rẽ sẽ yêu cầu token. Token này nên được gửi trong HTTP header để chúng ta tiếp tục với các **ý tưởng** của các request phi trạng thái HTTP. Chúng ta cũng sẽ cần thiết lập server để chấp nhật các request từ tất cả các domain sử dụng `Access-Control-Allow-Origin: *` . Điều thú vị về chỉ định `*` trong header ACAO là nó không cho phép các request cung cấp các thông tin xác thực như xác thực HTTP, SSL client-side hoặc cookies.

Đây là đồ hoạ về thông tin giải thích quá trình: 

![Anh](https://cask.scotch.io/2014/11/tokens-new.png)

Khi chúng ta đã xác thực thông tin của chúng ta và chúng ta có token, chúng ta có thể thử hiện nhiều thứ với token này

Chúng ta thậm chí có thể tạo một permission based token và chuyển thông tin này đến ứng dụng bên thứ 3 (ví dụ một ứng dụng mobile mới chúng ta muốn sử dụng) và chúng sẽ có thể có quyền truy cập vào dữ liệu của chúngt ta nhưng chỉ thông tin mà chúng ta cho phép với token cụ thể.

## Lợi ích của token

### Phi trạng thái và có thể mở rộng

Token được lưu trữ phía client. Hoàn toàn phi trạng thái và sẵn sàng để được mở rộng. Các cân bằng tải của chúng ta có thể chuyển một người dùng đến bất kỳ máy chủ nào của chúng ta khi không cần thông tin trạng thái hay session nào.

Nếu chúng ta giữ thông tin session về một người dùng đã đăng nhập, điều này sẽ yêu cầu chúng ta tiếp tục gửi người dùng đó đến cùng máy chủ mà họ đã đăng nhập ( được gọi là mối quan hệ session)

Điều này mang lại vấn đề khi một số người dùng sẽ buộc phải đến cùng một máy chủ và điều này có thể dẫn đến một điểm truy cập với lưu lượng lớn. 

Đừng lo lắng ! Những vấn đề này đã biến mất với token khi chính token giữ dữ liệu cho người dùng đó.

### Bảo mật

Token không là cookie, được gửi theo mọi request và không có cookies nào được gửi, điều này giúp ngăn chặn tấn công CSRF. Ngay cả khi triển khai cụ thẻ lưu trữ token trong một cookies phía client, các cookie chỉ là một cơ chế lưu trữ thay vì một cơ chế xác thực. Không có thông tin nào dựa trên session để thao tác vì chúng ta không có session.

Token cũng hết hạn sau một khoảng thời gian nhất định, do đó, người dùng sẽ yêu cầu đăng nhập lại một lần nữa. Điều này giúp chúng ta vẫn bảo mật. Ngoài ra còn có khái niệm của [token revocation](https://tools.ietf.org/html/rfc7009) cho phép chúng ta vô hiệu hoá một token cụ thể và thậm chí một nhóm token dựa trên cùng một uỷ quyền.

### Khả năng mở rộng (Bạn của bạn và các quyền)

Token sẽ cho phép chúng ta xây dựng các ứng dụng mà chia sẻ quyền với người khác. Ví dụ, chúng ta có một link ngẫu nhiêu tài khoản mạng xã hội với tài khoảng chính như Facebook hoặc Twitter.

Khi chúng ta đăng nhập Twitter thông qua một dịch vụ (Như Buffer), chúng ta đang cho phép Buffer post lên Twitter stream của chúng ta.

Bằng cách sử dụng token, đó là cách chúng ta cấp quyền có chọn lọc cho các ứng dụng bên thứ 3. Chúng ta thậm chí có thể xây dựng API của chính mình và cung cấp các token cụ thể nếu người dùng của chúng ta muốn cấp quyền truy cập dữ liệu của một ứng dụng khác.

### Trên nhiều nền tảng và domain

Chúng ta đã nói một chút về CORS ở trước. Khi ứng dụng và dịch vụ của chúng ta mở rộng, chúng ta sẽ cần cung cấp quyền truy cập cho tất cả các thiết bị và ứng dụng (vì ứng dụng chúng ta sẽ chắc chắn trở lên phổ biến)

Khi API của chúng ta chỉ phân phối dữ liệu, chúng ta cũng có thể đưa ra các lựa chọn thiết kế để phân phối nội dung từ CDN. Điều này giúp loại bỏ các vấn đề mà CORS mang lại sau khi chúng ta thiết lập cấu hình header nhanh chóng cho ứng dụng của chúng ta. 

Dữ liệu và tài nguyên của chúng ta sẵn sàng cho các request từ bất kỳ domain nào ngay khi một người dùng có token hợp lệ.

### Dựa trên chuẩn

Khi tạo token, bạn có một vài tuỳ chọn. Chúng ta sẽ nghiên cứu sâu hơn về chủ đề này khi chúng ta bảo mật API trong một bài viết tiếp theo, nhưng tiêu chuẩn để sử dụng là [JSON Web Tokens](https://scotch.io/tutorials/the-anatomy-of-a-json-web-token)

Biểu đồ cho các debugger và thư hiện hiển thị các hỗ trợ cho  JSON Web Tokens. Bạn có thể thấy rằng nó có nhiều hỗ trợ trên nhiều ngôn ngữ. Điều này có nghĩa là bạn có thể sẵn sàng chuyển đổi cơ chế xác thực của mình nếu bạn cho làm điều đó trong tương lai.

## Kết luận

Đây chỉ là bề nổi của cách thức và lý do của  token based authentication. Như mọi khi trong thé giới bảo mật, có nhiều, rất nhiều cho mỗi chủ đề và nó thay đổi theo từng trường hợp sử dụng. Chúng ta thậm chí còn nghiên cứu một số chủ đề về khả năng mở rộng để phù hợp với các cuộc giao tiếp chủ chúng. 

Cái này ở mức tổng quan cao, vì vậy xin vui lòng chỉ ra bất cứ điều gì đã bị bỏ lỡ hoặc bất kỳ câu hỏi nào của bạn về vấn đề này.

Trong bài tiếp theo của chúng tôi, chúng tôi sẽ phân tích JSON Web Token. Để biết đầy đủ ví dụ code về cách xác thực Node API sử dụng JSON Web Token, hãy xem cuốn sách MEAN Machine



















