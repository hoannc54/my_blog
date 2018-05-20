## Những sai lầm phổ biến trong quá trình phát triển cơ sở dữ liệu bởi những người phát triển ứng dụng là gì ?

_source https://stackoverflow.com/questions/621884/database-development-mistakes-made-by-application-developers_

**1. Không sử dụng các chỉ mục thích hợp**

Đây là một điều tương đối dễ dàng nhưng nó vẫn xảy ra mọi lúc. Các khoá ngoài nên được đánh chỉ mục cho chúng. Nếu bạn sử dụng một trường trong WHERE bạn nên (có lẽ nên) có một chỉ mục trên nó. Các chỉ mục như vậy nên thuờng bao gồm nhiều cột dựa trên các truy vấn bạn cần thực hiện.

**2. Không sử dụng các ràng buộc cho tham chiếu**

Cơ sở dữ liệu của bạn có    thể thay đổi ở đây nhưng nếu cơ sở dữ liệu của bạn hỗ trợ referential integrity-- nghĩa là tất cả khoá ngoài được đảm bảo trỏ đến một thực thể đã tồn tại bạn nên sử dụng nó.

Nó khá là một biến để thấy sự thất bại trên cơ sở dữ liệu MySQL. TÔi không tin MyISAM hỗ trợ nó. InnoDB làm được. Bạn sẽ tìm người mà sử dụng MyISAM hoặc những người đang sử dụng InnoDB nhưng không sử dụng nó mọi nơi.

Xem thêm ở đây:

