[Source](https://www.codeproject.com/Tips/808058/Reasons-for-using-design-patterns "Permalink to Reasons for using design patterns")

# Lý do sử dụng design pattern

## Giới thiệu

Tiếp tục từ bài viết trước có chủ đề [Why design is Critical to Software Development][1], tôi muốn giải quyết một khía cạnh nâng cao hơn một chút về thiết kế phần mềm được gọi là `Design Patterns`. Như bài viết trước của tôi, ý tưởng đến trong một buổi hội thảo liên quan đến sự thành công của thiết kế phần mềm với một đồng nghiệp làm việc cùng. Chủ đề chính của cuộc thảo luận là cho rằng `Design Patterns` thì quá tốn thời gian để được sử dụng trong một lĩnh vực của phát triển phần mềm thương mại. Mục đích của tôi ở đây là để chứng minh tại sao tôi tin rằng điều đó là sai.

Tôi sẽ không đi vào chi tiết bất kỳ cái gì về cơ chế hoặc quá trình thực hiện của `Design Patterns` cụ thể. Những thứ đó có rất nhiều nguồn rất tốt có sẵn ở chỗ khác.

## Design Pattern là gì?

~~Từ khi bắt đầu đến giờ~~ (So getting started then) **Tôi sẽ bắt đầu với**, chính xác `Design Pattern` là gì? Dưới đây là 1 cặp định nghĩa cho thuật ngữ:

Trích từ [Wikipedia][2]:

> "Một design pattern trong kiến trúc và khoa học máy tính là một cách chính ghi lại một giải pháp cho vấn đề thiết kế trong một lĩnh vực chuyên môn cụ thể."

Trích từ [Data & Object Factory][3]:

> "Design patterns là các giải pháp lặp lại cho các vấn đề thiết kế phần mềm bạn ~~tìm thấy 1 lần và lại 1 lần nữa~~ (find again and again) **luôn tìm kiếm** trong phát triển ứng dụng thực tế. Các mẫu(Pattern) là về việc thiết kế và sự tương tác giữa các đối tượng, cũng như cung cấp một nền tảng giao tiếp giữa các phần liên quan, các giải pháp tái sử dụng được đối với các thách thức lập trình thường gặp phải."

Vì vậy  một `Design Pattern` là một sự trừu tượng hoá mục đích chung của một vấn đề, có thể được áp dụng cho một giải pháp cụ thể. Như các nhà phát triển phần mềm có xu hướng giải quyết nhiều loại vấn đề tương tự, nó có nghĩa là bất kỳ giải pháp phần mềm nào sẽ kết hợp các yếu tố tương tự từ các giải pháp khác. Tại sao lại phát minh ra. Tại sao lại đi lại lối mòn đó ? _Why reinvent the wheel?_

## Tài liệu tốt và hiểu nó

Như `Design Patterns` là một tài liệu tốt và hiểu rõ bởi các kiến trúc sư, các nhà thiết kế, các nhà phát triển phần mềm, sau đó ứng dụng của họ trong một giải pháp cụ thể ccũng sẽ được hiểu rõ.

`Design Patterns` cung cấp cho một nhà phát triển phần mềm một danh sách các giải pháp đã được thử và kiểm chứng cho các vấn đề chung, do đó giảm các nguy cơ về kỹ thuật cho dự án không phải sử dụng một thiết kế mới và chưa được kiểm chứng.

`Design Patterns` có thể ban đầu làm giảm thời gian phát triển, vì nó có một hướng học theo, nếu nhóm không quen với chúng. Tuy nhiên, nhìn sâu xuống đường phát triển, một khi quen với nó tăng lên, thì thời gian cần phát triển sẽ giảm dần.

## Sự tương đồng với kỹ thật xây dựng

Để đưa và một cái tương tự như một `Design Pattern` từ lĩnh vực kỹ thuật xây dựng(như tôi đã nêu ra trong bài viết của tôi [Why design is Critical to Software Development][1]) có những điểm tương đồng gần giống với công nghệ phần mềm), sẽ là một giải pháp để vượt qua một con sông. Đây là vấn đề thường xuyên đối với các kỹ sư xây dựng, mà có một vài giải pháp đã được chứng minh và được hiểu rõ. Các kỹ sư xây dựng có thể xây một cái cầu hoặc một cái hầm.

Tại sao một kỹ sư xây dựng phải cố giải quyết vấn đề này từ đầu, khi các giải pháp thực tế trên thế giới có thể đã được biết tới? Có sự tương tương đồng gần gũi giưã kỹ sư xây dựng giải quyết vấn đề qua sông, và kỹ sư phần mềm giải quyết một vấn đề phần mềm:

