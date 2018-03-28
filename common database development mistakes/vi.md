## Những sai lầm phổ biến trong quá trình phát triển cơ sở dữ liệu bởi những người phát triển ứng dụng là gì ?

_source https://stackoverflow.com/questions/621884/database-development-mistakes-made-by-application-developers_

**1. Không sử dụng các chỉ mục thích hợp**

Đây là một điều tương đối dễ dàng nhưng nó vẫn xảy ra mọi lúc. Các khoá ngoài nên được đánh chỉ mục cho chúng. Nếu bạn sử dụng một trường trong WHERE bạn nên (có lẽ nên) có một chỉ mục trên nó. Các chỉ mục như vậy nên thuờng bao gồm nhiều cột dựa trên các truy vấn bạn cần thực hiện.

**2. Không sử dụng các ràng buộc cho tham chiếu**

Cơ sở dữ liệu của bạn có thể thay đổi ở đây nhưng nếu cơ sở dữ liệu của bạn hỗ trợ ~~referential integrity~~ **tham chiếu ràng buộc**-- nghĩa là tất cả khoá ngoài được đảm bảo trỏ đến một thực thể đã tồn tại bạn nên sử dụng nó.

Nó khá là một biến để thấy sự thất bại trên cơ sở dữ liệu MySQL. TÔi không tin MyISAM hỗ trợ nó. InnoDB làm được. Bạn sẽ tìm người mà sử dụng MyISAM hoặc những người đang sử dụng InnoDB nhưng không sử dụng nó mọi nơi.

Xem thêm ở đây:

