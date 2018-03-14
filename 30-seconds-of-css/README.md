# 30 Seconds duyệtS

![logo](https://i.imgur.com/2L1bMyy.png)

[![License](https://img.shields.io/badge/license-CC0--1.0-blue.svg)](https://github.com/atomiks/30-seconds-of-css/blob/master/LICENSE) [![Gitter chat](https://img.shields.io/badge/chat-on%20gitter-4FB999.svg)](https://gitter.im/30-seconds-of-css/Lobby) [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com) [![Insight.io](https://img.shields.io/badge/insight.io-Ready-brightgreen.svg)](https://insight.io/github.com/atomiks/30-seconds-of-css/tree/master/?source=0)

A curated collection of useful CSS snippets you can understand in 30 seconds or less.
Inspired by [30 seconds of code](https://github.com/Chalarangelo/30-seconds-of-code).

## View online

https://atomiks.github.io/30-seconds-of-css/

## Contributing

See CONTRIBUTING.md for the snippet template.

### Nảy tải trang

`Creates a bouncing loader animation.`
`Tạo một hiệu ứng nảy khi tải trang`

Tạo hiệu ứng nảy khi tải trang.
#### HTML

```html
<div class="bouncing-loader">
  <div></div>
  <div></div>
  <div></div>
</div>
```

#### CSS

```css
@keyframes bouncing-loader {
  from {
    opacity: 1;
    transform: translateY(0);
  }
  to {
    opacity: 0.1;
    transform: translateY(-1rem);
  }
}
.bouncing-loader {
  display: flex;
  justify-content: center;
}
.bouncing-loader > div {
  width: 1rem;
  height: 1rem;
  margin: 3rem 0.2rem;
  background: #8385aa;
  border-radius: 50%;
  animation: bouncing-loader 0.6s infinite alternate;
}
.bouncing-loader > div:nth-child(2) {
  animation-delay: 0.2s;
}
.bouncing-loader > div:nth-child(3) {
  animation-delay: 0.4s;
}
```

#### Demo

<div class="snippet-demo">
  <div class="snippet-demo__bouncing-loader">
    <div></div>
    <div></div>
    <div></div>
  </div>
</div>

<style>
@keyframes bouncing-loader {
  from {
    opacity: 1;
    transform: translateY(0);
  }
  to {
    opacity: 0.1;
    transform: translateY(-1rem);
  }
}
.snippet-demo__bouncing-loader {
  display: flex;
  justify-content: center;
}
.snippet-demo__bouncing-loader > div {
  width: 1rem;
  height: 1rem;
  margin: 3rem 0.2rem;
  background: #8385aa;
  border-radius: 50%;
  animation: bouncing-loader 0.6s infinite alternate;
}
.snippet-demo__bouncing-loader > div:nth-child(2) {
  animation-delay: 0.2s;
}
.snippet-demo__bouncing-loader > div:nth-child(3) {
  animation-delay: 0.4s;
}
</style>

#### Giải thích

Chú ý: `1rem` thường là `16px`.

1. `@keyframes` để định nghĩa một hiệu ứng có 2 trạng thái, khi thành phần thay đổi `opacity` và nó được dịch chuyển lên trong mặt phẳng 2D thì sử dụng `transform: translateY()`.

2. `.bouncing-loader` là khối cha chứa các hình tròn nảy lên  và sử dụng các thuộc tính`display: flex` và `justify-content: center` để đặt chúng vào giữa.

3. `.bouncing-loader > div`, trỏ vào 3 thẻ `div` con của thẻ cha để định dạng. Những thẻ `div` được truyền vào `1rem` cho chiều rộng và chiều cao, sử dụng `border-radius: 50%` để biến chúng từ những hình vuông thành những hình tròn.

4. `margin: 3rem 0.2rem` xác định cho mỗi hình tròn căn lề trên/dưới `3rem` và căn lề trái/phải `0.2rem` để chúng không chạm trực tiếp vào nhau, cho chúng một vài khoảng trống.

5. `animation` là một thuộc tính nhanh với các thuộc hiệu ứng khác nhau: `animation-name`, `animation-duration`, `animation-iteration-count`, `animation-direction` đã được sử dụng.

6. `nth-child(n)` trỏ vào thành phần là con thứ n của thành phần cha.

7. `animation-delay` được dùng với 2s và tương ứng với thẻ `div` thứ 3, để các thành phần không bắt đầu hiệu ứng cùng một lúc.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">✅ No caveats.</span>

* https://caniuse.com/#feat=css-animation

<!-- tags: animation -->
### Box-sizing reset

Reset các mô hình dạng hộp với `width` và `height` không ảnh hưởng bởi các thẻ `border`s hoặc `padding` của chúng.

#### CSS

```css
html {
  box-sizing: border-box;
}

*,
*::before,
*::after {
  box-sizing: inherit;
}
```

#### Demo

<div class="snippet-demo">
  <div class="snippet-demo__box-sizing-reset">Demo</div>
</div>

<style>
.snippet-demo__box-sizing-reset {
  box-sizing: border-box;
  width: 200px;
  padding: 1.5em;
  color: #7983ff;
  font-family: sans-serif;
  background-color: white;
  border: 5px solid;
}
</style>

#### Giải thích

1. `box-sizing: border-box` tạo thêm thuộc tính `padding` hoặc `border` không ảnh hưởng tới `width` or `height` của thành phần.
2. `box-sizing: inherit` làm cho một thành phần tuần theo quy tắc của thuộc tính cha `box-sizing`.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">✅ No caveats.</span>

* https://caniuse.com/#feat=css3-boxsizing

<!-- tags: layout -->
### Clearfix

Đảm bảo rằng một thành phần làm sạch các thành phần con của chính mình.

###### Chú ý: Nó chỉ được sử dụng nếu bạn vẫn còn sử dụng thuộc tính float trong xây dựng bố cục. Hãy xem xét sử dụng cách tiếp cận hiện đại với bố cục flexbox hoặc lưới.

#### HTML

```html
<div class="clearfix">
  <div class="floated">float a</div>
  <div class="floated">float b</div>
  <div class="floated">float c</div>
</div>
```

#### CSS

```css
.clearfix::after {
  content: '';
  display: block;
  clear: both;
}

.floated {
  float: left;
}
```

#### Demo

<div class="snippet-demo">
  <div class="snippet-demo__clearfix">
    <div class="snippet-demo__floated">float a</div>
    <div class="snippet-demo__floated">float b</div>
    <div class="snippet-demo__floated">float c</div>
  </div>
</div>

<style>
.snippet-demo__clearfix::after {
  content: '';
  display: block;
  clear: both;
}

.snippet-demo__floated {
  float: left;
}
</style>

#### Giải thích

1. `.clearfix::after` định nghĩa một thành phần giả.
2. `content: ''` cho phép thành phần giả được ảnh hưởng bởi bố cụ.
3. `clear: both` chỉ các thành phần bên trái, phải, cả hay không thể kề nhau đến các thành phần đã lưu trước đó trong cùng một khối.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">⚠️ For this snippet to work properly you need to ensure that there are no non-floating children in the container and that there are no tall floats before the clearfixed container but in the same formatting context (e.g. floated columns).</span>

<!-- tags: layout -->
### Tỷ lệ chiều rộng/chiều cao không thay đổi

Cho một phần tử có chiều rộng thay đổi, nó sẽ đảm bảo chiều cao vẫn tương xứng khi hình dáng thay đổi (tỉ lệ chiều rộng/chiều cao vẫn không đổi).

#### HTML

```html
<div class="constant-width-to-height-ratio"></div>
```

#### CSS

```css
.constant-width-to-height-ratio {
  background: #333;
  width: 50%;
}
.constant-width-to-height-ratio::before {
  content: '';
  display: block;
  padding-top: 100%;
  float: left;
}
.constant-width-to-height-ratio::after {
  content: '';
  display: block;
  clear: both;
}
```

#### Demo

Thay đổi kích thuước cửa sổ trình duyệt của bạn để xem tỉ lệ của phần tử vẫn giống nhau.

<div class="snippet-demo">
  <div class="snippet-demo__constant-width-to-height-ratio"></div>
</div>

<style>
.snippet-demo__constant-width-to-height-ratio {
  background: #333;
  width: 50%;
}
.snippet-demo__constant-width-to-height-ratio::before {
  content: '';
  display: block;
  padding-top: 100%;
  float: left;
}
.snippet-demo__constant-width-to-height-ratio::after {
  content: '';
  display: block;
  clear: both;
}
</style>

#### Giải thích

`padding-top` trên phần tử giả `::before` dẫn đến chiều cao của phần tử bằng một tỷ lệ phần trăm với chiều rộng. Vì `100%` nghĩa là chiều cao của phần tử sẽ luôn luôn là `100%` của chiều rộng, tạo 1 hình vuông responsive.
Phương thức này cũng cho phép phần nội dung được thay thế bên trong phần tử bình thường.


#### Hỗ trợ trình duyệt

<span class="snippet__support-note">✅ No caveats.</span>

<!-- tags: layout -->
### Phân chia đều các phần tử con

Phân chia đều các phần tử con bên trong phần tử cha.

#### HTML

```html
<div class="evenly-distributed-children">
  <p>Item1</p>
  <p>Item2</p>
  <p>Item3</p>
</div>
```

#### CSS

```css
.evenly-distributed-children {
  display: flex;
  justify-content: space-between;
}
```

#### Demo

<div class="snippet-demo">
  <div class="snippet-demo__evenly-distributed-children">
    <p>Item1</p>
    <p>Item2</p>
    <p>Item3</p>
  </div>
</div>

<style>
.snippet-demo__evenly-distributed-children {
  display: flex;
  width: 100%;  
  justify-content: space-between;
}
</style>

#### Giải thích

1. `display: flex` cho phép flexbox.
2. `justify-content: space-between` phần chia đều các phần tử con theo chiều ngang. Phần tử đầu tiên được nằm về rìa bên trái, trong khi phần tử cuối cùng được nằm ở rìa bên phải.

Cách khác, dùng `justify-content: space-around` để chia các phần tử con với không gian xung quan nó, chứ không phải giữa chúng.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">⚠️ Needs prefixes for full support.</span>

* https://caniuse.com/#feat=flexbox

<!-- tags: layout -->
### Flexbox căn giữa

Một phần tử con căn giữa theo chiều ngang và chiều dọc bên trong của phần tử cha sử dụng `flexbox`.

#### HTML

```html
<div class="flexbox-centering">
  <div class="child">Centered content.</div>
</div>
```

#### CSS

```css
.flexbox-centering {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

#### Demo

<div class="snippet-demo">
  <div class="snippet-demo__flexbox-centering">
    <p class="snippet-demo__flexbox-centering__child">Centered content.</p>
  </div>
</div>

<style>
.snippet-demo__flexbox-centering {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 200px;
}
</style>

#### Giải thích

1. `display: flex` cho phép flexbox.
2. `justify-content: center` căn giữa phần tử con theo chiều ngang.
3. `align-items: center` căn giữa phần tử con theo chiều dọc.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">⚠️ Needs prefixes for full support.</span>

* https://caniuse.com/#feat=flexbox

<!-- tags: layout -->
### Căn giữa với lưới

Căn giữa một phần tử con theo chiều ngang và chiều dọc trong phần tử con sử dụng `grid`.

#### HTML

```html
<div class="grid-centering">
  <div class="child">Centered content.</div>
</div>
```

#### CSS

```css
.grid-centering {
  display: grid;
  justify-content: center;
  align-items: center;
}
```

#### Demo

<div class="snippet-demo">
  <div class="snippet-demo__grid-centering">
    <p class="snippet-demo__grid-centering__child">Centered content.</p>
  </div>
</div>

<style>
.snippet-demo__grid-centering {
  display: grid;
  justify-content: center;
  align-items: center;
  height: 200px;
}
</style>

#### Giải thích

1. `display: grid` cho phép dạng lưới.
2. `justify-content: center` căn giưaax phần tử con theo chiều ngang.
3. `align-items: center` căn giữa phần tử con theo chiều dọc.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">✅ No caveats.</span>

* https://caniuse.com/#feat=css-grid

<!-- tags: layout -->
### Bố dục dạng lưới

Bố cục một website cơ bản sử dụng `grid`.

#### HTML

```html
<div class="grid-layout">
  <div class="header">Header</div>
  <div class="sidebar">Sidebar</div>
  <div class="content">
    Content
    <br>
    Lorem ipsum dolor sit amet, consectetur adipisicing elit.
  </div>
  <div class="footer">Footer</div>
</div>
```

#### CSS

```css
.grid-layout {
  display: grid;
  grid-gap: 10px;
  grid-template-columns: repeat(3, 1fr);
  grid-template-areas:
    'sidebar header header'
    'sidebar content content'
    'sidebar footer  footer';
  color: white;
}
.grid-layout > div {
  background: #333;
  padding: 10px;
}
.sidebar {
  grid-area: sidebar;
}
.content {
  grid-area: content;
}
.header {
  grid-area: header;
}
.footer {
  grid-area: footer;
}
```

#### Demo

<div class="snippet-demo">
  <div class="snippet-demo__grid-layout">
    <div class="box snippet-demo__grid-layout__header">Header</div>
    <div class="box snippet-demo__grid-layout__sidebar">Sidebar</div>
    <div class="box snippet-demo__grid-layout__content">Content
      <br> Lorem ipsum dolor sit amet, consectetur adipisicing elit.
    </div>
    <div class="box snippet-demo__grid-layout__footer">Footer</div>
  </div>
</div>

<style>
.snippet-demo__grid-layout {
  margin: 1em;
  display: grid;
  grid-gap: 10px;
  grid-template-columns: repeat(3, 1fr);
  grid-template-areas:
    "sidebar header header"
    "sidebar content content"
    "sidebar  footer  footer";
  background-color: #fff;
  color: white;
}
.snippet-demo__grid-layout > div {
  background: #333;
  padding: 10px;
}
.snippet-demo__grid-layout__sidebar {
    grid-area: sidebar;
}
.snippet-demo__grid-layout__content {
    grid-area: content;
}
.snippet-demo__grid-layout__header {
    grid-area: header;
}
.snippet-demo__grid-layout__footer {
    grid-area: footer;
}
</style>

#### Giải thích

1. `display: grid` cho phép dạng lưới.
2. `grid-gap: 10px` xác định khoảng không giữa các phần tử.
3. `grid-template-columns: repeat(3, 1fr)` xác định kích thuước giống nhau của 3 cột.
4. `grid-template-areas` xác định tên của khu vực lưới.
5. `grid-area: sidebar` tạo phần tử dùng khu vực với tên `sidebar`.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">✅ No caveats.</span>

* https://caniuse.com/#feat=css-grid

<!-- tags: layout -->
### Loại bỏ văn bản

Nếu đoạn văn bản dài quá 1 dòng, nó sẽ được loại bỏ và thêm vào cuối dấu chấm lửng  `…`.

#### HTML

```html
<p class="truncate-text">If I exceed one line's width, I will be truncated.</p>
```

#### CSS

```css
.truncate-text {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  width: 200px;
}
```

#### Demo

<div class="snippet-demo">
  <p class="snippet-demo__truncate-text">
    This text will be truncated if it exceeds 200px in width.
  </p>
</div>

<style>
.snippet-demo__truncate-text {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  width: 200px;
  margin: 0;
}
</style>

#### Giải thích

1. `overflow: hidden` ngăn ngừa đoạn văn bản tràn ra quá kích thước của nó (một khó, 100% chiều rộng và chiều cao tự động).
2. `white-space: nowrap` ngăn ngừa văn bản vượt quá 1 dòng theo chiều cao.
3. `text-overflow: ellipsis` thực hiện nếu văn bản vượt quá kích thuước của nó, nó sẽ kết thúc một dấu chấm lửng.
4. `width: 200px;` đảm bảo phần tử có một kích thuước, để biết khi nào thì lấy dấu chấm lửng.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">⚠️ Chỉ làm việc với các phần tử 1 dòng đơn.</span>

* https://caniuse.com/#feat=text-overflow

<!-- tags: layout -->
### Hình tròn

Tạo một hình tròn với chỉ CSS.

#### HTML

```html
<div class="circle"></div>
```

#### CSS

```css
.circle {
  border-radius: 50%;
  width: 2rem;
  height: 2rem;
  background: #333;
}
```

#### Demo

<div class="snippet-demo">
  <div class="snippet-demo__circle"></div>
</div>

<style>
.snippet-demo__circle {
  border-radius: 50%;
  width: 2rem;
  height: 2rem;
  background: #333;
}
</style>

#### Explanation

`border-radius: 50%` uống cong đường viền của thành phần để tạo ra một hình tròn.

Để một hình tròn có một bán kính giống nhau tại bất kỳ điểm nào, thuộc tính `width` và `height` phải giống nhau. Giá trị khác nhau sẽ tạo ra một hình ellipse.

#### Browser support

<span class="snippet__support-note">✅ No caveats.</span>

* https://caniuse.com/#feat=border-radius


<!-- tags: visual -->
### Tuỳ biến thanh cuộn

Tuỳ biến định dạng của thanh cuộn cho tài liệu và các phần từ với scrollable overflow, trên WebKit platforms.

#### HTML

```html
<div class="custom-scrollbar">
  <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Iure id exercitationem nulla qui repellat laborum vitae, molestias tempora velit natus. Quas, assumenda nisi. Quisquam enim qui iure, consequatur velit sit?</p>
</div>
```

#### CSS

```css
/* Document scrollbar */
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
  border-radius: 10px;
}

::-webkit-scrollbar-thumb {
  border-radius: 10px;
  box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.5);
}

/* Scrollable element */
.some-element::webkit-scrollbar {
}
```

#### Demo

<div class="snippet-demo">
  <div class="snippet-demo__custom-scrollbar">
    <p>
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Iure id exercitationem nulla qui repellat laborum vitae, molestias tempora velit natus. Quas, assumenda nisi. Quisquam enim qui iure, consequatur velit sit?
    </p>
    <p>
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Iure id exercitationem nulla qui repellat laborum vitae, molestias tempora velit natus. Quas, assumenda nisi. Quisquam enim qui iure, consequatur velit sit?
    </p>
  </div>
</div>

<style>
.snippet-demo__custom-scrollbar {
  height: 100px;
  overflow: auto;
}

.snippet-demo__custom-scrollbar::-webkit-scrollbar {
  width: 8px;
}

.snippet-demo__custom-scrollbar::-webkit-scrollbar-track {
  box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
  border-radius: 10px;
}

.snippet-demo__custom-scrollbar::-webkit-scrollbar-thumb {
  border-radius: 10px;
  box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.5);
}
</style>

#### Giải thích

1. `::-webkit-scrollbar` trỏ đến tất cả phần tử thanh cuộn.
2. `::-webkit-scrollbar-track` trỏ đến chỉ thanh cuộn theo dõi.
3. `::-webkit-scrollbar-thumb` trỏ đến thanh cuộc chính.

Có rất nhiều phần tử pseudo khác mà bạn có thể dụng để định dạng cho thanh cuộn. Thêm thông tin, ghé thăm [WebKit Blog](https://webkit.org/blog/363/styling-scrollbars/)

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">⚠️ Kiểu dáng của thanh cuộc không xuất hiện trong bất kỳ chuẩn nào.</span>

* https://caniuse.com/#feat=css-scrollbar

<!-- tags: visual -->
### Tuỳ biến văn bản trong selection

Thay đổi định dạng của văn bản trong selection.

#### HTML

```html
<p class="custom-text-selection">Select some of this text.</p>
```

#### CSS

```css
::selection {
  background: aquamarine;
  color: black;
}
.custom-text-selection::selection {
  background: deeppink;
  color: white;
}
```

#### Demo

<div class="snippet-demo">
  <p class="snippet-demo__custom-text-selection">Select some of this text.</p>
</div>

<style>
.snippet-demo__custom-text-selection::selection {
  background: deeppink;
  color: white;
}
.snippet-demo__custom-text-selection::-moz-selection {
  background: deeppink;
  color: white;
}
</style>

#### Giải thích

`::selection` định nghĩa một pseudo selector trên một phần tử để định dạng văn bản trong nó khi được chọn. Chú ý rằng nếu bạn không phối hợp với định dạng của selector khác của bạn sẽ được áp dụng với mức độ cao nhất của tài liệu, đến bất kỳ thành phần nào được chọn.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">⚠️ Requires prefixes for full support and is not actually
in any specification.</span>

* https://caniuse.com/#feat=css-selection

<!-- tags: visual -->
### Đổ bóng động

Tạo một đổ bóng tương tự `box-shadow` nhưng dựa trên các màu ủa chính phần tử đó.

#### HTML

```html
<div class="dynamic-shadow-parent">
  <div class="dynamic-shadow"></div>
</div>
```

#### CSS

```css
.dynamic-shadow-parent {
  position: relative;
  z-index: 1;
}
.dynamic-shadow {
  position: relative;
  width: 10rem;
  height: 10rem;
  background: linear-gradient(75deg, #6d78ff, #00ffb8);
}
.dynamic-shadow::after {
  content: '';
  width: 100%;
  height: 100%;
  position: absolute;
  background: inherit;
  top: 0.5rem;
  filter: blur(0.4rem);
  opacity: 0.7;
  z-index: -1;
}
```

#### Demo

<div class="snippet-demo">
  <div class="snippet-demo__dynamic-shadow-parent">
    <div class="snippet-demo__dynamic-shadow"></div>
  </div>
</div>

<style>
.snippet-demo__dynamic-shadow-parent {
  position: relative;
  z-index: 1;
}
.snippet-demo__dynamic-shadow {
  position: relative;
  width: 10rem;
  height: 10rem;
  background: linear-gradient(75deg, #6d78ff, #00ffb8);
}
.snippet-demo__dynamic-shadow::after {
  content: '';
  position: absolute;
  width: 100%;
  height: 100%;
  background: inherit;
  top: 0.5rem;
  filter: blur(0.4rem);
  opacity: 0.7;
  z-index: -1;
}
</style>

#### Giải thích

Đoạn mã yêu cầu một trường hợp khá phức tạp của ngữ cảnh của các ngữ cảnh được sắp xếp đúng. Sao cho phần tử pseudo sẽ được đặt bên dưới chính phần tử đó trong khi vẫn nhìn thấy được.

1. `position: relative` trên phần tử cha khởi tạo một vị trí theo ngữ cảnh cho các phần tử con.
2. `z-index: 1` khởi tạo một ngữ cảnh xếp chồng mới.
3. `position: relative` trên phần tử con khởi tạo một ví trí theo ngữ cảnh cho các phần tử pseudo.
4. `::after` định nghĩa một phần tử pseudo.
5. `position: absolute` đưa phần tử pseudo ra khỏi luồng của tài liệu và đặt nó ở vị trí có liên quan với phần tử cha.
6. `width: 100%` và `height: 100%` kích thước phần tử pseudo để lấp đầy kích thước của phần tử cha, tạo nó bằng nhau về kích thước.
7. `background: inherit` làm cho phần tử pseudo kế thừa đường gradient xác định trên phần tử.
8. `top: 0.5rem` khoảng cách phần tử pseudo xuống dứoi tính từ phần tử cha.
9. `filter: blur(0.4rem)` sẽ làm mờ phần tử pseudo để tạo bề ngoài của bóng bên dưới.
10. `opacity: 0.7` làm một phần của phần tử pseudo trở lên trong suốt.
11. `z-index: -1` đặt phần tử pseudo bên dưới của phần tử cha.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">⚠️ Requires prefixes for full support.</span>

* https://caniuse.com/#feat=css-filters

<!-- tags: visual -->
### Etched text

Tạo một hiệu ứng nơi mà văn bản xuất hiện được "etched" or engraved vào nền.

#### HTML

```html
<p class="etched-text">I appear etched into the background.</p>
```

#### CSS

```css
.etched-text {
  text-shadow: 0 2px white;
  font-size: 1.5rem;
  font-weight: bold;
  color: #b8bec5;
}
```

#### Demo

<div class="snippet-demo">
  <p class="snippet-demo__etched-text">I appear etched into the background.</p>
</div>

<style>
.snippet-demo__etched-text {
  font-size: 1.5rem;
  font-weight: bold;
  color: #b8bec5;
  text-shadow: 0 2px 0 white;
}
</style>

#### Giải thích

`text-shadow: 0 2px white` tạo một đổ bóng màu trắng với khoảng cách `0px` theo chiều ngang và `2px` theo chiều dọc tính từ vị trí gốc.
Hình nền phải tối hơn bóng đổ cho hiệu ứng làm việc,
Màu văn bản nên hơi nhạt để làm cho nó giống như it's engraved/carved out của hình nền.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">✅ No caveats.</span>

* https://caniuse.com/#feat=css-textshadow

<!-- tags: visual -->
### Văn bản dạng Gradient

Cho văn bản một màu gradient.

#### HTML

```html
<p class="gradient-text">Gradient text</p>
```

#### CSS

```css
.gradient-text {
  background: -webkit-linear-gradient(pink, red);
  -webkit-text-fill-color: transparent;
  -webkit-background-clip: text;
}
```

#### Demo

<div class="snippet-demo">
  <p class="snippet-demo__gradient-text">
    Gradient text
  </p>
</div>

<style>
.snippet-demo__gradient-text {
  background: -webkit-linear-gradient(pink, red);
  -webkit-text-fill-color: transparent;
  -webkit-background-clip: text;
  font-size: 2rem;
  font-weight: bold;
  margin: 0;
}
</style>

#### Giải thích

1. `background: -webkit-linear-gradient(...)` cho phần tử văn bản một hình nền gradient.
2. `webkit-text-fill-color: transparent` lấp đầy văen bản với màu trong suốt.
3. `webkit-background-clip: text` cắt hình nền với văn bản, lấp đầy văn bản với nền gradient giống như một màu.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">⚠️ Uses non-standard properties.</span>

* https://caniuse.com/#feat=text-stroke

<!-- tags: visual -->
### Đường viền dạng mảnh

CHo một phần tử một đường viền bằng 1px thiết bị gốc về chiều rộng, có thể nhìn rất sắc nét.

#### HTML

```html
<div class="hairline-border">text</div>
```

#### CSS

```css
.hairline-border {
  box-shadow: 0 0 0 1px;
}

@media (min-resolution: 2dppx) {
  .hairline-border {
    box-shadow: 0 0 0 0.5px;
  }
}

@media (min-resolution: 3dppx) {
  .hairline-border {
    box-shadow: 0 0 0 0.33333333px;
  }
}

@media (min-resolution: 4dppx) {
  .hairline-border {
    box-shadow: 0 0 0 0.25px;
  }
}
```

#### Demo

<div class="snippet-demo">
  <p class="snippet-demo__hairline-border">Text with a hairline border around it.</p>
</div>

<style>
.snippet-demo__hairline-border {
  box-shadow: 0 0 0 1px;
}

@media (min-resolution: 2dppx) {
  .snippet-demo__hairline-border {
    box-shadow: 0 0 0 0.5px;
  }
}

@media (min-resolution: 3dppx) {
  .snippet-demo__hairline-border {
    box-shadow: 0 0 0 0.33333333px;
  }
}

@media (min-resolution: 4dppx) {
  .snippet-demo__hairline-border {
    box-shadow: 0 0 0 0.25px;
  }
}
</style>

#### Giải thích

1. `box-shadow`, khi chỉ sử dụng bề rộng, thêm một đường viền pseudo cái à có thể dùng pixel phụ\*.
2. Dùng `@media (min-resolution: ...)` để kiểm tra tỉ lệ của thiết bị (`1dppx` bằng 96 DPI), cài đặt bề rộng của `box-shadow` bằng `1 / dppx`.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">⚠️ Needs alternate syntax and JavaScript user agent checking for full support.</span>

* https://caniuse.com/#feat=css-boxshadow
* https://caniuse.com/#feat=css-media-resolution

<hr>

\*Chrome does not support subpixel values on `border`. Safari does not support subpixel values on `box-shadow`. Firefox supports subpixel values on both.

<!-- tags: visual -->
### gradient thanh cuộn tràn

Thêm một hiệu ứng mờ gradient để phần tử tràn hiển thị tốt hơn có nhiều nội dung để được cuộn.

#### HTML

```html
<div class="overflow-scroll-gradient">
  <div class="overflow-scroll-gradient__scroller">
    Content to be scrolled
  </div>
</div>
```

#### CSS

```css
.overflow-scroll-gradient {
  position: relative;
}
.overflow-scroll-gradient::after {
  content: '';
  position: absolute;
  bottom: 0;
  width: 240px;
  height: 25px;
  background: linear-gradient(
    rgba(255, 255, 255, 0.001),
    white
  ); /* transparent keyword is broken in Safari */
  pointer-events: none;
}
.overflow-scroll-gradient__scroller {
  overflow-y: scroll;
  background: white;
  width: 240px;
  height: 200px;
  padding: 15px 0;
  line-height: 1.2;
  text-align: center;
}
```

#### Demo

<div class="snippet-demo">
  <div class="snippet-demo__overflow-scroll-gradient">
    <div class="snippet-demo__overflow-scroll-gradient__scroller">
      Content to be scrolled
    </div>
  </div>
</div>

<style>
.snippet-demo__overflow-scroll-gradient {
  position: relative;
}
.snippet-demo__overflow-scroll-gradient::after {
  content: '';
  background: linear-gradient(rgba(255, 255, 255, 0.001), white);
  position: absolute;
  width: 240px;
  height: 25px;
  bottom: 0;
  pointer-events: none;
}
.snippet-demo__overflow-scroll-gradient__scroller {
  overflow-y: scroll;
  background: white;
  width: 240px;
  height: 200px;
  padding: 15px 0;
  line-height: 1.2;
  text-align: center;
}
</style>

<script>
document.querySelector('.snippet-demo__overflow-scroll-gradient__scroller').innerHTML = 'content '.repeat(100)
</script>

#### Giải thích

1. `position: relative` trên phần tử cha khởi tạo một vị trí theo ngữ cảnh cho các phần tử.
2. `::after` Định nghĩa phần tử pseudo.
3. `background-image: linear-gradient(...)` Thêm một gradient tuyến tính nó mờ dần từ dạng trong suốt sang màu trắng(trên xuống dưới).
4. `position: absolute` cho phần tử pseudo ra khỏi luồng của tài liệu và vị trí nó tuân theo phần tử cha.
5. `width: 240px` hợp với kích thuước của phần tử cuộn( nó là một phần tử con của cha có phần tử pseudo).
6. `height: 25px` là chiều cao của bóng mờ gradient của phần tử pseudo, nó nên giữ tương đối nhỏ.
7. `bottom: 0` đặt vị trí của phần tử pseudo ở dưới so với phần tử cha.
8. `pointer-events: none` quy định phần tử pseudo không thể chỉ vào khi dùng chuột, cho phép văn bản đằng sau vẫn có thể chọn hoặc tương tác.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">✅ No caveats.</span>

* https://caniuse.com/#feat=css-gradients

<!-- tags: visual -->
### Gạch dưới văn bản đẹp

Một thay thế đẹp hơn cho `text-decoration: underline` nơi mà phần thụt chữ không bị cắt bởi gạch dưới. Được thực hiện như `text-decoration-skip-ink: auto` nhưng nó có điều chỉnh được ít gạch dưới hơn.

#### HTML

```html
<p class="pretty-text-underline">Pretty text underline without clipping descending letters.</p>
```

#### CSS

```css
.pretty-text-underline {
  font-family: Arial, sans-serif;
  display: inline;
  font-size: 18px;
  text-shadow: 1px 1px 0 #f5f6f9, -1px 1px 0 #f5f6f9, -1px -1px 0 #f5f6f9, 1px -1px 0 #f5f6f9;
  background-image: linear-gradient(90deg, currentColor 100%, transparent 100%);
  background-position: 0 0.98em;
  background-repeat: repeat-x;
  background-size: 1px 1px;
}
.pretty-text-underline::-moz-selection {
  background-color: rgba(0, 150, 255, 0.3);
  text-shadow: none;
}
.pretty-text-underline::selection {
  background-color: rgba(0, 150, 255, 0.3);
  text-shadow: none;
}
```

#### Demo

<div class="snippet-demo">
  <p class="snippet-demo__pretty-text-underline">Pretty text underline without clipping descending letters.</p>
</div>

<style>
.snippet-demo__pretty-text-underline {
  font-family: Arial, sans-serif;
  display: inline;
  font-size: 18px !important;
  text-shadow: 1px 1px 0 #f5f6f9,
    -1px 1px 0 #f5f6f9,
    -1px -1px 0 #f5f6f9,
    1px -1px 0 #f5f6f9;
  background-image: linear-gradient(90deg, currentColor 100%, transparent 100%);
  background-position: 0 0.98em;
  background-repeat: repeat-x;
  background-size: 1px 1px;
}

.snippet-demo__pretty-text-underline::-moz-selection {
  background-color: rgba(0, 150, 255, 0.3);
  text-shadow: none;
}

.snippet-demo__pretty-text-underline::selection {
  background-color: rgba(0, 150, 255, 0.3);
  text-shadow: none;
}
</style>

#### Giải thích

1. `text-shadow: ...` có 4 giá trị với khoảng cách bao gồm một khu vực 4x4 px để đảm bảo gạch dứoi có một bóng đổ "thick" bao gồm đường kẻ nơi mà phần thụt được cắt. Sử dụng màu giống màu nền. Cho một font lớn, sử dụng kích thước lớn hơn `px` .
2. `background-image: linear-gradient(...)` tạo một  90deg gradient với màu hiện tại (`currentColor`).
3. Thuộc tính `background-*` tạo kích thước gradient là 1x1px tại vị trí bên dưới và được lặp lại dọc theo x-axis.
4. `::selection` pseudo selector đảm bảo bóng đổ văn bản không gây trở ngại với sự lựa chọn của văn bản..

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">⚠️ The distance of the underline from the text depends on the internal metrics of a font, so you must ensure everyone sees the same font (i.e. no system fonts which will change based on the OS).</span>

* https://caniuse.com/#feat=css-textshadow
* https://caniuse.com/#feat=css-gradients

<!-- tags: visual -->
### Reset tất cả styles

Resets tất cả styles về giá trị mặc định với một thuộc tính . Nó sẽ không ảnh hưởng đến thuộc tính`direction` và `unicode-bidi`.

#### HTML

```html
<div class="reset-all-styles">
  <h4>Title</h4>
  <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Iure id exercitationem nulla qui repellat laborum vitae, molestias tempora velit natus. Quas, assumenda nisi. Quisquam enim qui iure, consequatur velit sit?</p>
</div>
```

#### CSS

```css
.reset-all-styles {
  all: initial;
}
```

#### Demo

<div class="snippet-demo">
  <div class="snippet-demo__reset-all-styles">
    <h3>Title</h3>
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Iure id exercitationem nulla qui repellat laborum vitae, molestias tempora velit natus. Quas, assumenda nisi. Quisquam enim qui iure, consequatur velit sit?</p>
  </div>
</div>

<style>
.snippet-demo__reset-all-styles {
  all: initial;
}
</style>

#### Giải thích

Thuộc tính `all` cho phép bạn reset tất cả style( được kế thừa và không) về giá trị mặc định.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">⚠️ MS Edge status is under consideration.</span>

* https://caniuse.com/#feat=css-all

<!-- tags: visual -->
### Hình dạng ngăn cách

Sử dụng một hình SVG để chia 2 khối khác nhau để tạo một hình ảnh thú vị hơn theo chiều ngang tiêu chuẩn.

#### HTML

```html
<div class="shape-separator"></div>
```

#### CSS

```css
.shape-separator {
  position: relative;
  height: 48px;
  background: #333;
}
.shape-separator::after {
  content: '';
  background-image: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgZmlsbC1ydWxlPSJldmVub2RkIiBjbGlwLXJ1bGU9ImV2ZW5vZGQiIHN0cm9rZS1saW5lam9pbj0icm91bmQiIHN0cm9rZS1taXRlcmxpbWl0PSIxLjQxNCI+PHBhdGggZD0iTTEyIDEybDEyIDEySDBsMTItMTJ6IiBmaWxsPSIjZmZmIi8+PC9zdmc+);
  position: absolute;
  width: 100%;
  height: 24px;
  bottom: 0;
}
```

#### Demo

<div class="snippet-demo is-distinct">
  <div class="snippet-demo__shape-separator"></div>
</div>

<style>
.snippet-demo__shape-separator {
  position: relative;
  height: 48px;
  margin: -0.75rem -1.25rem;
}
.snippet-demo__shape-separator::after {
  content: '';
  background-image: url(data:image/svg+xml;base64,PHN2ZyB2aWV3Qm94PSIwIDAgMjQgMjQiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgZmlsbC1ydWxlPSJldmVub2RkIiBjbGlwLXJ1bGU9ImV2ZW5vZGQiIHN0cm9rZS1saW5lam9pbj0icm91bmQiIHN0cm9rZS1taXRlcmxpbWl0PSIxLjQxNCI+PHBhdGggZD0iTTEyIDEybDEyIDEySDBsMTItMTJ6IiBmaWxsPSIjZmZmIi8+PC9zdmc+);
  position: absolute;
  width: 100%;
  height: 24px;
  bottom: 0;
}
</style>

#### Giải thích

1. `position: relative` trên phân tử khởi tạo vị trí theo ngữ cảnh cho phần tử pseudo.
2. `::after` định nghĩa một phần tử pseudo.
3. `background-image: url(...)` thêm hình SVG (một hình tam giác 24x24 trên định dạng base64) như một ảnh nền của phần tử pseudo, lặp lại theo mặc định. Nó phải giống màu của khối đang được ngăn cách.
4. `position: absolute` cho phần tử pseudo ra khỏi luồng của tài liệu và vị trí nó tuân theo phần tử cha.
5. `width: 100%` đảm bảo phần tử trải dài theo toàn bộ chiều rộng của phần tử cha.
6. `height: 24px` có cùng chiều cao với hình.
7. `bottom: 0` đặt vị trí của phần tử pseudo so với phía dưới của phần tử cha.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">✅ No caveats.</span>

* https://caniuse.com/#feat=svg

<!-- tags: visual -->
### Hệ thống hàng đợi font

Sử dụng font tự nhiên của hệ điều hành để gần gũi hơn với ứng dụng hiện tại.

#### HTML

```html
<p class="system-font-stack">This text uses the system font.</p>
```

#### CSS

```css
.system-font-stack {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen-Sans, Ubuntu,
    Cantarell, 'Helvetica Neue', Helvetica, Arial, sans-serif;
}
```

#### Demo

<div class="snippet-demo">
  <p class="snippet-demo__system-font-stack">This text uses the system font.</p>
</div>

<style>
.snippet-demo__system-font-stack {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen-Sans, Ubuntu, Cantarell, "Helvetica Neue", Helvetica, Arial, sans-serif;
}
</style>

#### Giải thích

Trình duyệt nhìn thấy mỗi font kế tiếp, lấy cái đầu tiên nếu có thể và trở lại cái kế tiếp nếu nó không thể tìm thấy font (trên hệ thống hoặc xác định trong CSS).

1. `-apple-system` là San Francisco, được dùng trên iOS và macOS (tuy nhiên không trên Chrome)
2. `BlinkMacSystemFont` là San Francisco, được dùng trên macOS Chrome
3. `Segoe UI` được dùng trên Windows 10
4. `Roboto` được dùng trên Android
5. `Oxygen-Sans` được dùng trên GNU+Linux
6. `Ubuntu` được dùng trên Linux
7. `"Helvetica Neue"` và `Helvetica` được dùng trên macOS 10.10 và thấp hơn( được bao bở dấu nháy kép bởi vì có khoảng trắng ở giữa)
8. `Arial` là một font được hỗ trợ rộng rãi bởi tất cả hệ điều hành.
9. `sans-serif` font dự phòng sans-serif nếu không có font nào khác được hỗ trợ.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">✅ No caveats.</span>

<!-- tags: visual -->
### Hình tam giác

Tạo một hình tam giác từ CSS.

#### HTML

```html
<div class="triangle"></div>
```

#### CSS

```css
.triangle {
  width: 0;
  height: 0;
  border-top: 20px solid #333;
  border-left: 20px solid transparent;
  border-right: 20px solid transparent;
}
```

#### Demo

<div class="snippet-demo">
  <div class="snippet-demo__triangles">
    <div class="snippet-demo__triangle snippet-demo__triangle-1"></div>
    <div class="snippet-demo__triangle snippet-demo__triangle-2"></div>
    <div class="snippet-demo__triangle snippet-demo__triangle-3"></div>
    <div class="snippet-demo__triangle snippet-demo__triangle-4"></div>
    <div class="snippet-demo__triangle snippet-demo__triangle-5"></div>
  </div>
</div>

<style>
.snippet-demo__triangles {
  display: flex;
  align-items: center;
}

.snippet-demo__triangle {
  display: inline-block;
  width: 0;
  height: 0;
  margin-right: 0.25rem;
}

.snippet-demo__triangle-1 {
  border-top: 20px solid #333;
  border-left: 20px solid transparent;
  border-right: 20px solid transparent;
}

.snippet-demo__triangle-2 {
  border-bottom: 20px solid #333;
  border-left: 20px solid transparent;
  border-right: 20px solid transparent;
}

.snippet-demo__triangle-3 {
  border-left: 20px solid #333;
  border-top: 20px solid transparent;
  border-bottom: 20px solid transparent;
}

.snippet-demo__triangle-4 {
  border-right: 20px solid #333;
  border-top: 20px solid transparent;
  border-bottom: 20px solid transparent;
}

.snippet-demo__triangle-5 {
  border-top: 40px solid #333;
  border-left: 15px solid transparent;
  border-right: 15px solid transparent;
}
</style>

#### Giải thích

[Xem trong link để có giải thích chi tiết.](https://stackoverflow.com/q/7073484)

Màu của đường viền là màu của hình tam giác. Cạnh bên của tam giác tương ứng với thuộc tính `border-*`. Ví dụ, một màu trên `border-top` nghĩa là mũi tên chỉ xuống.

Thử nghiệm với giá trị `px` để thay đổi tỉ lệ của hình tam giác.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">✅ No caveats.</span>

<!-- tags: visual -->
### Nảy tải trang

`Creates a bouncing loader animation.`
`Tạo một hiệu ứng nảy khi tải trang`

Tạo hiệu ứng nảy khi tải trang.
#### HTML

```html
<div class="bouncing-loader">
  <div></div>
  <div></div>
  <div></div>
</div>
```

#### CSS

```css
@keyframes bouncing-loader {
  from {
    opacity: 1;
    transform: translateY(0);
  }
  to {
    opacity: 0.1;
    transform: translateY(-1rem);
  }
}
.bouncing-loader {
  display: flex;
  justify-content: center;
}
.bouncing-loader > div {
  width: 1rem;
  height: 1rem;
  margin: 3rem 0.2rem;
  background: #8385aa;
  border-radius: 50%;
  animation: bouncing-loader 0.6s infinite alternate;
}
.bouncing-loader > div:nth-child(2) {
  animation-delay: 0.2s;
}
.bouncing-loader > div:nth-child(3) {
  animation-delay: 0.4s;
}
```

#### Demo

<div class="snippet-demo">
  <div class="snippet-demo__bouncing-loader">
    <div></div>
    <div></div>
    <div></div>
  </div>
</div>

<style>
@keyframes bouncing-loader {
  from {
    opacity: 1;
    transform: translateY(0);
  }
  to {
    opacity: 0.1;
    transform: translateY(-1rem);
  }
}
.snippet-demo__bouncing-loader {
  display: flex;
  justify-content: center;
}
.snippet-demo__bouncing-loader > div {
  width: 1rem;
  height: 1rem;
  margin: 3rem 0.2rem;
  background: #8385aa;
  border-radius: 50%;
  animation: bouncing-loader 0.6s infinite alternate;
}
.snippet-demo__bouncing-loader > div:nth-child(2) {
  animation-delay: 0.2s;
}
.snippet-demo__bouncing-loader > div:nth-child(3) {
  animation-delay: 0.4s;
}
</style>

#### Giải thích

Chú ý: `1rem` thường là `16px`.

1. `@keyframes` để định nghĩa một hiệu ứng có 2 trạng thái, khi thành phần thay đổi `opacity` và nó được dịch chuyển lên trong mặt phẳng 2D thì sử dụng `transform: translateY()`.

2. `.bouncing-loader` là khối cha chứa các hình tròn nảy lên  và sử dụng các thuộc tính`display: flex` và `justify-content: center` để đặt chúng vào giữa.

3. `.bouncing-loader > div`, trỏ vào 3 thẻ `div` con của thẻ cha để định dạng. Những thẻ `div` được truyền vào `1rem` cho chiều rộng và chiều cao, sử dụng `border-radius: 50%` để biến chúng từ những hình vuông thành những hình tròn.

4. `margin: 3rem 0.2rem` xác định cho mỗi hình tròn căn lề trên/dưới `3rem` và căn lề trái/phải `0.2rem` để chúng không chạm trực tiếp vào nhau, cho chúng một vài khoảng trống.

5. `animation` là một thuộc tính nhanh với các thuộc hiệu ứng khác nhau: `animation-name`, `animation-duration`, `animation-iteration-count`, `animation-direction` đã được sử dụng.

6. `nth-child(n)` trỏ vào thành phần là con thứ n của thành phần cha.

7. `animation-delay` được dùng với 2s và tương ứng với thẻ `div` thứ 3, để các thành phần không bắt đầu hiệu ứng cùng một lúc.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">✅ No caveats.</span>

* https://caniuse.com/#feat=css-animation

<!-- tags: animation -->
### Hiệu ứng con quay tròn

Tạo một hiệu ứng con quay tròn được sử dụng biểu thị khi tải nội dung.

#### HTML

```html
<div class="donut"></div>
```

#### CSS

```css
@keyframes donut-spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
.donut {
  display: inline-block;
  border: 4px solid rgba(0, 0, 0, 0.1);
  border-left-color: #7983ff;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  animation: donut-spin 1.2s linear infinite;
}
```

#### Demo

<div class="snippet-demo">
  <div class="snippet-demo__donut-spinner"></div>
</div>

<style>
@keyframes snippet-demo__donut-spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg);}
}
.snippet-demo__donut-spinner {
  display: inline-block;
  border: 4px solid rgba(0, 0, 0, 0.1);
  border-left-color: #7983ff;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  animation: snippet-demo__donut-spin 1.2s linear infinite;
}
</style>

#### Giải thích

Sử dụng nửa dạng trong suốt `border` cho toàn thể phần tử, ngoại trừ bên sẽ đóng vai trò phần tải của con quay. Sử dụng `animation` để quay phần tử.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">⚠️ Requires prefixes for full support.</span>

* https://caniuse.com/#feat=css-animation
* https://caniuse.com/#feat=transforms2d

<!-- tags: animation -->
### Các biến làm dịu

Các biến có thể tái sử dụng cho thuộc tính `transition-timing-function` , hiệu quả hơn xây dựng  `ease`, `ease-in`, `ease-out` và `ease-in-out`.

#### HTML

```html
<div class="easing-variables"></div>
```

#### CSS

```css
:root {
  --ease-in-quad: cubic-bezier(0.55, 0.085, 0.68, 0.53);
  --ease-in-cubic: cubic-bezier(0.55, 0.055, 0.675, 0.19);
  --ease-in-quart: cubic-bezier(0.895, 0.03, 0.685, 0.22);
  --ease-in-quint: cubic-bezier(0.755, 0.05, 0.855, 0.06);
  --ease-in-expo: cubic-bezier(0.95, 0.05, 0.795, 0.035);
  --ease-in-circ: cubic-bezier(0.6, 0.04, 0.98, 0.335);

  --ease-out-quad: cubic-bezier(0.25, 0.46, 0.45, 0.94);
  --ease-out-cubic: cubic-bezier(0.215, 0.61, 0.355, 1);
  --ease-out-quart: cubic-bezier(0.165, 0.84, 0.44, 1);
  --ease-out-quint: cubic-bezier(0.23, 1, 0.32, 1);
  --ease-out-expo: cubic-bezier(0.19, 1, 0.22, 1);
  --ease-out-circ: cubic-bezier(0.075, 0.82, 0.165, 1);

  --ease-in-out-quad: cubic-bezier(0.455, 0.03, 0.515, 0.955);
  --ease-in-out-cubic: cubic-bezier(0.645, 0.045, 0.355, 1);
  --ease-in-out-quart: cubic-bezier(0.77, 0, 0.175, 1);
  --ease-in-out-quint: cubic-bezier(0.86, 0, 0.07, 1);
  --ease-in-out-expo: cubic-bezier(1, 0, 0, 1);
  --ease-in-out-circ: cubic-bezier(0.785, 0.135, 0.15, 0.86);
}

.easing-variables {
  width: 50px;
  height: 50px;
  background: #333;
  transition: transform 1s var(--ease-out-quart);
}

.easing-variables:hover {
  transform: rotate(45deg);
}
```

#### Demo

<div class="snippet-demo">
  <div class="snippet-demo__easing-variables">Hover</div>
</div>

<style>
:root {
  --ease-in-quad: cubic-bezier(0.55, 0.085, 0.68, 0.53);
  --ease-in-cubic: cubic-bezier(0.55, 0.055, 0.675, 0.19);
  --ease-in-quart: cubic-bezier(0.895, 0.03, 0.685, 0.22);
  --ease-in-quint: cubic-bezier(0.755, 0.05, 0.855, 0.06);
  --ease-in-expo: cubic-bezier(0.95, 0.05, 0.795, 0.035);
  --ease-in-circ: cubic-bezier(0.6, 0.04, 0.98, 0.335);

  --ease-out-quad: cubic-bezier(0.25, 0.46, 0.45, 0.94);
  --ease-out-cubic: cubic-bezier(0.215, 0.61, 0.355, 1);
  --ease-out-quart: cubic-bezier(0.165, 0.84, 0.44, 1);
  --ease-out-quint: cubic-bezier(0.23, 1, 0.32, 1);
  --ease-out-expo: cubic-bezier(0.19, 1, 0.22, 1);
  --ease-out-circ: cubic-bezier(0.075, 0.82, 0.165, 1);

  --ease-in-out-quad: cubic-bezier(0.455, 0.03, 0.515, 0.955);
  --ease-in-out-cubic: cubic-bezier(0.645, 0.045, 0.355, 1);
  --ease-in-out-quart: cubic-bezier(0.77, 0, 0.175, 1);
  --ease-in-out-quint: cubic-bezier(0.86, 0, 0.07, 1);
  --ease-in-out-expo: cubic-bezier(1, 0, 0, 1);
  --ease-in-out-circ: cubic-bezier(0.785, 0.135, 0.15, 0.86);
}

.snippet-demo__easing-variables {
  width: 75px;
  height: 75px;
  background: #333;
  color: white;
  font-size: 0.8rem;
  font-weight: bold;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: transform 1s var(--ease-out-quart);
}

.snippet-demo__easing-variables:hover {
  transform: rotate(45deg);
}
</style>

#### Giải thích

Các biến được định nghĩa toàn cục trong `:root` lớp pseudo CSS cái mà phù hợp với phần tử của root trong cây đại diện cho tài liệu.. Trong HTML, `:root` đại diện cho phần tử `<html>` và giống với  selector `html`, ngoại trừ độ đặc hiệu cao hơn.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">✅ No caveats.</span>

* https://caniuse.com/#feat=css-variables

<!-- tags: animation -->
### Hiệu ứng gạch chân khi lướt qua

Tao một hiệu ứng gạch chân khi văn bản được chỉ qua.

<small>**Credit:** https://flatuicolors.com/</small>

#### HTML

```html
<p class="hover-underline-animation">Hover this text to see the effect!</p>
```

#### CSS

```css
.hover-underline-animation {
  display: inline-block;
  position: relative;
  color: #0087ca;
}
.hover-underline-animation::after {
  content: '';
  position: absolute;
  width: 100%;
  transform: scaleX(0);
  height: 2px;
  bottom: 0;
  left: 0;
  background-color: #0087ca;
  transform-origin: bottom right;
  transition: transform 0.25s ease-out;
}
.hover-underline-animation:hover::after {
  transform: scaleX(1);
  transform-origin: bottom left;
}
```

#### Demo

<div class="snippet-demo">
  <p class="snippet-demo__hover-underline-animation">Hover this text to see the effect!</p>
</div>

<style>
.snippet-demo__hover-underline-animation {
  display: inline-block;
  position: relative;
  color: #0087ca;
}
.snippet-demo__hover-underline-animation::after {
  content: '';
  position: absolute;
  width: 100%;
  transform: scaleX(0);
  height: 2px;
  bottom: 0;
  left: 0;
  background-color: #0087ca;
  transform-origin: bottom right;
  transition: transform 0.25s ease-out;
}
.snippet-demo__hover-underline-animation:hover::after {
  transform: scaleX(1);
  transform-origin: bottom left;
}
</style>

#### Giải thích

1. `display: inline-block` tạo khối thẻ `p` một `inline-block` để ngăn chặn phần gạch chân kéo dài toàn bộ chiều rộng của phần tử cha hơn là phần nội dung văn bản.
2. `position: relative` trên phần tử khởi tạo vị trí ngữ cảnh cho phần stuwr pseudo
3. `::after` định nghĩa một phần tử pseudo.
4. `position: absolute` cho phần tử pseudo ra khỏi luồng của tài liệu và vị trí nó tuân theo phần tử cha.
5. `width: 100%` đảm bảo phần tử pseudo kéo dài toàn bộ chiều rộng của khối văn bản.
6. `transform: scaleX(0)` khởi tạo giá trị scales của phần tử pseudo là 0 nên nó không có chiều rộng và không nhìn thấy.
7. `bottom: 0` và `left: 0` vị trí so với dưới và bên trái của khối.
8. `transition: transform 0.25s ease-out` nghĩa là thay đổi `transform` sẽ chuyển đổi trong 0.25 giấy với một hàm thời gian `ease-out` .
9. `transform-origin: bottom right` nghĩa là chuyển điểm neo là vị trí bên dưới bên phải của khối.
10. `:hover::after` sau đó sử dụng `scaleX(1)` để chuyển đổi chiều rộng 100%, sau đó thay đổi `transform-origin` đến `bottom left` để điểm neo đảo ngược, cho phép nó chuyển tiếp 1 hướng khác khi lơ lửng.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">✅ No caveats.</span>

* https://caniuse.com/#feat=transforms2d
* https://caniuse.com/#feat=css-transitions

<!-- tags: animation -->
### Tắt lựa chọn

Làm cho vùng nội dung không thể chọn được.

#### HTML

```html
<p>You can select me.</p>
<p class="unselectable">You can't select me!</p>
```

#### CSS

```css
.unselectable {
  user-select: none;
}
```

#### Demo

<div class="snippet-demo">
  <p>You can select me.</p>
  <p class="snippet-demo__disable-selection">You can't select me!</p>
</div>

<style>
.snippet-demo__disable-selection {
  user-select: none;
}
</style>

#### Giải thích

`user-select: none` quy định rằng văn bản không thể chọn được.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">⚠️ Requires prefixes for full support.</span>
<span class="snippet__support-note">⚠️ This is not a secure method to prevent users from copying content.</span>

* https://caniuse.com/#feat=user-select-none

<!-- tags: interactivity -->
### Hiệu ứng gradient theo con trỏ chuột

Một hiệu ứng lướt qua nơi mà gradien sẽ chạy theo con trỏ chuột.

<small class="snippet__credit">**Credit:** [Tobias Reich](https://codepen.io/electerious/pen/MQrRxX)</small>

#### HTML

```html
<button class="mouse-cursor-gradient-tracking">
  <span>Hover me</span>
</button>
```

#### CSS

```css
.mouse-cursor-gradient-tracking {
  position: relative;
  background: #7983ff;
  padding: 0.5rem 1rem;
  font-size: 1.2rem;
  border: none;
  color: white;
  cursor: pointer;
  outline: none;
  overflow: hidden;
}

.mouse-cursor-gradient-tracking span {
  position: relative;
}

.mouse-cursor-gradient-tracking::before {
  --size: 0;
  content: '';
  position: absolute;
  left: var(--x);
  top: var(--y);
  width: var(--size);
  height: var(--size);
  background: radial-gradient(circle closest-side, pink, transparent);
  transform: translate(-50%, -50%);
  transition: width 0.2s ease, height 0.2s ease;
}

.mouse-cursor-gradient-tracking:hover::before {
  --size: 200px;
}
```

#### JavaScript

```js
var btn = document.querySelector('.mouse-cursor-gradient-tracking')
btn.onmousemove = function(e) {
  var x = e.pageX - btn.offsetLeft
  var y = e.pageY - btn.offsetTop
  btn.style.setProperty('--x', x + 'px')
  btn.style.setProperty('--y', y + 'px')
}
```

#### Demo

<div class="snippet-demo">
  <button class="snippet-demo__mouse-cursor-gradient-tracking">
    <span>Hover me</span>
  </button>
</div>

<style>
.snippet-demo__mouse-cursor-gradient-tracking {
  position: relative;
  background: #7983ff;
  padding: 0.5rem 1rem;
  font-size: 1.2rem;
  border: none;
  color: white;
  cursor: pointer;
  outline: none;
  overflow: hidden;
}

.snippet-demo__mouse-cursor-gradient-tracking span {
  position: relative;
}

.snippet-demo__mouse-cursor-gradient-tracking::before {
  --size: 0;
  content: '';
  position: absolute;
  left: var(--x);
  top: var(--y);
  width: var(--size);
  height: var(--size);
  background: radial-gradient(circle closest-side, aqua, rgba(0,255,255,0.0001));
  transform: translate(-50%, -50%);
  transition: width .2s ease, height .2s ease;
}

.snippet-demo__mouse-cursor-gradient-tracking:hover::before {
  --size: 200px;
}
</style>

<script>
(function () {
  var btn = document.querySelector('.snippet-demo__mouse-cursor-gradient-tracking')
  btn.onmousemove = function (e) {
    var x = e.pageX - btn.offsetLeft - btn.offsetParent.offsetLeft
    var y = e.pageY - btn.offsetTop - btn.offsetParent.offsetTop
    btn.style.setProperty('--x', x + 'px')
    btn.style.setProperty('--y', y + 'px')
  }
})()
</script>

#### Giải thích

_TODO_

**Chú ý!**

Nếu phần tử của cha có một vị trí theo ngữ cảnh (`position: relative`), bạn sẽ cần trừ đi các khoảng cách của nó để đẹp hơn.

```js
var x = e.pageX - btn.offsetLeft - btn.offsetParent.offsetLeft
var y = e.pageY - btn.offsetTop - btn.offsetParent.offsetTop
```

#### Hỗ trợ trình duyệt

<div class="snippet__requires-javascript">Requires JavaScript</div>
<span class="snippet__support-note">⚠️ Requires JavaScript.</span>

* https://caniuse.com/#feat=css-variables

<!-- tags: visual, interactivity -->
### Bật ra menu

Bật ra một menu để tương tác khi di chuyển qua.

#### HTML

```html
<div class="reference">
  <div class="popout-menu">
    Popout menu
  </div>
</div>
```

#### CSS

```css
.reference {
  position: relative;
}
.popout-menu {
  position: absolute;
  visibility: hidden;
  left: 100%;
}
.reference:hover > .popout-menu {
  visibility: visible;
}
```

#### Demo

<div class="snippet-demo">
  <div class="snippet-demo__reference">
    <div class="snippet-demo__popout-menu">
      Popout menu
    </div>
  </div>
</div>

<style>
.snippet-demo__reference {
  background: linear-gradient(135deg, #ff4c9f, #ff7b74);
  height: 75px;
  width: 75px;
  position: relative;
  will-change: transform;
}
.snippet-demo__popout-menu {
  position: absolute;
  visibility: hidden;
  left: 100%;
  background: #333;
  color: white;
  font-size: 0.9rem;
  padding: 0.4rem 0.8rem;
  width: 100px;
  text-align: center;
}
.snippet-demo__reference:hover > .snippet-demo__popout-menu {
  visibility: visible;
}
</style>

#### Giải thích

1. `position: relative` trên phần tử cha khởi tạo một vị trí cho phần tử con.
2. `position: absolute` cho phần tử pseudo ra khỏi luồng của tài liệu và vị trí nó tuân theo phần tử cha.
3. `left: 100%` đưa menu 100% theo chiều rộng của cha về bên trái.
4. `visibility: hidden` ẩn menu khi khởi tạo và cho phép chuyển đổi (không giống `display: none`).
5. `.reference:hover > .popout-menu` nghĩa là khi  `.reference` được di chuyển qua, chọn một phần tử con ngay lập tức với lớp `.popout-menu` và thay đổi `visibility` sang `visible` của chúngm, nó hiển thị ra popout.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">✅ No caveats.</span>

<!-- tags: interactivity -->
### Hiệu ứng mờ dần các phần tử liền kề

Mờ dần các phần tử kề nó khi được trỏ qua.

#### HTML

```html
<div class="sibling-fade">
  <span>Item 1</span>
  <span>Item 2</span>
  <span>Item 3</span>
  <span>Item 4</span>
  <span>Item 5</span>
  <span>Item 6</span>
</div>
```

#### CSS

```css
span {
  padding: 0 1rem;
  transition: opacity 0.2s;
}

.sibling-fade:hover span:not(:hover) {
  opacity: 0.5;
}
```

#### Demo

<div class="snippet-demo">
  <nav class="snippet-demo__sibling-fade">
    <span>Item 1</span>
    <span>Item 2</span>
    <span>Item 3</span>
    <span>Item 4</span>
    <span>Item 5</span>
    <span>Item 6</span>
  </nav>
</div>

<style>
.snippet-demo__sibling-fade {
  cursor: default;
  line-height: 2;
  vertical-align: middle;
}

.snippet-demo__sibling-fade span {
  padding: 1rem;
  transition: opacity 0.2s;
}

.snippet-demo__sibling-fade:hover span:not(:hover) {
  opacity: 0.5;
}
</style>

#### Giải thích

1. `transition: opacity 0.2s` quy định về thay đổi độ mờ sẽ đươc chuyển đỏi trong 0.2s.
2. `.sibling-fade:hover span:not(:hover)` quy định khi phần tử cha được di qua, chọn bất kỳ thẻ `span` con nào hiện tại đang không được di qua và thay đổi độ mờ của nó về `0.5`.

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">✅ No caveats.</span>

* https://caniuse.com/#feat=css-sel3
* https://caniuse.com/#feat=css-transitions

<!-- tags: interactivity -->
### Tuỳ biến các giá trị

Các giá trị css chứa giá trị riêng được sử dụng lại thông qua một tài liệu.

#### HTML

```html
<p class="custom-variables">CSS is awesome!</p>
```

#### CSS

```css
:root {
  --some-color: #da7800;
  --some-keyword: italic;
  --some-size: 1.25em;
  --some-complex-value: 1px 1px 2px whitesmoke, 0 0 1em slategray, 0 0 0.2em slategray;
}

.custom-variables {
  color: var(--some-color);
  font-size: var(--some-size);
  font-style: var(--some-keyword);
  text-shadow: var(--some-complex-value);
}
```

#### Demo

<div class="snippet-demo">
  <div class="snippet-demo__custom-variables">
    <p>CSS is awesome!</p>
  </div>
</div>

<style>
.snippet-demo__custom-variables {
  --some-color: #686868;
  --some-keyword: italic;
  --some-size: 1.25em;
  --some-complex-value: 1px 1px 2px whitesmoke, 0 0 1em slategray , 0 0 0.2em slategray;
}

.snippet-demo__custom-variables p {
  color: var(--some-color);
  font-size: var(--some-size);
  font-style: var(--some-keyword);
  text-shadow: var(--some-complex-value);
}
</style>

#### Giải thích

Các biến được định nghĩa toàn cục trong lớp `:root` cái mà khớp với phần tử root trong cây đại diện cho tài liệu. Các biến có thể cũng được bao gồm phạm vi một selector nếu định nghĩa trong khối

Khai báo một biến với `--variable-name:`.

Sử dụng lại biến thông qua tài liệu sử dụng hàm `var(--variable-name)` .

#### Hỗ trợ trình duyệt

<span class="snippet__support-note">✅ No caveats.</span>

* https://caniuse.com/#feat=css-variables

<!-- tags: other -->
