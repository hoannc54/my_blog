# Làm thế nào viết các thẻ Meta Descriptions trong một thế giới thay đổi liên tục (AKA Google Giveth, Google Taketh Away)

**Tóm tắt: Giữa tháng 5 2018, Google đã quay trở lại hiển thị đoạn trích ngắn hơn. Dữ liệu của chúng tôi cho thấy những thay đổi này phổ biến và hầu hết các thẻ meta description bị cắt bỏ trong phạm vi 155-160 ký tự**

Quay trở lại tháng 10, Google thực hiện một sự thay đổi đáng kể trong cách họ hiển thị các đoạn trích dẫn tìm kiếm, với nghiên cứu của chúng tôi cho thấy có nhiều đoạn trích dẫn dài hơn 300 ký tự. Cuối tuần qua, họ dường như quay lại sự thay đổi đó (Danny Sullivan một phần đã xác nhận điều này trên Twitter vào 14/5). Bên cạnh câu hỏi hiển nhiên - Giới hạn mới là bao nhiêu ? Nó có thể khiến bạn tự hỏi làm thế nào đối phó với các quy tắc tiếp tục thay đổi. Không ai trong chúng ta là một người giỏi toàn bộ, nhưng tôi sẽ cố gắng trả lời cả hai câu hỏi dựa trên những gì chúng ta biết bây giờ.

## Lies, dirty lies, and statistics

Tôi đã lấy tất cả đoạn trích dẫn tìm kiếm từ MozCast 10k(Trang 1 kết quả tìm kiếm google với 10,000 từ khoá), vì đó là tập dữ liệu chúng tôi thu thập hàng ngày và có lịch sử đa dạng. Có 89,383 đoạn trích hiển thị trên tập dữ liệu vào sáng ngày 15/5.

Tôi có thể nói với bạn rằng, trên toàn bộ tập dữ liệu, độ dài tối thiểu 6 ký tự, tối đa là 386, và trung bình là 159. Điều đó không hữu ích vì một vài lý do. Đầu tiên, yêu cần bạn viết thẻ meta description trong khoảng 6–386 ký tự không phải là lời khuyên hữu ích. Thứ hai, chúng tôi đang phải giải quyết nhiều trường hợp. Ví dụ đây là đoạn trích tìm kiếm cho "USMC":

