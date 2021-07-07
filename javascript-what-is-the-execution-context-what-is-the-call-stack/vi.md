# Javascript: Execution Context là gì ? Call Stack là gì?

**Execution Context trong Javascript** là gì ?

Tôi cá rằng bạn không biết câu trả lời. 

Các thành phần cơ bản nhất của ngôn ngữ lập trình là gì ?

Có phải là biến và hàm không ? Mọi người có thể học từng khối xây dựng.

Nhưng những gì nằm ngoài những điều cơ bản ?


Những nền tảng nào của Javascript mà bạn nên làm chủ trước khi gọi chính bạn là Javascript developer ở mức trung (hoặc senior)
Có một số: Scope, Closure, Callbacks, Prototype, và những thứ tương tự.

Nhưng trước khi đi sâu hơn vào các khai niệm này bạn nên ít nhất hiểu cách hoạt động của Javascript Engine.

Trong bài viết này, chúng ta sẽ đi qua 2 phần cơ bản của mọi Javascript engine: the Execution Context và the Call Stack

(Đừng sợ. Nó dễ hơn bạn nghĩ)

Sẵn sàng chưa ?

## Javascript: Execution Context là gì ? Bạn sẽ học gì

Trong bài viết này bạn sẽ học: 

* Làm thế nào Javascript Engine hoạt động
* Execution Context trong Javascript
* Call Stack là gì
* Khác nhau giữa Global Execution Context và Local Execution Context

## Javascript: Execution Context là gì ? Làm thế nào Javascript chạy code của bạn ?

Làm thế nào Javascript chạy code của bạn ?

Nếu bạn là một senior developer bạn có thể đã biết câu trả lời 

Nếu bạn là một người mới chúng ta sẽ làm việc cùng nhau.

Sự thật là, Javascript internals không dễ dàng.

Nhưng tôi đảm bảo bạn có thể học chúng.

Vào thời điểm bạn học chúng, bạn sẽ cảm thấy được toàn quyền và thông minh hơn.

Bằng cách xem xét chức năng bên trong Javascript, bạn sẽ trở thành một Javascript developer tôt hơn, ngay cả khi bạn không nắm vững từng chi tiêt riêng biệt.

Bây giờ hãy xem đoạn code sau: 

```
var num = 2;

function pow(num) {
    return num * num;
}
```

Xong

Nó trông không khó !

Bây giờ hãy cho tôi biết: Theo thứ tự bạn nghĩ trình duyệt sẽ đoán đoạn code thế nào ?

Nói cách khác, nếu bạn là trình duyệt, làm thế nào bạn sẽ đọc được đoạn code đó ?

Nghe có vẻ đơn giản. 

Hầu hết mọi người nghĩ "vâng, trình duyệt thực hiện hàm mũ và trả về kết quả sau đó nó gán 2 đến num"

Bạn có muốn biết câu trả lời của học sinh của tôi không ?

__Từ trên xuống__

__Trình duyệt sẽ bắt đầu ở hàm pow, tính toán num * num__

__JS engine sẽ chạy code từng dòng từng dòng__

Tôi đã mong đợi điều đó.

Tôi đã nói chính xác những điều này vào năm trước.

Trong phần tiếp theo, bạn sẽ khám phá những thứ đằng sau những dòng code trông có vẻ đơn giản đó

## Javascript: Execution Context là gì? Javascript Engines

Bạn có muốn trở thành một developer Javascript trung bình không ?

Tôi cá là bạn sẽ không muốn.

Nếu bạn muốn tạo ấn tượng tốt trong một buổi phỏng vấn Javascript bạn nên ít nhất biết **cách Javascript chạy code của bạn**.

(Và một loạt các thứ khác mà tôi sẽ không đề cập ở đây)

Đừng vội vàng trên những khái niệm này

Bạn không thể học mọi thứ trong một ngày. Nó sẽ tốn thời gian.

Đó là tin tốt ? Tôi sẽ làm mọi thứ trở nên dễ hiểu với mọi người (ít nhất là tôi sẽ cố gắng làm điều đó).

Để hiểu cách Javascript chạy code của bạn chúng ta sẽ gặp điều đáng sợ đầu tiên: **the Execution Context**

Execution Context trong Javascript là gì ?

Mỗi lần bạn chạy Javascript trên một trình duyệt (hoặc trên Node) engine đi qua một loạt các bước.

Một trong những bước này liên quan đến **việc tạo ra Global Execution Context**

Nhưng đợi đã, engine là gì ?

Đó là, Javascript engine là một "động cơ" để chạy code Javascript.

Ngày nay, có 2 Javascript engine: Google V8 và SpiderMonkey.

V8 là một Javascript engine mã nguồn mở của Google, được sử dụng trong Google Chrome và Node.js

SpiderMonkey là một Javascript engine của Mozila, được sử dụng trong Firefox.

Cho đến nay chúng ta có Javascript engine và Execution Context.