* Các giải pháp (cầu hoặc hầm) đều được hiểu rõ và được ghi lại.
* Các giải pháp (cầu hoặc hầm) được giải quyết thường xuyên trong các vấn đề xây dựng
* Các vấn đề (cầu hoặc hầm) không phải được xác định hoặc quy định, nhưng có thể trừ tượng hoá và có thể điều chỉnh các vấn đề cụ thể bằng tay(vật liệu xây dựng cầu hoặc hầm có thể được lựa chọn cho chúng chính xác với các vấn đề cụ thê)

Lập luận chống lại `Design Patterns` rằng chúng không thích hợp cho sử dụng \ thương mại do quá trình thực hiện quá lâu và không làm chủ được nó. `Design Patterns` tiết kiệm thời gian (khi xem qua xuyên suốt vòng đời cuả ứng dụng) bằng cách đưa cho các nhà phát triển một sự lựa chọn với các giải pháp đã được thử nghiệm và kiểm chứng có sẵn mà họ có thể tuỳ chỉnh theo yêu cầu của riêng họ.

Vấn đề duy nhất tôi gặp phải với `Design Patterns` là nó mất nhiều thời gian để học. Một số trong số chúng có thể khó hiểu và dễ hiểu. Đây là một lời chỉ trích thích hợp, vì nó đòi hỏi một nhà phát triển có tay nghề cao hơn để sử dụng chúng. Điều này có thể làm tăng các chi phí ban đầu cho dự án. Tuy nhiên, nhìn tổng quan cả quãng thời gian của một ứng dụng tôi hoàn toàn mong đợi những chi phí phát triển ban đầu được bù đắp do các chi phí bảo trì liên tục sẽ thấp hơn và khả năng mở rộng dễ dàng hơn. (làm cho ứng dụng dễ dàng mở rộng trong tương lai để đáp ứng những cơ hội mới và sẽ nổi lên).

`Design Patterns` giảm sự phức tạp và do đó giải pháp trở nên dễ hiểu hơn.

`Design Patterns` là các giải pháp đã được thử và kiểm chứng, nhà phát triển không cần bắt đầu từ đầu, và có thể bắt đầu thực hiện với một giải pháp đã được chứng minh có thể làm việc tốt (miễn là  `Design Pattern` đang được sử dụng để giải quyết một vấn đề tương tự). Sẽ là sai khi muốn một chiếc cầu giải quyết vấn đề vượt qua đại dương, chỗ này một chiếc cầu là không phù hợp.

## Lợi ích của việc sử dụng Design Paterns

`Design Patterns` cung cấp các lợi ích sau:

* Chúng đem đến cho nhà phát triển một sự lựa chọn các giải pháp đã được thử nghiệm và kiểm chứng để làm việc tốt.
* Chúng là ngôn ngữ trung giản và có thể được áp dụng cho bất kỳ ngôn ngữ nào cso hỗ trợ hướng đối tượng.   
* Chúng hỗ trợ các giao tiếp rất thực thế rằng chúng là các vấn đề ghi lại tốt và được nghiên cứu nếu chúng không có trường hợp khác.
* Chúng có một bản ghi được theo dõi đã được chứng minh vì chúng được sử dụng rộng rãi và do đó làm giảm các nguy cơ về kỹ thuật cho dự án.
* Chúng rất linh hoạt và có thể được sủ dụng trong bất kỳ loại ứng ựng hoặc tên miền nào.

## Kết luận

`Design Patterns`, mặc dù khó khăn trong đầu tư học ban đầu, nhưng là một đầu tư rất đáng giá. Chúng ẽ cho phép bạn thực hiện các giải pháp đã được thử nghiệm và kiểm chứng cho các vấn đề, do đó tiết kiệm thời gian và nỗ lực trong giai đoạn thực thi của vòng đời phát triển phần mềm. Bằng cách sử dụng các giải pháp được hiểu rõ và có tài liệu rõ ràng, sản phẩm cuối cùng sẽ có một mức độ hiểu biết cao hơn nhiều. Nếu giải pháp dễ dàng để hiểu, sa đó mở rộng, nó cũng sẽ dễ dàng hơn trong quá trình duy trì.

[1]: http://www.codeproject.com/Tips/806867/Why-Design-is-Critical-to-Software-Development
[2]: http://en.wikipedia.org/wiki/Design_pattern
[3]: http://www.dofactory.com/Patterns/Patterns.aspx

## Practice
- https://coderoncode.com/design-patterns/programming/php/coding/development/2014/01/19/design-patterns-php-factories.html
- https://www.binpress.com/tutorial/the-factory-design-pattern-explained-by-example/142
- https://www.codeproject.com/Articles/852232/PHP-Singleton-Pattern-A-Step-by-Step-And-Problem-s
- http://www.fluffycat.com/PHP-Design-Patterns/

**Từ khoá:** `how to separate code to package php`, `step by step php design pattern singleton`(thay đổi singleton sang các thứ khác để có nhiều kết quả hơn)
