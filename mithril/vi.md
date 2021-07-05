# Ứng dụng đơn giản

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

Theo mặc định, Mithril view được mô tả sử dụng [hyperscript](https://mithril.js.org/hyperscript.html). Hyperscript cung cấp một cú pháp vắn tắt mà có thể  thụt vào một cách tự nhiên hơn HTML cho các thẻ phức tạp, và ngoài ra, từ cú pháp đơn giản là Javascript, có thể tận dụng nhiều hệ sinh thái các công cụ của Javascript, ví dụ [Babel][7], [JSX][8] (inline-HTML syntax extension), [eslint][9] (linting), [uglifyjs][10] (minification), [istanbul][11] (code coverage), [flow][12] (static type analysis), ....

Hãy sử dụng Mithril hyperscript để tạo một danh sách các item. Hyperscript là ngôn ngữ phổ biến cách viết Mithril views, nhưng [JSX là một lựa chọn phổ biế khác bạn có thể thử dùng][8] bạn sẽ thấy thoải mãi hơn với những thứ cơ bản:
    
    
    // src/views/UserList.js
    var m = require("mithril")
    var User = require("../models/User")
    
    module.exports = {
        view: function() {
            return m(".user-list")
        }
    }
    

`".user-list"` là một CSS selector, và giống như bạn muốn, `.user-list` đại diện cho một class. Khi một thẻ không được chỉ định, `div` là thẻ mặc định. Vì vậy, view này tương đương với `<div class="user-list"></div>`


Bây giờ, hãy tham khảo danh sách các user từ model chúng ta đã tạo (`User.list`) để tự động duyệt qua data:
    
    
    // src/views/UserList.js
    var m = require("mithril")
    var User = require("../models/User")
    
    module.exports = {
        view: function() {
            return m(".user-list", User.list.map(function(user) {
                return m(".user-list-item", user.firstName + " " + user.lastName)
            }))
        }
    }
    

Từ `User.list` là một mảng Javascript, và từ khi hyperscript views là Javascript, chúng ta có thể lặp thông qua mản sử dụng phương thức `.map`. Điều này tạo ra một mảng các vnodes đại diện cho một danh sách các thẻ `div`, mỗi trên chứa một user.

Vấn đề, dĩ nhiên chúng ta không bao giờ họi hàm  `User.loadList`. Mặc dù, `User.list` vẫn là một mảng rỗng và do đó view này vẫn hiển thị một trang trắng. Từ khi chúng ta muốn `User.loadList` được gọi khi chúng ta render component này, chúng ta có thể tận dụng lợi thể cửa component  [lifecycle methods][13]:
    
    
    // src/views/UserList.js
    var m = require("mithril")
    var User = require("../models/User")
    
    module.exports = {
        oninit: User.loadList,
        view: function() {
            return m(".user-list", User.list.map(function(user) {
                return m(".user-list-item", user.firstName + " " + user.lastName)
            }))
        }
    }
    

Lưu ý chúng ta đã thêm một phương thức `oninit` cho commponet, cái mà tham chiếu đến `User.loadList`. Điều đó có nghĩa là khi khởi tạo component , User.loadList sẽ được gọi, kích hoạt một XHR request. KHi server trả về một response, `User.list` được định nghĩa.

CŨng lưu ý chúng ta **không** thực hiện `oninit: User.loadList()` (với dấu ngoặc đơn ở cuối). Sự khác biệt là `oninit: User.loadList()` gọi hàm một lần và ngay lập tức, nhưng `oninit: User.loadList` chỉ gọi hàm khi component render. Đó là một sự khác biệt quan trọng và một nhầm lẫn phổ biến với các develper javascript: lời gọi hàm ngay nghĩa là XHR request sẽ được kích hoạt ngay sau khi mã nguồn được xác định, ngay cả component không bao giờ được render. Ngoài ra nếu compoent được tạo lại (thông qua ) Also, if the component is ever recreated (through navigating back and forth through the application), hàm không được gọi như mong đợi.
* * *

Hãy render view từ file đầu vào `src/index.js` chúng ta đã tạo trước đó:
    
    
    // src/index.js
    var m = require("mithril")
    
    var UserList = require("./views/UserList")
    
    m.mount(document.body, UserList)
    

`m.mount` gọi render componet xác định (`UserList`) vào một DOM element (`document.body`), xoá bất kỳ DOM nào đã có trước đó. Mở file HTML trong một trình duyệt sẽ hiển thị danh sách tên các người.

* * *

Ngay bây giờ, danh sách trông khá đơn giản vì chúng tôi chưa xác định bất kỳ style nào.

Có nhiều quy ước và thư viện tương tự giúp tổ chức các kiểu ứng dụng ngày nay. Một số, như Bootstrap chạy một tập hợp các cấu trúc HTML cụ thể và các tên lớp có ý nghĩa về ngữ nghĩa, trong đó có xu hướng cung cấp sự thiếu hụt nhận thức thấp, nhưng nhược điểm của việc tùy biến trở nên khó khăn hơn. Những người khác, như Tachyons cung cấp một số lượng lớn các tên lớp tự mô tả, mức nguyên tử với chi phí tự đặt tên lớp không phải theo ngữ nghĩa. "CSS-in-JS" là một loại hệ thống CSS khác đang ngày càng phổ biến, về cơ bản bao gồm phạm vi CSS thông qua công cụ chuyển biên. Các thư viện CSS-in-JS đạt được khả năng bảo trì bằng cách giảm kích thước của không gian vấn đề, nhưng đến với chi phí có độ phức tạp cao.

Bất kể quy ước / thư viện CSS mà bạn chọn, nguyên tắc chung là tránh khía cạnh xếp tầng của CSS. Để giữ cho hướng dẫn này đơn giản, chúng tôi sẽ chỉ sử dụng CSS đơn giản với các tên lớp rõ ràng, để các kiểu tự cung cấp mức nguyên tử của Tachyons và sự xung đột tên lớp được thực hiện không qua độ dài của tên lớp. CSS thuần có thể đủ cho các dự án có độ phức tạp thấp (ví dụ: 3 đến 6 tháng của thời gian triển khai ban đầu và một vài giai đoạn dự án).

Để thêm styles, đầu tiên hãy tạo file được gọi là `styles.css` và bao gồm file `index.html`:
    
    
    
    
        
            
            
            
            https://mithril.js.org/styles.css" rel="stylesheet">
        
        
            ">https://mithril.js.org/bin/app.js">
        
    
    

Bây giờ chúng ta có thể stlye `UserList` component:
    
    
    .user-list {list-style:none;margin:0 0 10px;padding:0;}
    .user-list-item {background:#fafafa;border:1px solid #ddd;color:#333;display:block;margin:0 0 1px;padding:8px 15px;text-decoration:none;}
    .user-list-item:hover {text-decoration:underline;}
    

CSS ở trên được viết bằng cách sử dụng quy ước giữ tất cả các style cho quy tắc trong một dòng, theo thứ tự bảng chữ cái. Quy ước này được thiết kế để tận dụng tối đa diện tích màn hình và dễ dàng quét các CSS selector (vì chúng luôn ở bên trái) và nhóm logic của chúng, và nó thực thi các quy tắc CSS có thể dự đoán và thống nhất cho mỗi selector.

Rõ ràng bạn có thể sử dụng bất kỳ khoảng trắng / indentation bạn thích. Ví dụ trên chỉ là một minh họa của một quy ước không phổ biến rộng rãi có các lý do mạnh mẽ đằng sau nó, nhưng đi chệch khỏi các quy ước khoảng trắng rộng hơn về quy chuẩn.

Tải lại cửa sổ trình duyệt bây giờ sẽ hiển thị một số style element.

* * *

Hãy thêm route vào ứng dụng của chúng ta.

Route có nghĩa là liên kết màn hình với một URL duy nhất, để làm chuyển từ một "trang" sang một trang khác. Mithril được thiết kế cho Single Page Applications, do đó, các "trang" này không nhất thiết là các file HTML khác nhau theo nghĩa truyền thống. Thay vào đó, route trong các Single Page Applications giữ lại cùng một file HTML trong suốt vòng đời của nó, nhưng thay đổi trạng thái của ứng dụng thông qua Javascript. Định tuyến phía client có lợi ích của việc tránh nhấp nháy màn hình trống giữa các chuyển tiếp trang và có thể giảm lượng dữ liệu được gửi xuống từ máy chủ khi được sử dụng kết hợp với kiến ​​trúc hướng dịch vụ web (tức là một ứng dụng tải xuống dữ liệu dưới dạng JSON thay vì tải xuống các đoạn mã chi tiết được trả trước).


Chúng ta có thể thêm route được thay đổi bởi `m.mount` gọi đến một `m.route`:
    
    
    // src/index.js
    var m = require("mithril")
    
    var UserList = require("./views/UserList")
    
    m.route(document.body, "/list", {
        "/list": UserList
    })
    

`m.route` gọi chỉ định rằng ứng dụng sẽ được hiển thị vào `document.body`. Đối số `"/list"` là mặc định của route. Điều đó có nghĩa là người dùng sẽ được chuyển hướng đến đường dẫn đó nếu họ đến một đường dẫn không tồn tại. Đối tượng `{"/list": UserList}` khai báo một bản đồ các route hiện có và các thành phần mà mỗi route xử lý.

Việc làm mới trang trong trình duyệt bây giờ sẽ thêm `#!/list` vào URL để cho biết định tuyến đang hoạt động. Do route đó render UserList, chúng ta vẫn sẽ thấy danh sách những người trên màn hình như trước đây.

`#!` được biết đến như một hashbang, và nó là một chuỗi thường được sử dụng để thực hiện định tuyến phía client. Có thể cấu hình chuỗi này qua [`m.route.prefix`][16]. Một số cấu hình yêu cầu hỗ trợ thay đổi phía máy chủ, vì vậy chúng tôi sẽ tiếp tục sử dụng hashbang cho phần còn lại của hướng dẫn này.

* * *

Hãy thêm một route khác vào ứng dụng của chúng tôi để chỉnh sửa người dùng. Đầu tiên hãy tạo một mô-đun gọi là `views/UserForm.js`
    
    
    // src/views/UserForm.js
    
    module.exports = {
        view: function() {
            // TODO implement view
        }
    }
    

Sau đó chúng ta có thể `require` module mới này từ `src/index.js`
    
    
    // src/index.js
    var m = require("mithril")
    
    var UserList = require("./views/UserList")
    var UserForm = require("./views/UserForm")
    
    m.route(document.body, "/list", {
        "/list": UserList
    })
    

Và cuối cùng, chúng ta có thể tạo một route tham chiếu đến nó:
    
    
    // src/index.js
    var m = require("mithril")
    
    var UserList = require("./views/UserList")
    var UserForm = require("./views/UserForm")
    
    m.route(document.body, "/list", {
        "/list": UserList,
        "/edit/:id": UserForm,
    })
    

Lưu ý rằng route mới có `:id` trong nó. Đây là một tham số route; bạn có thể nghĩ về nó như một thẻ gốc; route `/edit/1` sẽ xử lý `UserForm` với `id` là `"1"`. `/edit/2` cũng sẽ xử lý `UserForm`, nhưng với một `id` là `"2"`. Và cứ thế.

Hãy thực hiện `UserForm` component để nó có thể đáp ứng các thông số route đó:
    
    
    // src/views/UserForm.js
    var m = require("mithril")
    
    module.exports = {
        view: function() {
            return m("form", [
                m("label.label", "First name"),
                m("input.input[type=text][placeholder=First name]"),
                m("label.label", "Last name"),
                m("input.input[placeholder=Last name]"),
                m("button.button[type=button]", "Save"),
            ])
        }
    }
    

Và hãy thêm một số style vào `styles.css`:
    
    
    /* styles.css */
    body,.input,.button {font:normal 16px Verdana;margin:0;}
    
    .user-list {list-style:none;margin:0 0 10px;padding:0;}
    .user-list-item {background:#fafafa;border:1px solid #ddd;color:#333;display:block;margin:0 0 1px;padding:8px 15px;text-decoration:none;}
    .user-list-item:hover {text-decoration:underline;}
    
    .label {display:block;margin:0 0 5px;}
    .input {border:1px solid #ddd;border-radius:3px;box-sizing:border-box;display:block;margin:0 0 10px;padding:10px 15px;width:100%;}
    .button {background:#eee;border:1px solid #ddd;border-radius:3px;color:#333;display:inline-block;margin:0 0 10px;padding:10px 15px;text-decoration:none;}
    .button:hover {background:#e8e8e8;}
    

Ngay bây giờ, component này không có gì để phản hồi sự kiện của người dùng. Hãy thêm đoạn code vào `User` model trong `src/models/User.js`. Đây là cách mà code chạy đúng:
    
    
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
    

Hãy thêm đoạn code cho phép chúng ta load một user riêng lẻ:
    
    
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
    
        current: {},
        load: function(id) {
            return m.request({
                method: "GET",
                url: "https://rem-rest-api.herokuapp.com/api/users/" + id,
                withCredentials: true,
            })
            .then(function(result) {
                User.current = result
            })
        }
    }
    
    module.exports = User
    

Chú ý chúng ta đã thêm một `User.current` property, và một phương thức `User.load(id)` đưa vào thuộc tính. Chúng ta có thê đưa `UserForm` view sử dụng phương thức mới này:
    
    
    // src/views/UserForm.js
    var m = require("mithril")
    var User = require("../models/User")
    
    module.exports = {
        oninit: function(vnode) {User.load(vnode.attrs.id)},
        view: function() {
            return m("form", [
                m("label.label", "First name"),
                m("input.input[type=text][placeholder=First name]", {value: User.current.firstName}),
                m("label.label", "Last name"),
                m("input.input[placeholder=Last name]", {value: User.current.lastName}),
                m("button.button[type=button]", "Save"),
            ])
        }
    }
    

Tương tự như `UserList` component, `oninit` gọi `User.load()`. Hãy nhớ rằng chúng tôi đã có tham số route được gọi là  `:id` trên `"/edit/:id": UserForm` route? Tham số route trở thành một thuộc tính của `UserForm` component, vì vậy  routing đến `/edit/1` sẽ tạo  `vnode.attrs.id` có giá trị `"1"`.

Bây giờ, hãy chỉnh sửa `UserList` để chúng ta có thể điều hướng từ đó đến `UserForm`:
    
    
    // src/views/UserList.js
    var m = require("mithril")
    var User = require("../models/User")
    
    module.exports = {
        oninit: User.loadList,
        view: function() {
            return m(".user-list", User.list.map(function(user) {
                return m("a.user-list-item", {href: "/edit/" + user.id, oncreate: m.route.link}, user.firstName + " " + user.lastName)
            }))
        }
    }
    

Ở đây chúng đã thay đổi `.user-list-item` thành `a.user-list-item`. Chúng ta đã thêm một `href` để cập đến route chúng ta muốn, và cuối cùng chúng ta đã thêm `oncreate: m.route.link`. Điều này làm cho liên kết hoạt động giống như một liên kết route (trái ngược với việc chỉ hoạt động giống như một liên kết thông thường). Điều này có nghĩa là việc nhấp vào liên kết sẽ thay đổi một phần của URL xuất hiện sau hashbang `#!` (do đó thay đổi route mà không cần tải trang HTML hiện tại)

Nếu bạn làm mới trang trong trình duyệt, bây giờ bạn có thể nhấp vào một người và được đưa đến một form. Bạn cũng có thể nhấn nút quay lại trong trình duyệt để quay lại từ form tới danh sách người

* * *

Bản thân form vẫn không lưu khi bạn nhấn "Lưu". Hãy làm cho form này hoạt động:
    
    
    // src/views/UserForm.js
    var m = require("mithril")
    var User = require("../models/User")
    
    module.exports = {
        oninit: function(vnode) {User.load(vnode.attrs.id)},
        view: function() {
            return m("form", {
                    onsubmit: function(e) {
                        e.preventDefault()
                        User.save()
                    }
                }, [
                m("label.label", "First name"),
                m("input.input[type=text][placeholder=First name]", {
                    oninput: m.withAttr("value", function(value) {User.current.firstName = value}),
                    value: User.current.firstName
                }),
                m("label.label", "Last name"),
                m("input.input[placeholder=Last name]", {
                    oninput: m.withAttr("value", function(value) {User.current.lastName = value}),
                    value: User.current.lastName
                }),
                m("button.button[type=submit]", "Save"),
            ])
        }
    }
    

Chúng ta có thể thêm sự kiện `oninput` cho cả 2 đầu vào, thiết lập `User.current.firstName` và `User.current.lastName` properties khi một người dùng nhập.

Ngoài ra, chúng tôi đã mô tả phương thức `User.save` nên được gọi khi nút  "Save" được nhấp . Hãy thực hiện phương thức : 
    
    
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
    
        current: {},
        load: function(id) {
            return m.request({
                method: "GET",
                url: "https://rem-rest-api.herokuapp.com/api/users/" + id,
                withCredentials: true,
            })
            .then(function(result) {
                User.current = result
            })
        },
    
        save: function() {
            return m.request({
                method: "PUT",
                url: "https://rem-rest-api.herokuapp.com/api/users/" + User.current.id,
                data: User.current,
                withCredentials: true,
            })
        }
    }
    
    module.exports = User
    

Trong phương thức `save` ở dưới, chúng ta sử dụng phương thức HTTP `PUT` để chỉ ra rằng chúng tôi đang đẩy dữ liệu đến máy chủ.

Bây giờ hãy thử chỉnh sửa tên của người dùng trong ứng dụng. Khi bạn lưu thay đổi, bạn sẽ có thể thấy thay đổi được phản ánh trong danh sách người dùng.


* * *
Hiện tại, chúng ta chỉ có thể điều hướng trở lại danh sách người dùng thông qua nút quay lại trình duyệt. Lý tưởng nhất, chúng ta muốn có một menu - hoặc tổng quát hơn, một layout nơi chúng ta có thể đặt các element UI global.

Hãy tạo một file `src/views/Layout.js`:
    
    
    // src/views/Layout.js
    var m = require("mithril")
    
    module.exports = {
        view: function(vnode) {
            return m("main.layout", [
                m("nav.menu", [
                    m("a[href='https://mithril.js.org/list']", {oncreate: m.route.link}, "Users")
                ]),
                m("section", vnode.children)
            ])
        }
    }
    

component khá đơn giản, nó có `<nav>` với một link đến danh sách người dùng. Tương tự như những gì chúng ta đã làm với link `/edit`, link này sử dụng `m.route.link` để kích hoạt route trong link.

Chú ý cũng có một element `<section>` với `vnode.children` giống như children. `vnode` là một tham chiếu đến vnode đại diện cho một thể hiện của Layout (tức là vnode được trả về bởi một `m(Layout)`). Vì thế, `vnode.children` đề cập đến bất kỳ children nào của vnode đó. 

Hãy thêm một vài style:
    
    
    /* styles.css */
    body,.input,.button {font:normal 16px Verdana;margin:0;}
    
    .layout {margin:10px auto;max-width:1000px;}
    .menu {margin:0 0 30px;}
    
    .user-list {list-style:none;margin:0 0 10px;padding:0;}
    .user-list-item {background:#fafafa;border:1px solid #ddd;color:#333;display:block;margin:0 0 1px;padding:8px 15px;text-decoration:none;}
    .user-list-item:hover {text-decoration:underline;}
    
    .label {display:block;margin:0 0 5px;}
    .input {border:1px solid #ddd;border-radius:3px;box-sizing:border-box;display:block;margin:0 0 10px;padding:10px 15px;width:100%;}
    .button {background:#eee;border:1px solid #ddd;border-radius:3px;color:#333;display:inline-block;margin:0 0 10px;padding:10px 15px;text-decoration:none;}
    .button:hover {background:#e8e8e8;}
    

Hãy thay đổi router trong `src/index.js` để thêm vào layout của chúng ta trong mix:
    
    
    // src/index.js
    var m = require("mithril")
    
    var UserList = require("./views/UserList")
    var UserForm = require("./views/UserForm")
    var Layout = require("./views/Layout")
    
    m.route(document.body, "/list", {
        "/list": {
            render: function() {
                return m(Layout, m(UserList))
            }
        },
        "/edit/:id": {
            render: function(vnode) {
                return m(Layout, m(UserForm, vnode.attrs))
            }
        },
    })
    

Chúng ta thay thế mỗi component với một [RouteResolver][17] (về cơ bản, một đối tượng với một phương thức `render`). Phương thức `render` các phương thức có thể được viết theo cách tương tự như view component thông thường bởi lời gọi `m()` .

Điều thú vị cần chú ý là cách các component lời gọi `m()`. Ở đây, trong `/list` route, chúng ta có `m(Layout, m(UserList))`. Điều này có nghĩa là có một vnode gốc đại diện cho một thể hiện của `Layout`, trong đó có một `UserList` vnode là con duy nhất

Trong `/edit/:id` route, cũng có một đối số `vnode` mang theo tham số route trong `UserForm` component. Vì vậy nếu URL là `/edit/1`, sau đó `vnode.attrs` trong trường hợp này là `{id: 1}`, và `m(UserForm, vnode.attrs)`  này tương đương với `m(UserForm, {id: 1})`. code JSX tương đương sẽ là `<UserForm id={vnode.attrs.id} />`.

Làm mới trang trong trình duyệt và bây giờ bạn sẽ thấy điều hướng toàn bộ trên mọi trang trong ứng dụng.


* * *

kết thúc hướng dẫn.

Trong hướng dẫn này, chúng ta đã trải qua quá trình tạo một ứng dụng rất đơn giản, nơi chúng ta có thể liệt kê người dùng từ một máy chủ và chỉnh sửa chúng riêng lẻ. Là một bài tập bổ sung, hãy cố gắng tự tạo và xóa người dùng.

Nếu bạn muốn xem thêm ví dụ về code Mithril, hãy kiểm tra trang ví dụ. Nếu bạn có thắc mắc, vui lòng ghé qua phòng chat Mithril.


* * *

License: MIT. © Leo Horie.

[1]: https://flems.io/#0=N4IgzgpgNhDGAuEAmIBcICGAHLA6AVmCADQgBmAljEagNqgB2GAthGiAKqQBOAsgPZJoBIqVj8GiSewBuGbgAIuERQF4FwADoMFuhVAph4qBbQC6xbXv38MSADKHjCsgFcGCChIAUASg1W1nrcEPCu3DrMuCEAjq4QRt5aOkGprPAAFoImmiAA4gCiACq5limp1uFQOSAZ8PBYYKgA9M0hzAC0IUYd2BS4GSr8ANau2HjizM19za48YKWBFXoA7hSZAMIhQpIUGFBNCvDc8WXLAL6+S0G4mRAM3m4e8F4P3a5Q8P7Jy9bK3LgDEYFOp3p9cEgMPAMNdrJdrucytdYOEQpITMBEdcoLYkCYnp4fBQkN9YcFQuFItEIHEEvAkmS0qEsniFLlCiUSIyglUanUGk1Wu0unTelh+oNuCMxjhcJNpuLZvNmrkFABqBTEs6-XRrTbbe4vfaHY6nbnw8o3O4PAkvHxgr4BS3Lf5y1GGkEKB3mq6WrEMa5gDAyCD49yEh6k53ksIRBRRWLxRI-HXx5nZNkgAAKHE52p1vMz-MaLTaEE63XgYolQ1G4zl-CmMzmKjAKpA6qUPDd3DR8FwWu51kh0JMrpRvcN+d+eoyW2Qhr2BxMpog07hvrh2nOIERjBYbHQ-0cRhEJBA4kkhtk8i7KhP8E9Kd0EgoDHWY+7OLsD+nMgoEArGGzyvH4TrLCEsaRN4uS4C23AdEC8ClHeAJIbgzDYI84Z2g88FRqmXoUnGzAwZgcE8IhTgdOs5YocAGQhGQNTNMg6ztp28EDkgxAKBIsAhFCobxtE-CuIggJvsMiIKFxlDcEYAByB6dqqqoalxUAYEpB6bhUlx6bo5zbruxD7qw7D-AAYvw3BRIQ56XlI8A3oo1m2cwT7XK+77OLaoEyAwggQN8rrfkg3iBcFuBQscYDcb4-rWP+gHARGYHPkEkGUvGZFkB59FDhUEhgK4ABGzAfi4OGgSF4GERUEC4FgIQhpIAAiEBkBgHz0oZDV-N2QYhn4RWpMZ0bjbxtBjbluRaWVwgLdAKG5FZFAKY+TCsLkvjrhUpG5G+WDiQODAnfAtDwAAnlgECqIgAAe8BmLQWBabAEBZFAQjcKo62bQo20QGYhWTb8PkXSYUSzgAgvU3BkXIUDxCh-k+Mj8Shd2E59rg8k6awnqYxAlz7TqJOfioPZ4wT8DKTt4MbuTQSHSAy1QICGCLVAq0gPY2lbQeu0s9YbPHadEuXe9GCfd9v2qALwLA6DJD1QNkPidDuBwwjSP7Kjavow8JPY9TuOGlzhMQMTBuk3ts3JXbVMAhbkhW-TwtM3oZOzWzZXifAEi4AH9QSFdt33aVFXrKrvG5AAysGEAi9yZj9RNO57iAwPsAL11if2DliBIzmuQo+eF15lopUB1UgRjQVCARFTZSRZGYW+XMF+JKEzd7uhs0wMgYfcrh947ehszCauZQNjFdSYADkzRIUvosQx4gmINrUriU1BgMMMk9GfHnDzLts3pxvg9kZAEYoVFQhyhkVBIGi-XWOnCImdnufoPWYuF5S7XnQAmQuTUWpdQoI9bwS8ADES9fTgP3t4JA-AUSsHdmVQQ10z6rycGDawuQCFGFyBibkaJfppVwhlWabdoKV3ErxUix4nC+E-j7BE04SFsXgM0VAxJyHqyyvcah9d0pPzqnPVuxFGEYB7vAFh3h3J2V4lImKCMwAcPNNw7cvhTLmUPCAOUYBRDAKvNIdAOCkB4LOhdYgIdA4SA0PldEQU7L7AUAARgAGxYEegoAAaioSETAADcmFuAAHM3wmAAEwAAYAkTW0N3KuwAomxIYKgbxyTAk9SDpEjAj0OhrCQJkXJiTqkBPCRNUeDBXAaCyXExJCg2kAGZ8l1O0Gk+CVFgTACQh0Iw10YCoCCgwCAxSYmtPaT47pWA7BIDfNE1AiSekMAoioAZVZaKeWAGVWWwxol7wYHieB3UrkYHCTg7g1DvEBIUGAfgBgkAKHgUgL54TxA4m4KgeBHSgXhJWWAGW11UBlRxLAYYMzsnrPmY8x64SllfNWagAAHE87xABWWpT0qxCHENwKErwJkSGmfU-pwz9moCyCGRQwACUdCJbZUlEhUDuF+ofSlvStkcw0KC8FkLoWwpaTktpbS8XIvqVLDQdyHlPJeW8j5XykC3Nsr9LodgKBzFQB02pODSlgAoAAL3RQqnZRqQWGGFVCjBYr5DwslQs2pqKVkMDWXk7F0rwnlMqXkxJABSTZTiw46EOcc05YlzkAogPGjV9yVC5KVa84kqrvmWoQiSlZeqDXIt+bZAFQKOk2rBVpCFb4eUdHtTCuFcy2neuRe69FTafG+uZaykluFyVTNDaHIOOT6UqHlVGs5FyIAYsnZOupu4LDsykjQegOcDzsEqpkbgVBzxVHYMWQUsxzonIbFMddjEqAAAFvG4Cvb45op7N2cyATdO67AHLnDMOcIAA
[2]: https://nodejs.org/en/
[3]: https://mithril.js.org/installation.html
[4]: http://rem-rest-api.herokuapp.com/
[5]: https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods
[6]: https://mithril.js.org/hyperscript.html
[7]: https://mithril.js.org/es6.html
[8]: https://mithril.js.org/jsx.html
[9]: http://eslint.org/
[10]: https://github.com/mishoo/UglifyJS2
[11]: https://github.com/gotwarlost/istanbul
[12]: https://flowtype.org/
[13]: https://mithril.js.org/lifecycle-methods.html
[14]: http://getbootstrap.com/
[15]: http://tachyons.io/
[16]: https://mithril.js.org/route.html#mrouteprefix
[17]: https://mithril.js.org/route.html#routeresolver
[18]: https://mithril.js.org/examples.html
[19]: https://gitter.im/MithrilJS/mithril.js

  
