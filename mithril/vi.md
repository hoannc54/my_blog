# Simple application

Hãy phát triển một ứng dụng đơn giản bao gồm một số khía cạnh chính của Single Page Applications.

Một ví dụ đang chạy có thể thấy ở đây [flems: Simple Application]()

Đầu tiên hãy tạo một điểm đầu vào cho ứng dụng. Tạo một file `index.html`

```
<!doctype html>
<html>
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1" />
        <title>My Application</title>
    </head>
    <body>
        <script src="bin/app.js"></script>
    </body>
</html>
```

Dòng `<!doctype html>` cho biết đây là một HTML 5 document. Thẻ meta `charset` cho biết encodeing của document và thẻ `viewport` đưa ra cách trình duyệt mobile nên mở rộng page. Thẻ `title` chứa đoạn chữ hiển thị trên tab trình duyệt cho ứng dụng này, và thẻ `script` cho biết đường dẫn nào của file Javascript để ứng dụng sử dụng.

Chúng ta nên tạo entire ứng dụng trong một file Javascript duy nhất, nhưng làm như vậy sẽ gây khó khăn cho việc điều hướng codebase sau này. Thay vào đó, hãy chia code vào các module và tập hợp các module đó vào một bundle `bin/app.js`.

Đó là một số cách để cài đặt công cụ bundler, nhưng hầu hết được phân phối qua NPM. Trong thực tế, hầu hết các thư viện và công cụ Javascript hiện đại được phân phối qua các đó, bao gồm cả Mithril. NPM viết tắt cho Node.js Package Manager. Để download NPM, cài đặt Node.js; NPM được cài tự động với nó. Khi bạn có Node.js và NPM được cài đặt, mở command line và chạy câu lệnh này: 

```
npm init -y
```
Nếu NPM được cài đúng, một file `package.json` sẽ được cài đặt. File này sẽ chưa một bộ mô tả file của dự án. Cảm thấy tự do để chỉnh sửa project và thông tin tác giả trong file này.

Để cài đặt Mithril, theo dõi hướng dẫn trong trang installtion. Khi bạn có một project skeleton với Mithril được cài đặt, chúng ta sẵn sàng để tạo ứng dụng. 

Hãy bắt đầu bằng cách tạo một module để lưu trữ trạng thái của chúng ta. Hãy tạo một file với tên gọi `src/models/User.js`

```
// src/models/User.js
var User = {
    list: []
}

module.exports = User
```
Bây giờ hãy thêm đoạn code để load một vài dữ liệu từ một server. Để giao tiếp với một server, chúng ta có thể sử dụng Mithril's XHR utility, `m.request`. Đầu tiên, chúng ta include Mithril vào module: 

```
// src/models/User.js
var m = require("mithril")

var User = {
    list: []
}

module.exports = User
```


Tiếp theo chúng ta tạo một hàm sẽ trigger một XHR call. Hãy gọi nó `loadList`

```
// src/models/User.js
var m = require("mithril")

var User = {
    list: [],
    loadList: function() {
        // TODO: make XHR call
    }
}

module.exports = User
```

Sau đó chúng ta có thể thêm một lời gọi `m.request` để tạo một XHR request. Đối với hướng dẫn này, chúng ta sẽ tạo một lời gọi XHR đển REM API, một mock REST API được thiết kế cho rapid prototyping. API này trả về một danh sách các user từ link `GET https://rem-rest-api.herokuapp.com/api/users`. Hãy sử dụng `m.request` để tạo một XHR request và lưu trữ dữ liệu của chúng ta với response của endpoint.

```
// src/models/User.js
var m = require("mithril")

var User = {
    list: [],
    loadList: function() {
        return m.request({
            method: "GET",
            url: "https://rem-rest-api.herokuapp.com/api/users",
            withCredentials: true,
        })
        .then(function(result) {
            User.list = result.data
        })
    },
}

module.exports = User
```
Tùy chọn `method` là một HTTP method. Để nhận dữ liệu từ server mà không gây ra tác dụng phụ của server, chúng ta cần sử dụng phương thức `GET`. `url` là địa chỉ cho API endpoint. Dòng `withCredentials: true` cho biết chúng ta đang sử dụng cookies (đó là một yêu cầu cho REM API).

Lời gọi `m.request` trả về một Promise để giải quyết dữ liệu từ endpoint. Theo mặc định, Mithril lấy một body HTTP response ở dạng JSON và tự động parse nó vào object hoặc array trong Javascript. Lời gọi callback `.then` chạy khi XHR request hoàn thành. Trong trường hợp này, callback assign `result.data` array đến `User.list`

Chú ý chúng ta cũng có một `return` statement trong `loadList`. Đây là một cách chung thực hành tốt khi làm việc với Promises, nó cho phép chúng ta đăng kỳ nhiều hơn callback để chạy sau khi hoàn thành XHR request. 

Đây là mô hình đơn giản thể hiện cho 2 thành viên: `User.list` (một mảng của các user object) và `User.loadList` (một phương thức mà phổ biến `User.list` với dữ liệu server).

Bây giờ, hãy tạo một view module mà chúng ta có thể hiển thị dữ liệu từ module User model của chúng ta. 

Tạo một file được gọi là `src/views/UserList.js`. Đầu tiên, hãy include Mithril và model của chúng ta, chúng ta cần sử dụng cả hai: 

```
// src/views/UserList.js
var m = require("mithril")
var User = require("../models/User")
```

Tiếp theo, hãy tạo một Mithril component. Một component là một object đơn giản mà có một phương thức `view`: 

```
// src/views/UserList.js
var m = require("mithril")
var User = require("../models/User")

module.exports = {
    view: function() {
        // TODO add code here
    }
}
```

Theo mặc định, Mithril view được mô tả sử dụng [hyperscript](https://mithril.js.org/hyperscript.html). Hyperscript cung cấp một cú pháp vắn tắt mà có thể  thụt vào một cách tự nhiên hơn HTML cho các thẻ phức tạp, và ngoài ra, từ cú pháp đơn giản là Javascript, 