- [Các ràng buộc NOT NULL và FOREIGN KEY quan trọng thế nào nếu bạn luôn luôn kiểm soát dữ liệu đầu vào với PHP ?](https://stackoverflow.com/questions/382309/how-important-are-constraints-like-not-null-and-foreign-key-if-ill-always-contr)
- [Các khoá ngoài có thực sự cần thiết trong một thiết kế cơ sở dữ liệu không?](https://stackoverflow.com/questions/18717/are-foreign-keys-really-necessary-in-a-database-design)
- [Các khoá ngoài có thực sự cần thiết trong một thiết kế cơ sở dữ liệu không?](http://www.diovo.com/2008/08/are-foreign-keys-really-necessary-in-a-database-design/)

**3. Sử dụng khoá chính tự nhiên thay vì khóa đại diện (kỹ thuật)**

Khoá tự nhiên là khoá dựa trên dữ liệu bên ngoài có nghĩa đó là (khó bên ngoài)duy nhất. Các ví dụ phổ biến là các mã code của sản phẩm, các mã code với 2 trạng thái, mã số an sinh xã hội, và nhiều hơn nữa. Các khoá chính đại diện hoặc kỹ thuật là những khoá hoàn toàn không có ý nghĩa bên ngoài hệt thống. Chúng được phát minh ra hoàn toàn để xác định các thực thể và các trường tự động tăng (SQL Server, MySQL, các csdl khác) hoặc các chuỗi(nhất là trong Oracle).

Theo ý kiến của tôi bạn nên **luôn luôn** sử dụng các khoá đại diện. Vấn đề này được đề cập đến trong những câu hỏi này: 

- [Làm thế nào bạn thích khoá chính của bạn?](https://stackoverflow.com/questions/404040/how-do-you-like-your-primary-keys)
- [Cách tốt nhất để thực hành cho khoá chính của bảng là gì?](https://stackoverflow.com/questions/337503/whats-the-best-practice-for-primary-keys-in-tables)
- [Định dạng khoá chính nào bạn nên sử dụng trong trường hợp này.](https://stackoverflow.com/questions/506164/which-format-of-primary-key-would-you-use-in-this-situation)
- [Khoá đại diện so với tự nhiên/kinh doanh](https://stackoverflow.com/questions/63090/surrogate-vs-natural-business-keys)
- [Tôi có nên có một trường riêng cho khoá chính không ?](https://stackoverflow.com/questions/166750/should-i-have-a-dedicated-primary-key-field)

Đây là một chủ đề gây nhiều tranh cãi về cái mà bạn không nhận được sự đồng ý của toàn bộ. Trong khi bạn có thể tìm thấy một vài người, người mà nghĩ khoá tự nhiên là tốt trong một số trường hợp, bạn sẽ không tìm thấy bất kỳ lời chỉ trích về các khoá đại diện khác được cho là không cần thiết. Đó là một nhược điểm nhỏ nếu bạn hỏi tôi.

Hãy thớ rằng, cũng có [những đất nước không tồn tại ](http://en.wikipedia.org/wiki/ISO_3166-1) (ví dụ, Nam Tư).

**4. Viết các câu truy vấn DISTINCT để làm việc**

Bạn thường thấy điều này trong các câu truy vấn tạo ra bởi ORM. Nhìn vào log đầu ra từ Hibernate và bạn sẽ thấy tất cả câu truy vấn bắt đầu với:

```SELECT DISTINCT ...``

Đây là một mẹo nhỏ để đảm bảo rằng không trả về những dòng dữ liệu trùng lặp và lấy các đối tượng trùng lặp. Bạn sẽ thỉnh thoảng thấy những người làm điều này tốt. Nếu bạn thấy chúng quá nhiều, thì đó thực sự là một cờ. Không phải DISTINCT là xấu hoặc không có ứng dụng phù hợp. Nó ( về cả 2 cách tính) nhưng nó không là một đại diện hoặc là một cái ngăn chặn viết các câu truy vấn đúng.

Từ [Tại sao tôt ghét DISTINCT](http://weblogs.sqlteam.com/markc/archive/2008/11/11/60752.aspx):

> Theo ý kiến của tôti lúc mà mọi thứ bắt đầu trở nênn khó chịu là khi một nhà phát triển đang đang xây dựng một lượng truy vấn lớn, kết hợp các bảng với nhau, và bất ngờ anh ta nhận ra rằng có vẻ như anh ta đang lấy các bản ghi trùng lặp( thậm trí nhiều hơn) và phản ứng ngay lập tức của anh ta .. giải pháp của anh ta cho vấn đề này là sử dụng từ khoá DISTINCT và tất cả rắc rối đó được loại bỏ.

**5. Khuyến khích dùng phép hợp các tập**

Một lỗi phổ biến khác bởi nhà phát triển cơ sở dữ liệu ứng dụng là không nhận ra các tập tốn chi phí hơn bao nhiêu. (mệnh đè GROUP BY) có thể được so sánh với phép hợp.

Để cho bạn một ý tưởng làm thế nào để phổ biến rộng rãi điều này, tôi đã viết một chủ đề nhiều lần ở đây và được hạ đánh giá rất nhiều cho nó. 
Ví dụ:

Từ [SQL statement - “join” vs “group by and having”](https://stackoverflow.com/questions/477006/sql-statement-join-vs-group-by-and-having/477013#477013):

> Truy vấn đầu tiên:

```
SELECT userid
FROM userrole
WHERE roleid IN (1, 2, 3)
GROUP by userid
HAVING COUNT(1) = 3
```

> Thời gian truy vấn: 0.312 s

> Truy vấn thứ 2:

```
SELECT t1.userid
FROM userrole t1
JOIN userrole t2 ON t1.userid = t2.userid AND t2.roleid = 2
JOIN userrole t3 ON t2.userid = t3.userid AND t3.roleid = 3
AND t1.roleid = 1
```

> Thời gian truy vấn: 0.016 s

> Điều đó là đúng. Bản jon tôi thấy **thời gian nhanh gấp 20 lần với bản tổng hợp.**

**6. Không làm đơn giản hoá các truy vấn phức tạp thông qua cách xem**

Không phải tất cả các loại cơ sở dữ liệu đều hỗ trợ hiển thị nhưng đối với những người làm, họ có thể đơ giản hoá nhiều truy vấn nếu họ dử dụng một cách cẩn trọng. Ví dụ trong một dự án tôi đã sử dụng một [generic Party model](http://www.tdan.com/view-articles/5014/) cho CRM. Đây là một mô hình kỹ thuật rất mạnh và linh hoạt nhưng có thể dẫn đến nhiều phép hợp. Trong mô hình này có :

- **Party**: Con người và các tổ chức;
- **Party Role**: Những thứ mà các nhóm làm, ví dụ Nhân viên và Nhà tuyển dụng;
- **Party Role Relationship**: Các vai trò được liên quan đến vai trò khác thế nào.

Ví dụ:

- Ted là một Người, là một  Person, một tập con của Party;
- Ted có nhiều vai trò, một trong số đó là một Nhân Viên
- Intel là một tổ chức, là một tập con của Party;
- Intel có nhiều vai trò, một trong số đó là Nhà tuyển dụng;
- Intel thuê Ted, nghĩa là có một quan hệ giữa các vai trò tương ứng.

Vì vậy có 5 bảng tham gia vào liên kết Ted đến nhà tuyển dụng của anh ấy. Bạn giả sử tất cả nhân viên là Persons(Không phải tổ chức) và cung cấp các hiển thị hỗ trợ:

CREATE VIEW vw_employee AS
SELECT p.title, p.given_names, p.surname, p.date_of_birth, p2.party_name employer_name
FROM person p
JOIN party py ON py.id = p.id
JOIN party_role child ON p.id = child.party_id
JOIN party_role_relationship prr ON child.id = prr.child_id AND prr.type = 'EMPLOYMENT'
JOIN party_role parent ON parent.id = prr.parent_id = parent.id
JOIN party p2 ON parent.party_id = p2.id

Và bất ngời bạn có một cái nhìn về dữ liệu rất đơn giản mà bạn muốn nhưng trên một mô hình dữ liệu có tính linh hoạt cao.

**7. Không loại bỏ đầu vào**

Đây là một vấn đề lớn. Bây giờ tôi thích PHp nhưng nếu bạn không biết làm gì thì dễ dàng tạo ra một trang web dễ bị tấn công. Không có gì tổng tkeets hơn hơn là [câu chuyện của little Bobby Tables](http://xkcd.com/327/).

Dữ liệu được cung cấp bởi người dùng thông qua các URL, dữ liệu form và **cookies** nên luôn luôn được coi là kẻ thù và xử lý chúng. Đảm bảo bạn nhận được những gì bạn mong đợi.

**8. Không sử dụng các câu lệnh trước**

Các câu lệnh xử lsy là khi bạn biên dịch một câu truy vấn bớt dữ liệu được sử dụng trong chèn, cập nhật và mệnh đề WHERE và sau đó cung cấp sau. 
Ví dụ:

`SELECT * FROM users WHERE username = 'bob' `

với

`SELECT * FROM users WHERE username = ?`

hoặc

`SELECT * FROM users WHERE username = :username`
tuỳ thuộc vào nền tảng.

Tối thấy cơ sở dữ liệu đó mang đến **their knees** bằng cách làm điều này. Về cơ bản, mỗi lần bất kỳ cơ sở dữ liệu hiện đại gặp một câu truy vấn mới, nó phải biên dịch nó. Nếu nó gặp một câu truy vấn trước đó, nó đưa đến cơ sở dữ liệu câu truy vấn được cache và thực hiện nó. Bằng cách thực hiện các câu truy vấn nhiều lần, nó đem đến cơ sở dữ liệu cơ hội để ra các con số và tối ưu nó phù hợp (ví dụ, bằng cách dữ các câu truy vấn đã được biên dịch trong bộ nhớ).

Sử dụng các câu lệnh chuẩn bị cũng sẽ đem đến cho bạn những số liệu thống kê có ý nghĩa về số lần câu truy vấn được sử dụng.

Các câu lệnh chuẩn bị cũng sẽ bảo vệ tốt hơn chống lại các cuộc tấn công SQL injection.

**9. Không chuẩn hoá đủ**

[Database normalization](http://en.wikipedia.org/wiki/Database_normalization) về cơ bản là quá trình tối ưu hoá thiết kế cơ sở dữ liệu hoặc làm thế nào để tổ chức dữ liệu của bạn vào các bảng.

Trong tuần này tôi bắt gặp một vài mã code của ai đó đã tách 1 mảng và thêm nó vào trong một trường của cơ sở dữ liệu. Chuẩn hoá là việc coi các phần tử của một mảng như là một dòng dữ liệu riêng biệt trong một bảng con (ví dụ quan hệ một-nhiều).

Điều này cũng được nhắc tới trong [Phương thức tốt nhất cho lưu trữ một danh sách các ID của người dùng](https://stackoverflow.com/questions/620645/best-method-for-storing-a-list-of-user-ids):

> Tôi đã nhìn thấy trong các hệ thống khác danh sách được lưu trong một mảng PHP tuần tự.

Nhưng sự thiếu sót chuẩn hoá còn đến từ nhiều mẫu.

Xem thêm:

- [Chuẩn hoá: Bao nhiêu thì đủ?](http://www.techrepublic.com/article/normalization-how-far-is-far-enough/)
- [SQL by Design: Tại sao bạn cẩn chuẩn hoá cơ sở dữ liệu ](http://www.sqlmag.com/Article/ArticleID/4887/sql_server_4887.html)

**10. Chuẩn hoá quá nhiều**

Điều này có thể thấy mâu thuẫn với luận điểm trước, nhưng chuẩn hoá giống như là nhiều thứ, nó chỉ là một công cụ. Nó không có một điểm dừng rõ ràng cho chính nó. Tôi nghĩ nhiều developer quên mất điều này và bắt đầu "means" giống như một "end". Unit testing là một ví dụ điển hình cho điều này.

Tôi đã từng làm việc trên một hệ thống mà có hệ thống phân cấp rõ ràng cho các khách hàng nó giống như:

Licensee -&gt;  Dealer Group -&gt; Company -&gt; Practice -&gt; ...

mỗi điều đó bạn phải join khoảng 11 bảng với nhau trước khi bạn có thể lấy được dữ liệu hữu ích. Nó là một ví dụ tốt về sự chuẩn hoá tốn quá nhiều công.

Thêm vào đó, việc chuẩn hoá lại cẩn thận và lưu tâm có thể có lợi ích hiệu năng lớn nhưng bạn phải thực sự cẩn thận khi làm điều đó.

Xem thêm:

- [Tại sao chuẩn hoá cơ sở dữ liệu quá nhiều có thể là một thứ tồi tệ ?](http://www.selikoff.net/blog/2008/11/19/why-too-much-database-normalization-can-be-a-bad-thing/)
- [Chuẩn hoá trong thiết kế cơ sở dữ liệu nhiều như thế nào ?](https://stackoverflow.com/questions/496508/how-far-to-take-normalization-in-database-design)
- [Khi nào không chuẩn hoá trong cơ sở dữ liệu SQL của bạn ?](http://www.25hoursaday.com/weblog/CommentView.aspx?guid=cc0e740c-a828-4b9d-b244-4ee96e2fad4b)
- [Có thể chuẩn hoá là không bình thường ?](http://www.codinghorror.com/blog/archives/001152.html)
- [The Mother of All Database Normalization Debates on Coding Horror](http://highscalability.com/mother-all-database-normalization-debates-coding-horror)

**11. Sử dụng exclusive arcs**

Một exclusive arc là một sai lầm phổ biến ở một bảng được tạo bởi hai hoặc nhiều khoá ngoài tại đó một hoặc chỉ một trong số chúng có thể không null.  **Sai lầm lớn.** Đối với cái mà nó trở nên khó khăn hơn để duy trì tính toàn vẹn của dữ liệu. Sau tất cả điều đó, ngay cả với tính toàn vẹn tham chiếu, không có gì ngăn được hai hay nhiều khoá ngoại này được thiết lập(mặc dù phức tạp để kiểm tra những ràng buộc). 

Từ [Hướng dẫn thực hành thiết kế cơ sở dữ liệu quan hệ](http://books.google.com.au/books?id=7ZAk0YiKQV0C&pg=PA110&lpg=PA110&dq=%22exclusive+arc%22+database&source=bl&ots=AyNPWsac__&sig=gBFIerXckQlVpRdd6ycI5JEgq3U&hl=en&ei=PzGzSZfrFcPVkAWWyZDZBA&sa=X&oi=book_result&resnum=1&ct=result):

> Chúng ta đã khuyên nhiều tránh xây dựng exclusive arc bất cứ khi nào có thể, vì một lý do hợp lý rằng chúng có thể khó khăn trong viết code và bảo trì.

**12. Không phân tích hiệu năng dựa trên tất cả câu truy vấn**

Pragmatism reigns supreme, particularly in the database world. Nếu bạn đang cố theo nguyên tắc đến mức chúng trở thành một niềm tin thì có thể bạn đã mắc sai lầm. Lấy ví dụ về truy vấn tổng hợp bên trên. Phiên bản tổng hợp có thể trông "đẹp" nhưng hiệu năng của của nó là không tốt. Một sự so sánh hiệu năng nên được kết thúc trong buổi tranh luận (Nhưng nó không), nhưng quan trọng hơn việc phát hiện ra những quan điểm đầu tiên là không biết gì, thậm chí là nguy hiểm.

**13. Quá phụ thuộc dựa trên UNION ALL và đặc biệt cấu trúc UNION**

Một thuật ngữ UNION trong SQL chỉ sự ghép nối giữa các tập dữ liệu, nghĩa là chúng có cột loại hoặc số giống nhau. Sự khác nhhau giữa chúng là UNION ALL là một phép nối đơn giản và nên được ưu tiên hơn bất cứ chỗ nào có thể trong khi một UNION sẽ bản chất là thực hiện một DISTINCT để loại bỏ các bản dữ liệu trùng lặp.

Các UNION, giống như DISTINCT, có vị trí riêng của chúng. Đều có các ứng dụng hợp lệ. Nhưng nếu bạn tìm thấy chính bạn đang thực hiện chúng quá nhiều, đặc biệt trong các truy vấn phụ, thì bạn có thể đang làm sai. Đó có thể là một trường hợp xây dựng câu truy vấn tốn kém hoặc mô hình dữ liệu được thiết kế kém buộc bạn phải làm như bậy.

Các UNION, đặc biệt khi được dùng trong phép nối hoặc các truy vấn phụ thuộc, có thể làm treo một cơ sở dữ liệu. Cố gắng tránh chúng bất cứ khi nào có thể.

**14. Sử dụng điều kiện OR trong các câu truy vấn**

Điều này có vẻ vô hại. Cuối cùng các AND chính là OR. OR nên Thế OR có thực sự đúng không ? Về cơ bản một điều kiện AND **hạn chế** bộ dữ liệu trong khi một điều kiện OR **làm tăng** tập dữ liệu đó nhưng không là một cách được tối ưu. Đặc biệt khi các điều kiện OR khác nhau có thể giao nhau bắt buộc phải tối ưu để hiệu quả hơn thông qua kết quả của một DISTINCT.

Xấu:

... WHERE a = 2 OR a = 5 OR a = 11

Tốt hơn:

... WHERE a IN (2, 5, 11)

Bây giờ tối ưu SQL của bạn có thể hiệu quả từ câu truy vấn đầu tiên tới câu thứ hai. Nhưng nó không thể. Đừng làm điều đó.

**15. Không thiết kết mô hình dữ liệu để lấy giải pháp hiệu năng cao**

Đây là một điểm khó để ước lượng. Nó thuường được đánh giá bởi hiệu quả của chính nó. Nếu bạn thấy chính bạn đang viết các câu truy vấn cho những nhiệm vụ đơn giản hoặc các câu truy vấn đó để tìm thông tin của một đối tượng đơn giản là không hiệu quả, bạn chắc chắn đang sử dụng một mô hình dữ liệu tồi tệ.

Trong một số cách điểm này tổng kết lại tất cả những thứ trước đó nhưng nó lại có một câu cảnh báo về những thứ đang làm đó giống như được tối ưu đầu tiên trong khi nên được làm thứ hai. Đầu tiên và cũng là quan trọng nhất bạn nên chắc chắn bạn có một mô hình dữ liệu tốt trước khi thử tôtis ưu hiệu năng . Giống như Knuth nói:

> Tối ưu sớm là gốc rễ của mọi sự sai lầm.

**16. Sử dụng sai Database Transactions**

Tất cả thay đổi dữ liệu cho một tiến trình đặc biệt theo mô hình nhóm nguyên tử. Nếu thực hiện thành công, nó sẽ thực hiện đầy đủ. Nếu nó thất bại, dữ liệu không thay đổi. Không có những thay đổi ' nửa hoàn thành'.

Lý tưởng nhất, các đơn giản nhất để đạt được điều này là thiết toàn bộ hệ thống nên cố hỗ trợ tất cả sự thay đổi dữ liệu thông qua các câu lệnh đơn INSERT/UPDATE/DELETE . Trong trường hợp này, không cần xử lý giao dịch đặt biệt, một cơ sở dữ liệu nên được làm điều đó tự động.ne should do so automatically.

Tuy nhiên nếu các tiến trình yêu cầu nhiều câu lệnh hiệu như là giữ một đơn vị dữ liệu trong một trạng thái thích hợp, khi đó việc quản lsy các Giao dịch à cần thiết.

- Bắt đầu một Transaction trước câu lệnh đầu tiên.
- Commit Transaction sau câu lệnh cuối cùng.
- Khi gặp bất kỳ lỗi nào, Rollback the Transaction. Và chú ý không quên skip/abort tất cả câu lệnh mà theo sau là lỗi.

Khuyến cáo nữa là chú ý đến các dịch vụ phụ của việc kế nối các lớp cơ sở dữ liệu và tương tác của cơ sở dữ liệu

**17. Không hiểu mô hình 'set-based'**

Ngôn ngữ SQL tuân theo mô hình đặc biệt phù hợp với các loại vấn đề. Mặc dù có nhiều phần mở rộng cung cấp cụ thể, việc xung đột để giải quyết vấn đề trong các ngôn ngữ như Java, C#, Delphi etc.

Còn lại được thể hiện quả 1 vài cách:

- Áp dụng quá nhiều phương pháp hoặc thực thi logic không phù hợp với cơ sở dữ liệu.
- Việc sử dụng không phù hợp hoặc quá mức các con trỏ. Đặc biệt khi một câu truy vấn đơn đầy đủ.
- Một giả định không chính xác rằng các trigger sẽ thực hiện trên mỗi dùng trong khi cập nhật trên nhiều dòng.

Xác định phân chia các nhiệm vụ rõ ràng, và cố gắng sử dụng công cụ phù hợp để giải quyết từng vấn đề.