- [Các ràng buộc NOT NULL và FOREIGN KEY quan trọng thế nào nếu bạn luôn luôn kiểm soát dữ liệu đầu vào với PHP ?](https://stackoverflow.com/questions/382309/how-important-are-constraints-like-not-null-and-foreign-key-if-ill-always-contr)
- [Các khoá ngoài có thực sự cần thiết trong một thiết kế cơ sở dữ liệu không?](https://stackoverflow.com/questions/18717/are-foreign-keys-really-necessary-in-a-database-design)
- [Các khoá ngoài có thực sự cần thiết trong một thiết kế cơ sở dữ liệu không?](http://www.diovo.com/2008/08/are-foreign-keys-really-necessary-in-a-database-design/)

**3. Sử dụng khoá chính tự nhiên thay vì khóa đại diện (kỹ thuật)**

Khoá tự nhiên là khoá dựa trên dữ liệu bên ngoài có nghĩa đó là (~~khó bên ngoài~~ (ostensibly) **có vẻ**)duy nhất. Các ví dụ phổ biến là các mã code của sản phẩm, các mã code với 2 trạng thái, mã số an sinh xã hội, và nhiều hơn nữa. Các khoá chính đại diện hoặc kỹ thuật là những khoá hoàn toàn không có ý nghĩa bên ngoài hệ thống. Chúng được phát minh ra hoàn toàn để xác định các thực thể và các trường tự động tăng (SQL Server, MySQL, các csdl khác) hoặc các chuỗi(nhất là trong Oracle).

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

Đây là một mẹo nhỏ để đảm bảo rằng không trả về những dòng dữ liệu trùng lặp và lấy các đối tượng trùng lặp. Bạn sẽ thỉnh thoảng thấy những người làm điều này tốt. Nếu bạn thấy chúng quá nhiều, thì đó thực sự là ~~một cờ~~ (real red flag) thật sự đáng cảnh báo. Không phải DISTINCT là xấu hoặc không có ứng dụng phù hợp. Nó ( về cả 2 cách tính) nhưng nó không là một đại diện hoặc là một cái ngăn chặn viết các câu truy vấn đúng.

Từ [Tại sao tôt ghét DISTINCT](http://weblogs.sqlteam.com/markc/archive/2008/11/11/60752.aspx):

> Theo ý kiến của tôti lúc mà mọi thứ bắt đầu trở nênn khó chịu là khi một nhà phát triển đang đang xây dựng một lượng truy vấn lớn, kết hợp các bảng với nhau, và bất ngờ anh ta nhận ra rằng có vẻ như anh ta đang lấy các bản ghi trùng lặp( thậm trí nhiều hơn) và phản ứng ngay lập tức của anh ta .. giải pháp của anh ta cho vấn đề này là sử dụng từ khoá DISTINCT và tất cả rắc rối đó được loại bỏ.

**5. Khuyến khích dùng phép hợp các tập**

Một lỗi phổ biến khác bởi nhà phát triển cơ sở dữ liệu ứng dụng là không nhận ra ~~các tập tốn chi phí hơn bao nhiêu. (mệnh đè GROUP BY) có thể được so sánh với phép hợp.~~  (Another common mistake by database application developers is to not realize how much more expensive aggregation (ie the GROUP BY clause) can be compared to joins.) **tập hợp tốn kém hơn nhiều so với kết nối.**

~~Để cho bạn một ý tưởng làm thế nào để phổ biến rộng rãi điều này, tôi đã viết một chủ đề nhiều lần ở đây và được hạ đánh giá rất nhiều cho nó~~ (To give you an idea of how widespread this is, I've written on this topic several times here and been downvoted a lot for it. For example:) **Để cho bạn thấy ý tưởng này phổ biến như thế nào, tôi đã viết nó trong chủ đề này một vài lần ở đây và đã bị vote down nhiều lần. Ví dụ như:**.   
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

Đây là một vấn đề lớn. Bây giờ tôi thích PHp nhưng nếu bạn không biết làm gì thì dễ dàng tạo ra một trang web dễ bị tấn công. Không có gì tổng ~~tkeets hơn~~ **kết tốt** hơn là [câu chuyện của little Bobby Tables](http://xkcd.com/327/).

Dữ liệu được cung cấp bởi người dùng thông qua các URL, dữ liệu form và **cookies** nên luôn luôn được coi là kẻ thù và xử lý chúng. Đảm bảo bạn nhận được những gì bạn mong đợi.

**8. Không sử dụng các câu lệnh trước**

Các câu lệnh xử lý là khi bạn biên dịch một câu truy vấn bớt dữ liệu được sử dụng trong chèn, cập nhật và mệnh đề WHERE và sau đó cung cấp sau. 
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

**9. Not normalizing enough**

[Database normalization](http://en.wikipedia.org/wiki/Database_normalization) is basically the process of optimizing database design or how you organize your data into tables.

Just this week I ran across some code where someone had imploded an array and inserted it into a single field in a database. Normalizing that would be to treat element of that array as a separate row in a child table (ie a one-to-many relationship).

This also came up in [Best method for storing a list of user IDs](https://stackoverflow.com/questions/620645/best-method-for-storing-a-list-of-user-ids):

> I've seen in other systems that the list is stored in a serialized PHP array.

But lack of normalization comes in many forms.

More:

- [Normalization: How far is far enough?](http://www.techrepublic.com/article/normalization-how-far-is-far-enough/)
- [SQL by Design: Why You Need Database Normalization ](http://www.sqlmag.com/Article/ArticleID/4887/sql_server_4887.html)

**10. Normalizing too much**

This may seem like a contradiction to the previous point but normalization, like many things, is a tool. It is a means to an end and not an end in and of itself. I think many developers forget this and start treating a "means" as an "end". Unit testing is a prime example of this.

I once worked on a system that had a huge hierarchy for clients that went something like:

Licensee -&gt;  Dealer Group -&gt; Company -&gt; Practice -&gt; ...

such that you had to join about 11 tables together before you could get any meaningful data. It was a good example of normalization taken too far.

More to the point, careful and considered denormalization can have huge performance benefits but you have to be really careful when doing this.

More:

- [Why too much Database Normalization can be a Bad Thing](http://www.selikoff.net/blog/2008/11/19/why-too-much-database-normalization-can-be-a-bad-thing/)
- [How far to take normalization in database design?](https://stackoverflow.com/questions/496508/how-far-to-take-normalization-in-database-design)
- [When Not to Normalize your SQL Database](http://www.25hoursaday.com/weblog/CommentView.aspx?guid=cc0e740c-a828-4b9d-b244-4ee96e2fad4b)
- [Maybe Normalizing Isn't Normal](http://www.codinghorror.com/blog/archives/001152.html)
- [The Mother of All Database Normalization Debates on Coding Horror](http://highscalability.com/mother-all-database-normalization-debates-coding-horror)

**11. Using exclusive arcs**

An exclusive arc is a common mistake where a table is created with two or more foreign keys where one and only one of them can be non-null.  **Big mistake.** For one thing it becomes that much harder to maintain data integrity. After all, even with referential integrity, nothing is preventing two or more of these foreign keys from being set (complex check constraints notwithstanding).

From [A Practical Guide to Relational Database Design](http://books.google.com.au/books?id=7ZAk0YiKQV0C&pg=PA110&lpg=PA110&dq=%22exclusive+arc%22+database&source=bl&ots=AyNPWsac__&sig=gBFIerXckQlVpRdd6ycI5JEgq3U&hl=en&ei=PzGzSZfrFcPVkAWWyZDZBA&sa=X&oi=book_result&resnum=1&ct=result):

> We have strongly advised against exclusive arc construction wherever possible, for the good reason that they can be awkward to write code and pose more maintenance difficulties.

**12. Not doing performance analysis on queries at all**

Pragmatism reigns supreme, particularly in the database world. If you're sticking to principles to the point that they've become a dogma then you've quite probably made mistakes. Take the example of the aggregate queries from above. The aggregate version might look "nice" but its performance is woeful. A performance comparison should've ended the debate (but it didn't) but more to the point: spouting such ill-informed views in the first place is ignorant, even dangerous.

**13. Over-reliance on UNION ALL and particularly UNION constructs**

A UNION in SQL terms merely concatenates congruent data sets, meaning they have the same type and number of columns. The difference between them is that UNION ALL is a simple concatenation and should be preferred wherever possible whereas a UNION will implicitly do a DISTINCT to remove duplicate tuples.

UNIONs, like DISTINCT, have their place. There are valid applications. But if you find yourself doing a lot of them, particularly in subqueries, then you're probably doing something wrong. That might be a case of poor query construction or a poorly designed data model forcing you to do such things.

UNIONs, particularly when used in joins or dependent subqueries, can cripple a database. Try to avoid them whenever possible.

**14. Using OR conditions in queries**

This might seem harmless. After all, ANDs are OK. OR should be OK too right? Wrong. Basically an AND condition **restricts** the data set whereas an OR condition **grows** it but not in a way that lends itself to optimisation. Particularly when the different OR conditions might intersect thus forcing the optimizer to effectively to a DISTINCT operation on the result.

Bad:

... WHERE a = 2 OR a = 5 OR a = 11

Better:

... WHERE a IN (2, 5, 11)

Now your SQL optimizer may effectively turn the first query into the second. But it might not. Just don't do it.

**15. Not designing their data model to lend itself to high-performing solutions**

This is a hard point to quantify. It is typically observed by its effect. If you find yourself writing gnarly queries for relatively simple tasks or that queries for finding out relatively straightforward information are not efficient, then you probably have a poor data model.

In some ways this point summarizes all the earlier ones but it's more of a cautionary tale that doing things like query optimisation is often done first when it should be done second. First and foremost you should ensure you have a good data model before trying to optimize the performance. As Knuth said:

> Premature optimization is the root of all evil

**16. Incorrect use of Database Transactions**

All data changes for a specific process should be atomic. I.e. If the operation succeeds, it does so fully. If it fails, the data is left unchanged. - There should be no possibility of 'half-done' changes.

Ideally, the simplest way to achieve this is that the entire system design should strive to support all data changes through single INSERT/UPDATE/DELETE statements. In this case, no special transaction handling is needed, as your database engine should do so automatically.

However, if any processes do require multiple statements be performed as a unit to keep the data in a consistent state, then appropriate Transaction Control is necessary.

- Begin a Transaction before the first statement.
- Commit the Transaction after the last statement.
- On any error, Rollback the Transaction. And very NB! Don't forget to skip/abort all statements that follow after the error.

Also recommended to pay careful attention to the subtelties of how your database connectivity layer, and database engine interact in this regard. 

**17. Not understanding the 'set-based' paradigm**

The SQL language follows a specific paradigm suited to specific kinds of problems. Various vendor-specific extensions notwithstanding, the language struggles to deal with problems that are trivial in langues like Java, C#, Delphi etc.

This lack of understanding manifests itself in a few ways.

- Inappropriately imposing too much procedural or imperative logic on the databse.
- Inappropriate or excessive use of cursors. Especially when a single query would suffice.
- Incorrectly assuming that triggers fire once per row affected in multi-row updates.

Determine clear division of responsibility, and strive to use the appropriate tool to solve each problem.