Bây giờ là lúc để hiểu cách chúng làm việc cùng nhau

## Javascript: Execution Context là gì? Các mà nó hoạt động?

**Engine tạo một Global Execution Context** mỗi lúc bạn chạy một vài code Javascript.

Execution Context là một từ ưa thích để mô tả môi trường mà code Javascript của bạn chạy.

Thật khó để hình dung những điều trừu tượng này, tôi hiểu mà.

Bây giờ hãy nghĩ Global Execution Context giống như một cái hộp:

![anh](https://res.cloudinary.com/practicaldev/image/fetch/s--jWWiNqCD--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/ptgf765a4989zd6twwfp.png)

Hãy nhìn lại đoạn code của chúng ta 

```
var num = 2;

function pow(num) {
    return num * num;
}
```

Engine đọc đoạn code đó như thế nào ?

Đây là một phiên bản đơn giản :

**Engine:** _Dòng 1. Đây là một biến. Tuyệt. hãy lưu nó vào Bộ nhớ toàn cục_

**Engine:** _Dòng 3. Tôi thấy một mô tả hàm. Tuyệt. Hãy lưu chúng vào Bộ nhớ toàn cục_

**Engine:** _Có vẻ như tôi đã xong_

Nếu tôi hỏi bạn lần nữa: trình duyệt "xem" code tiếp theo như thế nào, bạn sẽ nói gì ?

Vâng, nó theo từ trên xuống dưới nhưng ....

Như vậy bạn có thể thấy engine không chạy hàm pow!

Đó là một khai báo hàm, không phải là lời gọi hàm.

Đoạn code ở trên sẽ dịch một vài biến lưu vào Bộ nhớ toàn cụ: Một mô tả hàm và một biến

Bộ nhớ toàn cục ?

Tôi đã nhầm lần bởi Execution Context và bây giờ bạn đang ném ra Bộ nhớ toàn cục ?

Đúng là tôi

Hãy xem Bộ nhớ toàn cục là gì.

## Javascript: Execution Context là gì ? Bộ nhớ toàn cục

Javascript engine cũng có một Bộ nhớ toàn cục.

Bộ nhớ toàn cục chứa các biến toàn cục và các mô tả hàm có lần sử dụng sau.

Nếu bạn đọc "Scope và Closure" của Kyle Simpson bạn có thể thấy rằng Biến toàn cục (Global Memory) trùng lặp với khái niệm Global Scope.

Trong thực tế chúng là những thứ tương tự nhau.

Tôi đang bay cao 10.000 feet ở đây, vì một lý do chính đáng.

Đó là những khái niệm khó.

Nhưng bạn không nên lo lắng bây giờ.

Tôi muốn bạn hiểu 2 phần quan trọng trong vấn đề của chúng ta.

Khi Javascript engine chạy code của bạn nó tạo :

* Một Global Execution context
* Một Global Memory (cũng được gọi là Global Scope hoặc Global Variable Environment)

Mọi thứ đều rõ ràng chưa ?

Nếu tôi là bạn vào thời điểm này tôi sẽ:

* Viết xuống một vài dòng code Javascript
* Phân tích cú pháp code từng bước từng bước như là một engine
* Tạo một đại diện cho cả Global Execution context  và Global Memory trong quá trình thực thi

Bạn có thể viết bài thực hành trên giấy hoặc với một công cụ mẫu

Đối với ví dụ nhỏ của tôi, hình ảnh sẽ trông giống thế này:

![anh](https://res.cloudinary.com/practicaldev/image/fetch/s--Ae3LEIz8--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/7sreqswm895lrphw4xy2.png)

Trong phần tiếp theo, chúng ta sẽ xem xét một điều đáng sợ khác: **Call Stack**

## Javascript: Execution Context là gì ? Call Stack là gì ?

Bạn có một bức tranh rõ ràng về cách **Execution Context**, **Global Memory** và **Javascript engine** phù hợp với nhau không ?

Nếu không hãy dành thời gian của bạn để xem lại phần trước.

Chúng tôi sẽ giới thiệu phần khác trong cả vấn đề: Call stack

Hãy xem điều gì xảy ra trong quá trình code thực thi.

Trước tiên hãy tóm tắt lại những gì xảy ra khi Javascript engine chạy code của bạn:

Nó tạo: 

* Một Global Execution context
* Một Global Memory

Ngoài ra trong ví dụ của chúng ta không có gì xảy ra nữa:

```
var num = 2;

function pow(num) {
    return num * num;
}
```

Code là một sự chỉ định giá trị thuần

Hãy tiến thêm 1 bước nữa.

Điều gì xảy ra nếu tôi gọi hàm ?

```
var num = 2;

function pow(num) {
    return num * num;
}

var res = pow(num);
```

Câu hỏi hấp dẫn.

hành động gọi một hàm trong Javascript làm cho engine yêu cầu giúp đỡ.

Và sự trợ giúp đó đến từ một người bạn của Javascript engine: Call stack

Nó có vẻ không rõ ràng nhưng Javascript engine cần theo dõi những gì đang xảy ra.

Nó dựa trên Call Stack thực hiện điều này.

Call stack giống như log về sự thực thi hiện tại của chương trình.

Trong thực tế nó là một cấu trúc dữ liệu: [Một stack](https://en.wikipedia.org/wiki/Stack_(abstract_data_type))

Làm thế nào Call stack làm việc chính xác ?

Không ngạc nhiên nó có 2 phương thức : Push và pop

Pushing là hành động đưa thứ gì đó vào stack.

Đó là, khi bạn chạy một hàm trong Javascript, engine push hàm đó vào Call stack.

Mọi lời gọi hàm được push vào Call stack

Thứ đầu tiên được đẩy vào là main() (hoặc global()), luồng chính trong thực thi chương trình Javascript của bạn.

Bây giờ, hình ảnh trước đó sẽ giống như sau: 

![](https://res.cloudinary.com/practicaldev/image/fetch/s--u-ktmKSh--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/ww11po2kybdij4zino4r.png)

Poping ở đầu kia là hành động xoá thứ gì đó khỏi stack.

Khi một hàm kết thúc việc thực thi, nó sẽ được pop ra khỏi Call Stack

Và Call stack của chúng ta sẽ trông giống như sau :

![](https://res.cloudinary.com/practicaldev/image/fetch/s--XzP4eR0c--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/2zbqkyan7uu67xrhpx1b.png)

Và bây giờ bạn đã sẵn sàng làm chủ mọi khái niệm Javascript.

Nhưng đâu đã xong! Chuyển đến phần tiếp theo 

## Javascript nâng cao: Execution Context là gì ? Local Execution context

Mọi thứ dường như rõ ràng cho đến giờ.

Chúng ta đang thiếu điều gì ?

Chúng ta biết rằng Javascript engine tạo một Global Execution context và một Global Memory

Sau đó, khi bạn gọi một hàm trong code của bạn: 

* Javascript engine yêu cầu giúp đỡ
* Sự giúp đỡ đến từ một người bạn của Javascript engine : Call stack
*  Call stack giữ sự theo dõi của các hàm đang được gọi trong code của bạn

Tuy nhiên, một điều khác xảy ra khi bạn chạy một hàm trong Javascript

Đầu tiên, hàm xuất hiện trong Global Execution context

Sau đó, context nhỏ khác xuất hiện bên cạnh hàm

Cái hộp nhỏ mới đó được gọi là Local Execution Context

Một Local Execution Context được tạo bên trong hàm đó~

Cái gì ??

Nếu bạn nhận thấy, trong hình trước đó một biến mới xuất hiện trong Global memory: `var res`

Biến `res` có một giá trị không xác định đầu tiên.

Sau đó ngay khi **pow** xuất hiện trong Global Execution Contexxt, hàm thực thi và res có giá trị trả về

Trong giai đoạn thực thi một Local Execution Context được tạo, bên cạnh Local Memory để giữ cá biến cục bộ.

Thật là một khái niệm mạnh mẽ

![](https://res.cloudinary.com/practicaldev/image/fetch/s--Zpkb7RDS--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://thepracticaldev.s3.amazonaws.com/i/8qoepadnbmo8alj8h0vi.png)

Ghi nhớ nó trong tâm trí.

Hiểu cả Global và Local Execution Context là từ khoá cho bậc thầy về Scope và Closures.

## Javascript: Execution Context là gì ? Call stack là gì ? Tóm tăt lại

Bạn có thể tin những gì đằng sau 4 dòng code không ?

Javascript engine tạo một Execution Context, một Global Memory và một Call stack

Nhưng một khi bạn gọi một hàm thì engine tạo một Local Execution Context trong đó có một Local Memory

Đến cuối bài này, bạn sẽ có thể hiểu điều gì xảy ra khi bạn chạy một vài code Javascript.

Thường bị bỏ qua, Javascript internal luôn được xem là điều bí ẩn với các developer mới.

Tuy nhiên, chúng là chìa khoá để làm chủ các khái niệm Javascript nâng cao.

Nếu bạn học Execution Context, Global Memory và Call stack, sau đó Scope, Closures, Callback và những thứ khác dễ dàng như là điều dễ dàng.

Đặc biệt, việc hiểu Call stack là điều rất quan trọng.

Tất cả Javascript sẽ bắt đầu có ý nghĩa khi bạn hình dung nó: cuối cùng bạn sẽ hiểu tại sao Javascript không đồng bộ và tại sao chúng ta cần Callback

Bạn có biết điều gì đăng sau 4 dòng code Javascript không ?

Bây giờ bạn biết.

Cảm ơn vì đã đọc

Tài liệu này là một phần của [Advanced Javascript class available both as remote 1 to 1 training](https://www.valentinog.com/) hoặc [on site training in Europe](https://www.servermanaged.it/formazione-javascript-react.html)