![anh](https://d1avok0lzls2w.cloudfront.net/uploads/blog/meta-desc-2018-1-4065.png)

Marine Corps Community Services có thể là một tổ chức chuyệt vời, nhưng tôi xin lỗi rằng thông báo cho thấy thẻ meta description của họ thực tế là "apple" (mà tôi nghĩ chính Google thêm đoạn đó vào). Đây là một đoạn trích cho tìm kiếm vị trí cửa hàng "Younkers":

![](https://d1avok0lzls2w.cloudfront.net/uploads/blog/meta-desc-2018-2-4999.png)

Đặt sang một bên sự nhầm lẫn thương hiệu của họ, tôi nghĩ rằng chúng ta tất cả đều đồng ý "BER Meta TAG1" là không tối ưu. Nếu những trường hợp này dạy bạn bất cứ cái gì , thì nó chỉ về những việc không nên làm. Điều gì xảy ra với trường hợp ngược lại? Đây là đoạn trích với 386 ký tự, từ một tìm kiếm cho cụm từ "non-compete agreement":

![](https://d1avok0lzls2w.cloudfront.net/uploads/blog/meta-desc-2018-3-12620.png)

Chú ý cụm từ "Jump to Exceptions"và các liên kết ở đầu. Chúng được thêm bởi Google, vì vậy thật khó để nói cái gì đếm ngược số ký tự và những gì không. Đây là một trong những trường hợp mà nếu không thêm phần bổ sung thì có đúng 370 ký tự, từ tìm kiếm "the Hunger Games books":

![](https://d1avok0lzls2w.cloudfront.net/uploads/blog/meta-desc-2018-4-11379.png)

Vì vậy, chúng tôi biết rằng các đoạn trích dài hơn vẫn tồn tại. Tuy nhiên, lưu ý rằng cả hai đoạn trích này đều đến từ Wikipedia, đó là một ngoại lệ cho nhiều quy tắc SEO. Có phải những đoạn mô tả dài này chỉ cho các trường hợp bên lề ? Nhìn vào giá trị trung bình (thậm trí là trung bình trong trường hợp này) không cho chúng ta biết điều gì.
## Bức tranh tổng quan lớn, phần 1

Đôi khi, bạn phải để cho dữ liệu tự lên tiếng cho chính mình, với sự dịu dàng tối thiểu. Hãy xem tất cả đoạn trích đã bị cắt (kết thúc với "...") và xoá bỏ các kết quả video (chúng ta đã biết từ nghiên cứu trước rằng chúng ngắn hơn một chút). Điều đó bao gồm 42,863 đoạn trích (chỉ bằng 1 nửa bộ dữ liệu của chúng tôi). Đây là biểu đồ của tất cả độ dài bị cắt, tập hợp thành 25 nhóm ký tự(0–25, 26–50, etc.):

![](https://d1avok0lzls2w.cloudfront.net/uploads/blog/meta-desc-2018-5-4779.png)

Điều này trông rất khác với dữ liệu trước của tháng 12, và được phân cụm rõ ràng trong khoảng 150–175 ký tự. Chúng tôi thấy rằng một vài đoạn trích hiển thị của Google bị cắt sau khoảng 300+, nhưng chúng đều được làm ngắn bởi các phầnn rút gọn.

## Bức tranh tổng quan lớn, phần 2

Chắc chắn, có nhiều điều xảy ra trong khoảng 125–175 ký tự, vì vậy hãy nhìn kỹ và nhìn vào đoạn giữa của phân bố tần số, chia thành các nhóm nhỏ hơn, gồm 5 ký tự:

![](https://d1avok0lzls2w.cloudfront.net/uploads/blog/meta-desc-2018-6-4992.png)

Chúng ta có thể thấy rõ rằng rằng phần lớn các lần cắt giảm này xảy ra trong khoảng 145–165 ký tự. Trước tháng 10, các hướng dẫn trước đây cho thẻ meta description giữ chúng dưới 155 ký tự, do đó, có vẻ như google đã nhiều hoặc ít là đã quay lại với các quy tắc cũ.

Hãy nhớ rằng Google sử dụng phông trữ theo tỉ lệ thuận, do đó không giới hạn ký tự chính xác. Một số người đã đưa ra giả thuyết về giới hạn của chiều rộng theo pixel, giống như với thẻ tiêu đều, nhưng tôi thấy rằng nó khó khăn hơn để ghim xuống đoạn trích nhiều dòng (tình huống càng trở nên khó hơn với kết quả trên di động). Thực tế, cũng khó viết đến giới hạn. Dữ liệu được gợi ý là xấp xỉ 155 ký tự là hợp lý.

## To the Wayback Machine... ?!

Chúng ta có nênn quay trở lại với 155 ký tự được cắt giảm ? Nếu bạn đã viết thẻ meta description dài hơn, bạn nên xoá nó đi và bắt đầu lại? Thật đơn giản là không ai trong chúng ta biết điều gì xảy ra trong tuần tới. Với cách tôi nhìn thấy nó, tôi có 4 lựa chọn khả thi: 

## (1) Let Google handle it

Một số site không có thẻ thẻ meta description. Wikipedia là một trong số chúng.Bây giờ, sự hiểu của Google với  nội dung Wikipedia sâu sắc hơn nhiều so với hầu hết các trang web (thanks, in part, to Wikidata), nhưng nhiều trang web làm tốt mà ko có thẻ. Nếu lựa chọn của bạn là viết các thẻ xấu, lặp đi lặp lại hoặc để trống, sau đó tôi muốn mói để trống và để Google sắp xếp.

## (2) Hãy để ... nơi nó có thể

Bạn có thể chỉ viết với độ dài mà bạn nghĩ là lý tưởng cho bất kỳ trang nào (trong suy luận), và nếu đoạn trích bị cắt, đừng lo lắng về nó. Có thể dấu ba chấm (...) được thêm vào sau. Tôi đang nói đùa, nhưng thực tế việc cắt giảm không phải là cái chết ngọt ngào. Một mô tả(description) hay nên lôi kéo mọi người muốn đọc nhiều hơn.

## (3)Tách mọi thứ về 155 ký tự

Bạn có thể quay trở lại và chuyển mọi thứ về 155 ký tự. Tôi nghĩ rằng đây là thời gian bị tốn nhiều và có thể kết quả tìm kiếm các đoạn trích tệ hơn. Nếu bạn muốn viết lại ngắn hơn thẻ Meta Descriptions cho các trang quan trọng của bạn, điều đó hoàn toàn hợp lý, nhưng hãy nhớ rằng một vài kết quả vẫn hiển đoạn trích dài hơn và tình trạng này sẽ tiếp tục tăng lên.

## (4) Viết mô tả tương ứng với độ dài

Có thể viết môt tả(description) tốt ở cả 2 độ dài không? TÔi nghĩ rằng có thể, với một số sự quan tâm và lập kế hoạch. Tôi sẽ không nhất thiết đề xuất điều này cho mọi trang đơn, nhưng có thể cũng có cách ăn hết cả miếng to, hoặc ăn 1 nửa nó...

## Hướng đến 150/150

Tôi bị ám ảnh một chút với kiểu "inverted pyramid" viết gần đây. Đây là một kiểu theo dạng báo chí nơi mà bạn bắt đầu với phần dẫn hoặc tóm tắt điểm chính của bạn và sau đó chia nhỏ các chi tiết, dữ liệu và ngữ cảnh. Mặc dù cách tiếp cận này rất phù hợp với web, nguồn gốc đến từ các hạn chế trong các bản hiển thị. Bạn không bao giờ biết khi nào trình soạn thảo của bạn sẽ phải cắt ngắn bài viết của bạn để vừa với không gian có sẵn, nên phong cách kim tự tháp ngược giúp đảm bảo phần quan trọng nhất sẽ được bỏ qua.

Sẽ thế nào nếu chúng ta tiếp cận theo phương pháp này với thẻ meta descriptions? Trong các từ khác, tại sao không viết lời dẫn 150 ký tự cho đoạn tóm tắt của trang, và sau đó thêm 150 ký tự hữu ích nhưng ít chi tiết hơn (khi thêm chi tiết đó sẽ có nghĩa và hữu ích)? 150/150 không là một số trừ tượng - bạn có thể thậm chi dùng 100/100 hoặc 100/200. Điều quan trọng là đảm bảo rằng văn bản trước đó bị cắt vẫn giữa được nội dung chính nó.

Hãy suy nghĩ một chút về nó giống như quảng cáo, với 2 dòng riêng biệt của bản copy. Hãy lấy bài đăng blog này:

### Dòng 1 (145ký tự.)

Vào tháng 10, chúng ta đã báo cáo rằng Google tăng snippet tìm kiếm lên trên 300 ký tự. Thật không may, quy tắc giống như đã thay đổi một lần nữa.

### DÒng 2 (122 chars.)

Theo nghiên cứu mới của chúng tôi (5/2018), giới hạn quay trở lại 155-160 ký tự. SEO nên thích ứng với những sự thay đổi này thế nào ?

Dòng 1 có một bản ngắn của của câu chuyện và hi vọng người tìm kiếm biết họ tìm đúng hướng. Dòng 2 đi ssau vào chi tiết và đưa ra đủ dữ liệu(hy vong) hấp dẫn. Nếu Google sử dụng đủ dài mô tả, nó sẽ hoạt động tốt, nhưng nếu không, chúng ta không tệ hơn khi thực hiện.

## Bạn có nên bận tâm không?

Đây có phải là nỗ lực đáng giá không ? Tôi nghĩ rằng viết đoạn mô tả hiệu quả thu hút khách tìm kiếm truy cập vẫn là quan trọng, theo lý thuyết (và điều này tác động gián tiếp ngay cả khi xếp hạng), nhưng bạn có thể tìm thấy bạn có thể viết hoàn toàn trong 155 ký tự tối đa. Chúng ta cũng phải đối mặt với thực tế rằng Google dường như viết lại nhiều hơn và nhiều hơn các mô tả. Điều này rất khó đo, giống như nhiều lần viết lại một phần, nhưng không đảm bảo rằng thẻ meta description sẽ được sử dụng như đã viết.

Có cách nào biết khi nào đoạn trích dài hơn (>300 ký tự) sẽ vấn được sử dụng? Một vài SEO đã đưa ra giả thuyết một link ở giữa snippets dài và đặc sắc ở trên đầu trang. Trong dữ liệu tổng thể của chúng tối, 13.3% của tất cả SERPs có đoạn trích đặc sắc. Nếu chúng ta chỉ xem SERPs với độ dài đoạn trích tối đa hiển thị 160 ký tự (i.e. không có kết quả nào nhiều hơn 160 ký tự), đoạn trích đặc sắc xảy ra 11.4%. Nếu chúng ta chỉ xem SERPs với đoạn trích hiển thị trên 300 ký tự , các đoạn trích đặc sắc xảy ra 41.8%. Trong khi tập dữ liệu thứ hai khá nhỏ, nó là sự khác biệt nổi bật. Dường như có một số kết nối giữa khả năng lấy câu trả lời trong form của GOogle dưới dạng các đoạn snippet nổi bật và khả năng của chúng hoặc sẵn sàng hiển thị các đoạn trích dẫn dài. Trong nhiều trường hợp, mặc dù, những đoạn trích dài hơn này được viết lại hoặc được lấy trực tiếp từ trang, vì vậy ngay cả khi không có gì đảm bảo rằng Google sẽ sử dụng đoạn môt tả thẻ meta dài hơn của bạn.

Hiện tại, có vẻ như hướng dẫn với 155 ký tự được quay trở lại.Nếu bạn đã tăng một số thẻ meta desciption của bạn, tôi không nghĩ có lý do gì để hoảng loạn cả. Nó có thể có nghĩa khi viết lại các đoạn mô tả quá dài trên các trang quan trọng, đặc biệt nếu việc cắt giảm dẫn đến kết quả xấu xảy ra. Nếu bạn chọn viết lại một số trong chúng, hãy xem xét cách dùng 150/150 - ít nhất bạn có thể được kiểm chứng một chút trong tương lai.







