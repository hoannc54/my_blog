# ITCSS: Mở rộng và bảo trì kiến trúc CSS
Làm thế nào tôi làm cho CSS của tôi có thể mở rộng và bảo trì ? Đó là vấn đề của mọi front-end developer. ITCSS có câu trả lời.

Năm ngoái khi chúng tôi bắt đầu kế hoạc để thiết kế lại [HEROized][1] và wesite Xfive.co mới, tôi tìm kiếm một kiến trúc CSS, cho phép phát triển website và bảo trì trong tương lai dễ dàng.

[CSS Modules][2] còn khá mới và mới lạ vào thời điểm đó đó và tôi luôn luôn xem xét với [Atomic Design][3] tương tự được tạo ra. Sau đó tôi đã gặp [Harry Roberts's][4] của ITCSS vào tháng 6-2015 vấn đề của [net magazine][5] và ngay lập tức yêu thích các tiếp cận CSS đơn giản.

## ITCSS là gì?

ITCSS viết tắt của _Inverted Triangle CSS_ và nó giúp bạn tổ chức các file CSS trong dự án của bạn theo cách mà bạn có thể làm tốt hơn **deal with** (not always easy-to-deal with) CSS đặc biệt giống **global namespace, cascade and selectors specificity**.

ITCSS có thể được dùng với bộ tiền xử lý hoặc không có chúng và tương thích với phương thức giống BEM, SMACSS or OOCSS.

Một trong những nguyên tắc chính của ITCSS là nó tách code cơ bản CSS của bạn thành một số phần (được gọi _layers_), có dạng của hình tam giác ngược    :

![ITCSS Layers][6]

Những lớp giống như sau:

* **Settings** – Được dùng với bộ tiền xử lý và chứ font, định nghĩa màu, ...
* **Tools** – globally used mixins and functions. Điều quan trọng là nó không tạo ra bất kỳ CSS nào trong 2 lớp đầu tiên.
* **Generic** –  cài lại và/hoặc chuẩn hóa style, định nghĩa box-sizing. Đây là lớp đầu tiên thực sự tạo ra css.
* **Elements** – Định kiểu cho các phần tử HTML cơ bản (như H1, A, v.v). Chúng đi kèm với định dạng mặc định từ trình duyệt để chúng tôi có thể xác định lại chúng ở đây.
* **Objects** – class-based selectors cái mà xác định các mẫu thiết kế chưa được thiết kế, ví dụ đối tượng truyền thông được biết đến từ OOCSS.
* **Components** – Các thành phần UI cụ thể. Đây là noi phần lớn các công việc của chúng tôi diễn ra và các thành phần UI của chúng tôi thường được tạo của Objects and Components
* **Utilities** – Lớp utilities và helper với khả năng ghi đè mọi thư phía trước của tam giác. Ví dụ: ẩn lớp helper

Hình tam giác cũng cho thấy cách trình bày các style bở các selector và order trong kết quả CSS: Từ các style chung đến rõ ràng, từ các selector có tính cụ thể thấp đến cụ thể hơn (nhưng vẫn không quá cụ thể, IDs không được phép) và từ ảnh hưởng rộng đến ảnh hưởng ít. 

![ITCSS Key Metrics][7]

Tổ chức CSS sẽ giúp bạn tránh Specificity Wars và được diễn tả [a healthy specificity graph][8].

## Tài liệu

_Cập nhật 10/27/2016: The net magazine vừa được tái bản bài báo gốc từ phiên bản in(Xem nguồn bên dưới)._

Thông thường, đến đây tôi sẽ giới thiệu bạn đến [ITCSS webpage][9] cho học thêm. Tuy nhiên, không có gì giống như tài liệu mã nguồn mở tồn tại.

ITCSS vẫn còn một phần thuộc tính và nếu bạn muốn tận dụng nó, bạn nên học phần giới thiệu ban đầu trong tạp chí. Tôi không ở đây để đánh giá ý định của tác giả(Tôi cảm ơn anh ấy đã chia sẻ sự hiểu biết của anh ấy), nhưng tôi nghĩ điều này tránh ITCSS mở rộng hơn(có thể là ý định sau tất cả).

> The partially proprietary character of ITCSS prevents its wider adoption.

Điều này không ngăn bạn bắt đầu sử dụng nó cho dự án của bạn, tuy nhiên, nếu bạn thực sự quan tâm đến việc làm như vậy. [Lấy các vấn đề cụ thể][10] của tạp trí để học các nguyên tắc cơ bản của ITCSS, và sau đó học sẵn sàng học tài nguyên online và ví dụ để giúp bạn áp dụng trong các dự án thực tế.


## Tài nguyên

Tôi đã sử dụng ITCSS trong 4 dự án cho đến nay (bao gồm Xfive.co) và các tài nguyên sau đã giúp tôi hiểu hơn về nó:

Bạn cũng có thể kiểm tra [Chisel][11], Yeoman bộ tạo ra của chúng tôi cho dự án front-end và WordPress, hỗ trợ ITCSS.


## Kinh nghiệm

Dưới đây là một vài suy nghĩ dựa trên kinh nghiệm của tôi với dự án ITCSS:

### Nghĩ ít về tên và style cục bộ

ITCSS quy định tính đặc biệt khi kết hợp với [quy ước đặt tên BEMIT][12] cho phép bạn tập trung hơn để giải quyết các thử thách front-end hơn là nghĩ về các đặt tên và style cục bộ. Đây là những gì Xfive.co main.scss nhìn thấy như:

    @import "settings.colors";
    @import "settings.global";
    
    @import "tools.mixins";
    
    @import "normalize-scss/normalize.scss";
    @import "generic.reset";
    @import "generic.box-sizing";
    @import "generic.shared";
    
    @import "elements.headings";
    @import "elements.hr";
    @import "elements.forms";
    @import "elements.links";
    @import "elements.lists";
    @import "elements.page";
    @import "elements.quotes";
    @import "elements.tables";
    
    @import "objects.animations";
    @import "objects.drawer";
    @import "objects.list-bare";
    @import "objects.media";
    @import "objects.layout";
    @import "objects.overlays";
    
    @import "components.404";
    @import "components.about";
    @import "components.archive";
    @import "components.avatars";
    @import "components.blog-post";
    @import "components.buttons";
    @import "components.callout";
    @import "components.clients";
    @import "components.comments";
    @import "components.contact";
    @import "components.cta";
    @import "components.faq";
    @import "components.features";
    @import "components.footer";
    @import "components.forms";
    @import "components.header";
    @import "components.headings";
    @import "components.hero";
    @import "components.jobs";
    @import "components.legal-nav";
    @import "components.main-cta";
    @import "components.main-nav";
    @import "components.newsletter";
    @import "components.page-title";
    @import "components.pagination";
    @import "components.post-teaser";
    @import "components.process";
    @import "components.quote-banner";
    @import "components.offices";
    @import "components.sec-nav";
    @import "components.services";
    @import "components.share-buttons";
    @import "components.social-media";
    @import "components.team";
    @import "components.testimonials";
    @import "components.topbar";
    @import "components.reasons";
    @import "components.wordpress";
    @import "components.work-list";
    @import "components.work-detail";
    
    @import "vendor.prism";
    
    @import "trumps.clearfix";
    @import "trumps.utilities";
    
    @import "healthcheck";

_Chú ý: Chúng ta dùng [các thư mục riêng biệt cho mỗi lớp][13] và tải các bảng slyle được thêm tự động trong[Chisel][11]._

### Các đối tượng sử dụng lại cho quá trình phát triển nhanh

Đối tượng của ITCSS là ứng cử viên hoàn hảo cho việc xây dựng một thư viện của các thành phần tái sử dụng cho phép phát triển front-end nhanh. Các thành phần UI sẽ bao gồm các đối tượng chung và các thành phần cụ thể dự án. Ví dụg, innuitcss giống như một ITCSS chung dựa trên framework chứa [một nhóm đối tượng][14] nhưng chỉ [một thành phần mẫu][15].

### Animations

TTôi khuyên bạn nên định nghĩa thành phần chung, animation toàn cục giống như đối tượng. Ví dụ. _@keyframes_ _o-fade-in_ trong file __objects.animations.scss_

Animations cụ thể nên được xác định tương ứng mỗi file thành phần, Ví dụ. _@keyframes c-hero-scale_ in __components.hero.scss._

### Tính mềm dẻo

ITCSS thì các điều kiện khá mềm dẻo trong luồng làm việc và công cụ của bạn. một trong những developer của chúng tôi bày tỏ một lo ngại về việc có bao nhiêu bản mẫu ITCSS đi cùng. Nhưng trên thực tế điều này hoàn toàn tuỳ thuộc vào bạn - ITCSS không quy định bạn cần có tất cả bao nhiêu lớp (Chỉ trong thứ tự cái gì nên có nếu chúng xuất hiện).

Vì vậy trong một thiếu lập bạn cần có tối thiểu các thành phần với các kiểu element mặc định từ trình duyệt. Dĩ nhiên, đây không phải rất thiết thực - trong một số cài đặt, thiết lâp lại và hoặc CSS bình thường được sử dụng bởi hầu hết mọi người vì những lý do tốt.

### Critical CSS

ITCSS chạy tốt với CSS chuẩn do các chỉ số trong tam giác ngược. Mô hình dựa trên các thành phần cho phép bạn chia các thư mục UI ở trên vào các thành phần hợp lý bạn thậm chí có thể chọn các phần trong chuẩn CSS của bạn bằng tay (sẽ nói nhiều hơn trong bài báo sắp tới).

### Kích thước file và sự sao chép style

Nếu có bất kỳ mối quan ngại về một kiến trúc giống như ITCSS hoặc về bất kỳ thành phần cơ bản nào giống như kiến trúc CSS, nó có thể là kết quả kích thuước file. Các thành phần đóng gói các style và cho phép chúng tôi tránh xung đột CSS và ghi đè nhưng nó cũng có nghĩa là sự lặp lại xảy ra khá thuường xuyên.

ITCSS không thể cạnh tranh với một trường hợp này [functional CSS][16] . Mặc khác, nếu bạn tìm thấy nhiều thành phần định dạng tự bạn lặp lại, bạn có thể cân nhăcs chuyển những style này thành các đối tượng riêng lẻ.

## Phần kết luận

Bạn không thể đi sai với ITCSS. Nó là kết quả của kinh nghiệm và nhiều năm làm việc bởi Harry Roberts, một trong những tác giả nổi tiếng nhất của CSS. Nếu bạn không nghĩ sâu bên trong tài nguyên một chút, bạn sẽ thuưởng với một kiến trúc đơn giản nhưng mạnh mẽ cho phép bạn tạo là dự án lớn hoặc nhỏ về CSS có khả năng mở rộng và bảo trì.

Nhưng đừng quên chú ý đến cái nhìn của những người khác giống nhưB [CSS modules][17], trong thời gian chờ đợi.

[1]: http://www.heroized.com/
[2]: http://www.sitepoint.com/understanding-css-modules-methodology/
[3]: http://patternlab.io/
[4]: http://csswizardry.com/
[5]: http://www.creativebloq.com/web-design/manage-large-scale-web-projects-new-css-architecture-itcss-41514731
[6]: https://www.xfivecdn.com/xfive/wp-content/uploads/2016/02/01083650/itcss-layers2.svg
[7]: https://www.xfivecdn.com/xfive/wp-content/uploads/2016/02/10154630/itcss-key-metrics.svg
[8]: https://jonassebastianohlsson.com/specificity-graph/
[9]: http://itcss.io/
[10]: https://www.myfavouritemagazines.co.uk/design/net-magazine-back-issues/
[11]: https://github.com/xfiveco/generator-chisel/
[12]: http://csswizardry.com/2015/08/bemit-taking-the-bem-naming-convention-a-step-further/
[13]: https://github.com/xfiveco/generator-chisel/tree/master/generators/app/templates/styles/itcss
[14]: https://github.com/inuitcss/inuitcss/tree/develop/objects
[15]: https://github.com/inuitcss/inuitcss/tree/develop/components
[16]: https://blog.colepeters.com/building-and-shipping-functional-css/
[17]: https://github.com/css-modules/css-modules
[18]: https://www.xfive.co/blog/itcss-year-after/
[19]: https://github.com/xfiveco/generator-chisel

  [*SMACSS]: Scalable and Modular Architecture for CSS
  [*OOCSS]: Object Oriented CSS
  [*BEM]: Block, Element, Modifier
