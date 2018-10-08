# Khởi tạo một ứng dụng đơn giản

Hãy phát triển một ứng dụng đơn giản bao gồm một số đặc điểm chính của Single Page Applications.

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

Dòng `<!doctype html>` cho biết đây là một HTML 5 document. Thẻ meta `charset` cho biết encodeing của document và thẻ `viewport` đưa ra cách trình duyệt mobile nên mở rộng page. Thẻ `title` chứa đoạn tiêu đề hiển thị trên tab trình duyệt cho ứng dụng này, và thẻ `script` cho biết đường dẫn nào của file Javascript để ứng dụng sử dụng.

Chúng ta nên tạo toàn bộ ứng dụng trong một file Javascript duy nhất, nhưng làm như vậy sẽ gây khó khăn cho việc điều hướng codebase sau này. Thay vào đó, hãy chia code vào các module và tập hợp các module đó vào một bundle `bin/app.js`.

Có nhiều cách để cài đặt công cụ quản lý code, nhưng hầu hết được phân phối qua NPM. Trong thực tế, hầu hết các thư viện và công cụ Javascript hiện đại được phân phối qua cách đó, bao gồm cả Mithril. NPM viết tắt cho Node.js Package Manager. Để download NPM, cài đặt Node.js; NPM được cài tự động với nó. Khi bạn có Node.js và NPM được cài đặt, mở command line và chạy câu lệnh này: 

```
npm init -y
```
Nếu NPM được cài đúng, một file `package.json` sẽ được cài đặt. File này sẽ chưa một bộ mô tả file của dự án. Cảm thấy tự do để chỉnh sửa project và thông tin tác giả trong file này.

Để cài đặt Mithril, làm theo hướng dẫn trong trang installtion. Khi bạn có một project cơ bản với Mithril được cài đặt, chúng ta sẵn sàng để tạo ứng dụng. 

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
Tùy chọn `method` là một HTTP method. Để nhận dữ liệu từ server mà không gây ra các ngoại lệ lên server, chúng ta cần sử dụng phương thức `GET`. `url` là địa chỉ cho API endpoint. Dòng `withCredentials: true` cho biết chúng ta đang sử dụng cookies (đó là một yêu cầu cho REM API).

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

Theo mặc định, Mithril view được mô tả sử dụng [hyperscript](https://mithril.js.org/hyperscript.html). Hyperscript cung cấp một cú pháp vắn tắt mà có thể chèn HTML vào một cách tự nhiên hơn cho các thẻ phức tạp, và ngoài ra, từ cú pháp đơn giản là Javascript, có thể tận dụng nhiều hệ sinh thái các công cụ của Javascript, ví dụ [Babel][7], [JSX][8] (inline-HTML syntax extension), [eslint][9] (linting), [uglifyjs][10] (minification), [istanbul][11] (code coverage), [flow][12] (static type analysis), ....

Hãy sử dụng Mithril hyperscript để tạo một danh sách các item. Hyperscript là ngôn ngữ phổ biến cách viết Mithril views, nhưng [JSX là một lựa chọn phổ biế khác bạn có thể thử dùng][8] khi bạn thấy thoải mãi hơn với những thứ cơ bản:
    
    
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
    

Vì `User.list` là một mảng trên Javascript, và hyperscript views cũng là Javascript, chúng ta có thể lặp thông qua mảng bằng cách sử dụng phương thức `.map`. Điều này tạo ra một mảng các vnodes đại diện cho một danh sách các thẻ `div`, mỗi trên chứa một user.

Có vấn đề, tất nhiên rồi, chúng ta chưa bao giờ gọi hàm User.loadList. Vì thế, User.list vẫn là một array rỗng, và view này chỉ trả về một trang rỗng. Vì chúng ta muốn User.loadList được gọi khi chúng ta render component, chúng ta có thể tận dụng được ưu điểm của vòng đời của các phương thức:
    
    
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
    

Notice that we added an `oninit` method to the component, which references `User.loadList`. This means that when the component initializes, User.loadList will be called, triggering an XHR request. When the server returns a response, `User.list` gets populated.

Also notice we **didn't** do `oninit: User.loadList()` (with parentheses at the end). The difference is that `oninit: User.loadList()` calls the function once and immediately, but `oninit: User.loadList` only calls that function when the component renders. This is an important difference and a common pitfall for developers new to javascript: calling the function immediately means that the XHR request will fire as soon as the source code is evaluated, even if the component never renders. Also, if the component is ever recreated (through navigating back and forth through the application), the function won't be called again as expected.
* * *

Let's render the view from the entry point file `src/index.js` we created earlier:
    
    
    // src/index.js
    var m = require("mithril")
    
    var UserList = require("./views/UserList")
    
    m.mount(document.body, UserList)
    

The `m.mount` call renders the specified component (`UserList`) into a DOM element (`document.body`), erasing any DOM that was there previously. Opening the HTML file in a browser should now display a list of person names.

* * *

Right now, the list looks rather plain because we have not defined any styles.

There are many similar conventions and libraries that help organize application styles nowadays. Some, like [Bootstrap][14] dictate a specific set of HTML structures and semantically meaningful class names, which has the upside of providing low cognitive dissonance, but the downside of making customization more difficult. Others, like [Tachyons][15] provide a large number of self-describing, atomic class names at the cost of making the class names themselves non-semantic. "CSS-in-JS" is another type of CSS system that is growing in popularity, which basically consists of scoping CSS via transpilation tooling. CSS-in-JS libraries achieve maintainability by reducing the size of the problem space, but come at the cost of having high complexity.

Regardless of what CSS convention/library you choose, a good rule of thumb is to avoid the cascading aspect of CSS. To keep this tutorial simple, we'll just use plain CSS with overly explicit class names, so that the styles themselves provide the atomicity of Tachyons, and class name collisions are made unlikely through the verbosity of the class names. Plain CSS can be sufficient for low-complexity projects (e.g. 3 to 6 man-months of initial implementation time and few project phases).

To add styles, let's first create a file called `styles.css` and include it in the `index.html` file:
    
    
    
    
        
            
            
            
            https://mithril.js.org/styles.css" rel="stylesheet">
        
        
            ">https://mithril.js.org/bin/app.js">
        
    
    

Now we can style the `UserList` component:
    
    
    .user-list {list-style:none;margin:0 0 10px;padding:0;}
    .user-list-item {background:#fafafa;border:1px solid #ddd;color:#333;display:block;margin:0 0 1px;padding:8px 15px;text-decoration:none;}
    .user-list-item:hover {text-decoration:underline;}
    

The CSS above is written using a convention of keeping all styles for a rule in a single line, in alphabetical order. This convention is designed to take maximum advantage of screen real estate, and makes it easier to scan the CSS selectors (since they are always on the left side) and their logical grouping, and it enforces predictable and uniform placement of CSS rules for each selector.

Obviously you can use whatever spacing/indentation convention you prefer. The example above is just an illustration of a not-so-widespread convention that has strong rationales behind it, but deviate from the more widespread cosmetic-oriented spacing conventions.

Reloading the browser window now should display some styled elements.

* * *

Let's add routing to our application.

Routing means binding a screen to a unique URL, to create the ability to go from one "page" to another. Mithril is designed for Single Page Applications, so these "pages" aren't necessarily different HTML files in the traditional sense of the word. Instead, routing in Single Page Applications retains the same HTML file throughout its lifetime, but changes the state of the application via Javascript. Client side routing has the benefit of avoiding flashes of blank screen between page transitions, and can reduce the amount of data being sent down from the server when used in conjunction with an web service oriented architecture (i.e. an application that downloads data as JSON instead of downloading pre-rendered chunks of verbose HTML).

We can add routing by changing the `m.mount` call to a `m.route` call:
    
    
    // src/index.js
    var m = require("mithril")
    
    var UserList = require("./views/UserList")
    
    m.route(document.body, "/list", {
        "/list": UserList
    })
    

The `m.route` call specifies that the application will be rendered into `document.body`. The `"/list"` argument is the default route. That means the user will be redirected to that route if they land in a route that does not exist. The `{"/list": UserList}` object declares a map of existing routes, and what components each route resolves to.

Refreshing the page in the browser should now append `#!/list` to the URL to indicate that routing is working. Since that route render UserList, we should still see the list of people on screen as before.

The `#!` snippet is known as a hashbang, and it's a commonly used string for implementing client-side routing. It's possible to configure this string it via [`m.route.prefix`][16]. Some configurations require supporting server-side changes, so we'll just continue using the hashbang for the rest of this tutorial.

* * *

Let's add another route to our application for editing users. First let's create a module called `views/UserForm.js`
    
    
    // src/views/UserForm.js
    
    module.exports = {
        view: function() {
            // TODO implement view
        }
    }
    

Then we can `require` this new module from `src/index.js`
    
    
    // src/index.js
    var m = require("mithril")
    
    var UserList = require("./views/UserList")
    var UserForm = require("./views/UserForm")
    
    m.route(document.body, "/list", {
        "/list": UserList
    })
    

And finally, we can create a route that references it:
    
    
    // src/index.js
    var m = require("mithril")
    
    var UserList = require("./views/UserList")
    var UserForm = require("./views/UserForm")
    
    m.route(document.body, "/list", {
        "/list": UserList,
        "/edit/:id": UserForm,
    })
    

Notice that the new route has a `:id` in it. This is a route parameter; you can think of it as a wild card; the route `/edit/1` would resolve to `UserForm` with an `id` of `"1"`. `/edit/2` would also resolve to `UserForm`, but with an `id` of `"2"`. And so on.

Let's implement the `UserForm` component so that it can respond to those route parameters:
    
    
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
    

And let's add some styles to `styles.css`:
    
    
    /* styles.css */
    body,.input,.button {font:normal 16px Verdana;margin:0;}
    
    .user-list {list-style:none;margin:0 0 10px;padding:0;}
    .user-list-item {background:#fafafa;border:1px solid #ddd;color:#333;display:block;margin:0 0 1px;padding:8px 15px;text-decoration:none;}
    .user-list-item:hover {text-decoration:underline;}
    
    .label {display:block;margin:0 0 5px;}
    .input {border:1px solid #ddd;border-radius:3px;box-sizing:border-box;display:block;margin:0 0 10px;padding:10px 15px;width:100%;}
    .button {background:#eee;border:1px solid #ddd;border-radius:3px;color:#333;display:inline-block;margin:0 0 10px;padding:10px 15px;text-decoration:none;}
    .button:hover {background:#e8e8e8;}
    

Right now, this component does nothing to respond to user events. Let's add some code to our `User` model in `src/models/User.js`. This is how the code is right now:
    
    
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
    

Let's add code to allow us to load a single user
    
    
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
    

Notice we added a `User.current` property, and a `User.load(id)` method which populates that property. We can now populate the `UserForm` view using this new method:
    
    
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
    

Similar to the `UserList` component, `oninit` calls `User.load()`. Remember we had a route parameter called `:id` on the `"/edit/:id": UserForm` route? The route parameter becomes an attribute of the `UserForm` component's vnode, so routing to `/edit/1` would make `vnode.attrs.id` have a value of `"1"`.

Now, let's modify the `UserList` view so that we can navigate from there to a `UserForm`:
    
    
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
    

Here we changed `.user-list-item` to `a.user-list-item`. We added an `href` that references the route we want, and finally we added `oncreate: m.route.link`. This makes the link behave like a routed link (as opposed to merely behaving like a regular link). What this means is that clicking the link would change the part of URL that comes after the hashbang `#!` (thus changing the route without unloading the current HTML page)

If you refresh the page in the browser, you should now be able to click on a person and be taken to a form. You should also be able to press the back button in the browser to go back from the form to the list of people.

* * *

The form itself still doesn't save when you press "Save". Let's make this form work:
    
    
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
    

We added `oninput` events to both inputs, that set the `User.current.firstName` and `User.current.lastName` properties when a user types.

In addition, we declared that a `User.save` method should be called when the "Save" button is pressed. Let's implement that method:
    
    
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
    

In the `save` method at the bottom, we used the `PUT` HTTP method to indicate that we are upserting data to the server.

Now try editing the name of a user in the application. Once you save a change, you should be able to see the change reflected in the list of users.

* * *

Currently, we're only able to navigate back to the user list via the browser back button. Ideally, we would like to have a menu - or more generically, a layout where we can put global UI elements

Let's create a file `src/views/Layout.js`:
    
    
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
    

This component is fairly straightforward, it has a `
` with a link to the list of users. Similar to what we did to the `/edit` links, this link uses `m.route.link` to activate routing behavior in the link.

Notice there's also a `
` element with `vnode.children` as children. `vnode` is a reference to the vnode that represents an instance of the Layout component (i.e. the vnode returned by a `m(Layout)` call). Therefore, `vnode.children` refer to any children of that vnode.

Let's add some styles:
    
    
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
    

Let's change the router in `src/index.js` to add our layout into the mix:
    
    
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
    

We replaced each component with a [RouteResolver][17] (basically, an object with a `render` method). The `render` methods can be written in the same way as regular component views would be, by nesting `m()` calls.

The interesting thing to pay attention to is how components can be used instead of a selector string in a `m()` call. Here, in the `/list` route, we have `m(Layout, m(UserList))`. This means there's a root vnode that represents an instance of `Layout`, which has a `UserList` vnode as its only child.

In the `/edit/:id` route, there's also a `vnode` argument that carries the route parameters into the `UserForm` component. So if the URL is `/edit/1`, then `vnode.attrs` in this case is `{id: 1}`, and this `m(UserForm, vnode.attrs)` is equivalent to `m(UserForm, {id: 1})`. The equivalent JSX code would be ``.

Refresh the page in the browser and now you'll see the global navigation on every page in the app.

* * *

This concludes the tutorial.

In this tutorial, we went through the process of creating a very simple application where we can list users from a server and edit them individually. As an extra exercise, try to implement user creation and deletion on your own.

If you want to see more examples of Mithril code, check the [examples][18] page. If you have questions, feel free to drop by the [Mithril chat room][19].

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

  
