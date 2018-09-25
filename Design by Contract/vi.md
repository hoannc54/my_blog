# Design by contract

Design by contract (DbC), cũng được biết đến như contract programming, programming by contract và design by contract programming, là một cách tiếp cận trong thiết kế phần mềm. Nó quy định rằng các nhà thiết kế phần mềm nên xác định trước formal, các đặc tả giao diện chính xác và có thể kiểm chứng được cho các thành phần phần mềm, mở rộng định nghĩa thông thường của các kiểu dữ liệu trừu tượng với các điều kiện tiên quyết, điều kiện sau và các bất biến. Những đặc điểm này được gọi là "contracts", phù hợp với nghĩa bóng với điều kiện và nghĩa vụ trong hợp đồng kinh doanh.

Cách tiếp cận DbC giả sửa trong tất cả các thành phần client mà gọi một hoạt động trên một thành phần máy chủ sẽ đáp ứng các điều kiện bạn đầu được chỉ định yêu cầu cho hoạt động đó. Giả định này được coi là quá rủi ro (như trong mô hình client-server đa kênh hoặc tính toán phân tán) cách tiếp cận đối mặt "defensive design" được thực hiện, nghĩa là một thành phần server thử nghiệm (trước hoặc trong quá trình thực hiện request client) tất cả các điều kiện trước có liên quan đều đúng và trả lời với thông báo lỗi phù hợp nếu không đúng.

## Lịch sử 

Thuật ngữ được đặt ra bởi Bertrand Meyer liên quan đến thiết kế ngôn ngữ lập trình Eiffel của anh ấy và được mô tả đầu tiên trong các bài viết khác nhau từ năm 1986 và hai ấn bản liên tiếp (1988, 1997) trong quyển sách Object-Oriented Software Construction của ông ấy. Eiffel Software đăng ký nhãn hiệu cho Design by Contract vào tháng 10, 2003 và nó đã được cấp vào tháng 10, 2004. Chủ sở hữu hiện tại của nhãn hiệu này là Eiffel Software.

Design by contract có nguồn gốc của nó trong công việc  formal verification, formal specification và Hoare logic. Những đóng góp ban đầu bao gồm: 

* Một ẩn dụ rõ ràng để hướng dẫn quá trình thiết kế
* Các ứng dụng để thừa kế, đặc biệt llaf một hình thức cho redefinition và dynamic binding.
* Ứng dụng để xử lý exception
* Kết nối tự động với tài liệu phần mềm

## Mô tả 

Ý tưởng chính của DbC là phép ẩn cách các thành phần của hệ thống phần mềm tương tác với nhau trên cơ sở nhiệm vụ và lợi ích với nhau.Phép ẩn dụ này xuất phát thực thực tế cuộc sống kinh doanh, trong trường hợp "khách hàng" và "nhà cung cấp" đồng ý về "hợp đồng" đã xác định ví dụ :
* Nhà cung cấp phải cung cấp một sản phẩm nhất định (nghĩa vụ) và và được quyền mong đợi rằng khách hàng đã trả phí (lợi ích) của mình.
* Khách hàng phải trả phí (nghĩa vụ) và được quyền nhận sản phẩm (lợi ích).
* Cả hai bên phải đáp ứng các nghĩa vụ nhất định, chẳng hạn như các luật và các quy định, áp dụng cho tất cả các hợp đồng.

Tương tự, nếu một routine từ một [lớp] [26] trong [lập trình hướng đối tượng] [27] cung cấp một chức năng nhất định, nó có thể:
* Mong đợi một điều kiện nhất định được đảm bảo khi được nhập vào bởi bất kỳ module client nào gọi nó: [điều kiện tiên quyết] [6] - nghĩa vụ của khách hàng, và một lợi ích cho nhà cung cấp (chính thủ tục), vì nó giải phóng nó khỏi việc phải xử lý các trường hợp ngoài điều kiện tiên quyết.
* Đảm bảo một tài sản nhất định khi thoát: [điều kiện sau] [7] - nghĩa vụ của nhà cung cấp, và rõ ràng là một lợi ích (lợi ích chính của việc gọi thủ tục) cho khách hàng.
* Bảo trì một property nhất định, giả định về đầu vào và được bảo đảm khi thoát ra: [bất biến lớp] [28].

Hợp đồng tương đương về mặt ngữ nghĩa với [Hoare triple] [29], hình thành các nghĩa vụ.Điều này có thể được tóm tắt bằng "ba câu hỏi" mà nhà thiết kế phải nhiều lần trả lời trong hợp đồng:

* Mong muốn của hợp đồng là gì?
* Điều gì đảm bảo cho hợp đồng ?
* Điều gì để duy trì hợp đồng ?
Nhiều [ngôn ngữ lập trình] [30] dễ dàng để tạo [xác nhận] [31] như thế. Tuy nhiên, DbC coi các hợp đồng này là rất quan trọng đối với [tính chính xác phần mềm] [32] rằng chúng phải là một phần của quá trình thiết kế. Thực tế, DbC chủ trương viết các xác nhận trước. Hợp đồng có thể được viết bởi [code comments] [33], được thi hành bởi [test suite] [34], hoặc cả hai, ngay cả khi không có hỗ trợ ngôn ngữ đặc biệt cho các hợp đồng.

Khái niệm về hợp đồng kéo dài xuống mức phương thức / thủ tục; hợp đồng cho mỗi phương thức thường sẽ chứa các thông tin sau [[_citation needed][35]_]
* Các giá trị hoặc loại đầu vào được chấp nhận và không được chấp nhận và ý nghĩa của chúng
* Trả lại giá trị hoặc loại và ý nghĩa của chúng
* Lỗi và [exception] [36] các giá trị hoặc loại điều kiện có thể xảy ra và ý nghĩa của chúng
* [Hiệu ứng phụ][37]
* [Điều kiện tiên quyết][6]
* [Điều kiện sau][7]
* [Tính bất biến][8]
* (một số ít) Đảm bảo hiệu suất, ví dụ: cho thời gian hoặc không gian được sử dụng

Các lớp con trong [phân cấp thừa kế] [38] được phép làm giảm các điều kiện tiên quyết (nhưng không được tăng chúng) và tăng cường điều kiện sau và bất biến (nhưng không làm giảm chúng). Các quy tắc này gần đúng [subtyping behaviory] [39].

Tất cả các quan hệ class là giữa các lớp khách hàng và các lớp nhà cung cấp. Lớp khách hàng có nghĩa vụ thực hiện các lời gọi đến các tính năng của nhà cung cấp, nơi trạng thái kết quả của nhà cung cấp không bị vi phạm bởi lời gọi của khách hàng. 
Sau đó, nhà cung cấp có nghĩa vụ cung cấp trạng thái trả về và dữ liệu không vi phạm các yêu cầu trạng thái của khách hàng. Ví dụ, một bộ đệm dữ liệu của nhà cung cấp có thể yêu cầu dữ liệu đó có trong bộ đệm khi một tính năng xóa được gọi. Sau đó, nhà cung cấp đảm bảo cho khách hàng rằng khi một tính năng xóa kết thúc công việc của nó, phần dữ liệu sẽ thực sự bị xóa khỏi bộ đệm. Các hợp đồng thiết kế khác là các khái niệm về "bất biến lớp". Lớp bảo đảm bất biến (đối với lớp cục bộ) rằng trạng thái của lớp sẽ được duy trì trong các mức chịu được chỉ định ở cuối mỗi thực thi tính năng.

Khi sử dụng hợp đồng, nhà cung cấp không nên cố gắng xác minh rằng các điều kiện hợp đồng được thỏa mãn,. ý tưởng chung là code nên "thất bại", với việc xác minh hợp đồng là quá trình an toàn. Thuộc tính "thất bại" của DbC đơn giản là việc gỡ rối hành vi hợp đồng, vì hành vi dự định của mỗi routine được xác định rõ ràng. Điều này phân biệt nó một cách rõ rệt từ một thực hành liên quan được gọi là [lập trình phòng thủ] [40], nơi nhà cung cấp chịu trách nhiệm tìm ra những việc cần làm khi điều kiện tiên quyết bị phá vỡ. Thường xuyên hơn không, nhà cung cấp ném một exception để thông báo cho khách hàng rằng điều kiện tiên quyết đã bị phá vỡ và trong cả hai trường hợp — DbC và lập trình phòng thủ — khách hàng phải tìm ra cách để đáp ứng điều đó. DbC giúp công việc của nhà cung cấp dễ dàng hơn.

Thiết kế theo hợp đồng cũng xác định tiêu chí cho tính chính xác cho một module phần mềm:

* Nếu lớp bất biến và điều kiện tiên quyết là đúng trước khi một nhà cung cấp được gọi bởi một client, thì sự bất biến và điều kiện sau sẽ là đúng sau khi dịch vụ đã được hoàn thành
* Khi thực hiện lời gọi đến nhà cung cấp, module phần mềm không được vi phạm các điều kiện tiên quyết của nhà cung cấp.

Design by contract cũng có thể tạo điều kiện tái sử dụng code, vì hợp đồng cho từng đoạn code được ghi đầy đủ. Các hợp đồng cho một module có thể được coi là một dạng [tài liệu phần mềm] [41] cho hành vi của module đó.

## Ý nghĩa hiệu suất[[edit][42]]

Các điều kiện hợp đồng sẽ không bao giờ bị vi phạm trong khi thực hiện chương trình không có lỗi. Các hợp đồng do đó thường chỉ được kiểm tra trong chế độ gỡ lỗi trong quá trình phát triển phần mềm. Sau khi phát hành, kiểm tra hợp đồng bị vô hiệu hóa để tối đa hóa hiệu suất.

Trong nhiều ngôn ngữ lập trình, hợp đồng được thực hiện với [yêu cầu] [31]. Asserts được mặc định biên dịch trong chế độ phát hành trong C / C ++, và tương tự như bị vô hiệu hóa trong C # [[8]] [43] và Java. Điều này có hiệu quả loại bỏ các chi phí thời gian chạy của hợp đồng phát hành.

## Mối quan hệ với kiểm thử phần mềm[[edit][44]]

Design by contract không thay thế chiến lược kiểm tra thường xuyên, chẳng hạn như [unit testing] [45], [thử nghiệm tích hợp] [46] và [thử nghiệm hệ thống] [47]. Thay vào đó, nó bổ sung cho kiểm thử bên ngoài với các phép kiểm thử bên trong có thể được kích hoạt cho cả các kiểm thử riêng biệt và trong code production trong giai đoạn kiểm thử. Lợi thế của tự kiểm thử nội bộ là họ có thể phát hiện lỗi trước khi chúng biểu hiện chính họ như là kết quả không hợp lệ được khách hàng quan sát. Điều này dẫn đến phát hiện lỗi sớm hơn và cụ thể hơn.

Việc sử dụng các xác nhận có thể được coi là một dạng [kiểm thử oracle] [48], một cách để kiểm tra thực hiện hdesign by contract 

## Ngôn ngữ hỗ trợ[[edit][49]]

### Ngôn ngữ gốc có hỗ trợ[[edit][50]]

Ngôn ngữ thực hiện hầu hết các tính năng của DbC bao gồm:

.....

### Languages with third-party support[[edit][51]]

Các thư viện, bộ tiền xử lý và các công cụ khác đã được phát triển cho các ngôn ngữ lập trình hiện tại mà không có sự hỗ trợ của Design by Contrac:

.....

## Xem thêm[[edit][97]]

1. [**^][98]** Meyer, Bertrand: _Design by Contract_, Technical Report TR-EI-12/CO, Interactive Software Engineering Inc., 1986
2. [**^][99]** Meyer, Bertrand: _Design by Contract_, in _Advances in Object-Oriented Software Engineering_, eds. D. Mandrioli and B. Meyer, Prentice Hall, 1991, pp. 1–50
3. [**^][100]** Meyer, Bertrand: _Applying "Design by Contract"_, in Computer (IEEE), 25, 10, October 1992, pp. 40–51, also available [online][101]
4. [**^][102]** ["United States Patent and Trademark Office registration for "DESIGN BY CONTRACT""][103]. 
5. [**^][104]** ["United States Patent and Trademark Office registration for the graphic design with words "Design by Contract""][105]. 
6. [**^][106]** ["Trademark Status & Document Retrieval"][107]. _tarr.uspto.gov_. 
7. [**^][108]** ["Trademark Status & Document Retrieval"][109]. _tarr.uspto.gov_. 
8. [**^][110]** ["Assertions in Managed Code"][111]. _msdn.microsoft.com_. 
9. [**^][112]** Bright, Walter (2014-11-01). ["D Programming Language, Contract Programming"][113]. Digital Mars. Retrieved 2014-11-10. 
10. [**^][114]** Hodges, Nick. ["Write Cleaner, Higher Quality Code with Class Contracts in Delphi Prism"][115]. Embarcadero Technologies. Retrieved 20 January 2016. 
11. [**^][116]** Findler, Felleisen [Contracts for Higher-Order Functions][117]
12. [**^][118]** ["Code Contracts"][119]. _msdn.microsoft.com_. 
13. **[^][120]** ["Bean Validation specification"][121]. _beanvalidation.org_. 
14. **[^][122]** 
15. **[^][123]** ["Archived copy"][124] (PDF). Archived from [the original][125] (PDF) on 2016-03-28. Retrieved 2016-03-25.  p. 2
16. **[^][126]** ["No chance of releasing under Apache/Eclipse/MIT/BSD license? · Issue #5 · nhatminhle/cofoja"][127]. _GitHub_. 

## Bibliography[[edit][128]]

* Mitchell, Richard, and McKim, Jim: _Design by Contract: by example_, Addison-Wesley, 2002
* A [wikibook][129] describing DBC closely to the original model.
* McNeile, Ashley: _[A framework for the semantics of behavioral contracts_][130]. Proceedings of the Second International Workshop on Behaviour Modelling: Foundation and Applications (BM-FA '10). ACM, New York, NY, USA, 2010. This paper discusses generalized notions of **Contract** and **Substitutability**.

## External links[[edit][131]]

[1]: https://upload.wikimedia.org/wikipedia/commons/thumb/e/ea/Design_by_contract.svg/220px-Design_by_contract.svg.png
[2]: https://en.wikipedia.org/wiki/Software "Software"
[3]: https://en.wikipedia.org/wiki/Formal_methods "Formal methods"
[4]: https://en.wikipedia.org/wiki/Component-based_software_engineering#Software_component "Component-based software engineering"
[5]: https://en.wikipedia.org/wiki/Abstract_data_type "Abstract data type"
[6]: https://en.wikipedia.org/wiki/Precondition "Precondition"
[7]: https://en.wikipedia.org/wiki/Postcondition "Postcondition"
[8]: /wiki/Invariant_(computer_science) "Invariant (computer science)"
[9]: https://en.wikipedia.org/wiki/Conceptual_metaphor "Conceptual metaphor"
[10]: https://en.wikipedia.org/wiki/Defensive_design "Defensive design"
[11]: /w/index.php?title=Design_by_contract&action=edit§ion=1 "Edit section: History"
[12]: https://en.wikipedia.org/wiki/Bertrand_Meyer "Bertrand Meyer"
[13]: /wiki/Eiffel_(programming_language) "Eiffel (programming language)"
[14]: https://en.wikipedia.org#cite_note-1
[15]: https://en.wikipedia.org#cite_note-2
[16]: https://en.wikipedia.org#cite_note-3
[17]: https://en.wikipedia.org/wiki/Object-Oriented_Software_Construction "Object-Oriented Software Construction"
[18]: https://en.wikipedia.org#cite_note-DbC_tm_words-4
[19]: https://en.wikipedia.org#cite_note-DbC_tm_design-5
[20]: https://en.wikipedia.org#cite_note-DbC_tm_words2-6
[21]: https://en.wikipedia.org#cite_note-DbC_tm_design2-7
[22]: https://en.wikipedia.org/wiki/Formal_verification "Formal verification"
[23]: https://en.wikipedia.org/wiki/Formal_specification "Formal specification"
[24]: https://en.wikipedia.org/wiki/Hoare_logic "Hoare logic"
[25]: /w/index.php?title=Design_by_contract&action=edit§ion=2 "Edit section: Description"
[26]: /wiki/Class_(computer_programming) "Class (computer programming)"
[27]: https://en.wikipedia.org/wiki/Object-oriented_programming "Object-oriented programming"
[28]: https://en.wikipedia.org/wiki/Class_invariant "Class invariant"
[29]: https://en.wikipedia.org/wiki/Hoare_triple "Hoare triple"
[30]: https://en.wikipedia.org/wiki/Programming_language "Programming language"
[31]: /wiki/Assertion_(software_development) "Assertion (software development)"
[32]: /wiki/Correctness_(computer_science) "Correctness (computer science)"
[33]: /wiki/Comment_(computer_programming) "Comment (computer programming)"
[34]: https://en.wikipedia.org/wiki/Test_suite "Test suite"
[35]: https://en.wikipedia.org/wiki/Wikipedia%3ACitation_needed "Wikipedia:Citation needed"
[36]: https://en.wikipedia.org/wiki/Exception_handling "Exception handling"
[37]: /wiki/Side_effect_(computer_science) "Side effect (computer science)"
[38]: /wiki/Inheritance_(object-oriented_programming) "Inheritance (object-oriented programming)"
[39]: https://en.wikipedia.org/wiki/Liskov_substitution_principle "Liskov substitution principle"
[40]: https://en.wikipedia.org/wiki/Defensive_programming "Defensive programming"
[41]: https://en.wikipedia.org/wiki/Software_documentation "Software documentation"
[42]: /w/index.php?title=Design_by_contract&action=edit§ion=3 "Edit section: Performance implications"
[43]: https://en.wikipedia.org#cite_note-8
[44]: /w/index.php?title=Design_by_contract&action=edit§ion=4 "Edit section: Relationship to software testing"
[45]: https://en.wikipedia.org/wiki/Unit_testing "Unit testing"
[46]: https://en.wikipedia.org/wiki/Integration_testing "Integration testing"
[47]: https://en.wikipedia.org/wiki/System_testing "System testing"
[48]: https://en.wikipedia.org/wiki/Test_oracle "Test oracle"
[49]: /w/index.php?title=Design_by_contract&action=edit§ion=5 "Edit section: Language support"
[50]: /w/index.php?title=Design_by_contract&action=edit§ion=6 "Edit section: Languages with native support"
[51]: /w/index.php?title=Design_by_contract&action=edit§ion=7 "Edit section: Languages with third-party support"
[52]: /wiki/Ada_(programming_language) "Ada (programming language)"
[53]: https://en.wikipedia.org/wiki/GNAT "GNAT"
[54]: /wiki/C_(programming_language) "C (programming language)"
[55]: https://en.wikipedia.org/wiki/C%2B%2B "C++"
[56]: https://www.boost.org/doc/libs/master/libs/contract/doc/html/index.html
[57]: https://en.wikipedia.org/wiki/Preprocessor "Preprocessor"
[58]: /w/index.php?title=GNU_Nana&action=edit&redlink=1 "GNU Nana (page does not exist)"
[59]: https://en.wikipedia.org/wiki/Digital_Mars "Digital Mars"
[60]: /w/index.php?title=CTESK&action=edit&redlink=1 "CTESK (page does not exist)"
[61]: /wiki/Loki_(C%2B%2B) "Loki (C++)"
[62]: /wiki/C_Sharp_(programming_language) "C Sharp (programming language)"
[63]: https://en.wikipedia.org#cite_note-12
[64]: https://en.wikipedia.org/wiki/Microsoft_Research "Microsoft Research"
[65]: https://en.wikipedia.org/wiki/.NET_Framework ".NET Framework"
[66]: /wiki/Groovy_(programming_language) "Groovy (programming language)"
[67]: /wiki/Java_(programming_language) "Java (programming language)"
[68]: https://en.wikipedia.org/wiki/Jtest "Jtest"
[69]: https://en.wikipedia.org#cite_note-14
[70]: https://en.wikipedia.org/wiki/Spring_Framework "Spring Framework"
[71]: http://csd.informatik.uni-oldenburg.de/~jass/
[72]: http://modernjass.sourceforge.net
[73]: https://en.wikipedia.org#cite_note-15
[74]: https://en.wikipedia.org#cite_note-16
[75]: /w/index.php?title=JavaTESK&action=edit&redlink=1 "JavaTESK (page does not exist)"
[76]: https://en.wikipedia.org/wiki/JavaScript "JavaScript"
[77]: https://www.npmjs.com/package/dbc-code-contracts
[78]: https://en.wikipedia.org/wiki/Common_Lisp "Common Lisp"
[79]: https://en.wikipedia.org/wiki/Common_Lisp_Object_System "Common Lisp Object System"
[80]: https://en.wikipedia.org/wiki/Metaobject "Metaobject"
[81]: https://en.wikipedia.org/wiki/Nemerle "Nemerle"
[82]: /wiki/Nim_(programming_language) "Nim (programming language)"
[83]: https://github.com/Udiknedormin/NimContracts
[84]: https://en.wikipedia.org/wiki/Perl "Perl"
[85]: https://en.wikipedia.org/wiki/CPAN "CPAN"
[86]: https://en.wikipedia.org/wiki/Damian_Conway "Damian Conway"
[87]: /w/index.php?title=Raphael_Manfredi&action=edit&redlink=1 "Raphael Manfredi (page does not exist)"
[88]: https://en.wikipedia.org/wiki/PHP "PHP"
[89]: https://github.com/php-deal/framework
[90]: https://en.wikipedia.org/wiki/Praspel "Praspel"
[91]: /wiki/Python_(programming_language) "Python (programming language)"
[92]: /wiki/Ruby_(programming_language) "Ruby (programming language)"
[93]: /wiki/Rust_(programming_language) "Rust (programming language)"
[94]: https://crates.io/crates/hoare
[95]: https://en.wikipedia.org/wiki/Tcl "Tcl"
[96]: https://en.wikipedia.org/wiki/XOTcl "XOTcl"
[97]: /w/index.php?title=Design_by_contract&action=edit§ion=8 "Edit section: See also"
[98]: https://en.wikipedia.org#cite_ref-1
[99]: https://en.wikipedia.org#cite_ref-2
[100]: https://en.wikipedia.org#cite_ref-3
[101]: http://se.ethz.ch/~meyer/publications/computer/contract.pdf
[102]: https://en.wikipedia.org#cite_ref-DbC_tm_words_4-0
[103]: http://tess2.uspto.gov/bin/showfield?f=doc&state=4010:lsqmmo.2.2
[104]: https://en.wikipedia.org#cite_ref-DbC_tm_design_5-0
[105]: http://tess2.uspto.gov/bin/showfield?f=doc&state=4010:lsqmmo.2.1
[106]: https://en.wikipedia.org#cite_ref-DbC_tm_words2_6-0
[107]: http://tarr.uspto.gov/servlet/tarr?regser=serial&entry=78342277
[108]: https://en.wikipedia.org#cite_ref-DbC_tm_design2_7-0
[109]: http://tarr.uspto.gov/servlet/tarr?regser=serial&entry=78342308
[110]: https://en.wikipedia.org#cite_ref-8
[111]: https://msdn.microsoft.com/en-us/library/ttcc4x86.aspx
[112]: https://en.wikipedia.org#cite_ref-d-contract-programming_9-0
[113]: http://dlang.org/contracts.html
[114]: https://en.wikipedia.org#cite_ref-10
[115]: http://edn.embarcadero.com/article/39398
[116]: https://en.wikipedia.org#cite_ref-11
[117]: http://www.eecs.northwestern.edu/~robby/pubs/papers/ho-contracts-icfp2002.pdf
[118]: https://en.wikipedia.org#cite_ref-12
[119]: https://docs.microsoft.com/en-us/dotnet/framework/debug-trace-profile/code-contracts
[120]: https://en.wikipedia.org#cite_ref-13
[121]: http://beanvalidation.org/1.1/spec/
[122]: https://en.wikipedia.org#cite_ref-14
[123]: https://en.wikipedia.org#cite_ref-15
[124]: https://web.archive.org/web/20160328164727/http://cofoja.googlecode.com/files/cofoja-20110112.pdf
[125]: https://cofoja.googlecode.com/files/cofoja-20110112.pdf
[126]: https://en.wikipedia.org#cite_ref-16
[127]: https://github.com/nhatminhle/cofoja/issues/5
[128]: /w/index.php?title=Design_by_contract&action=edit§ion=10 "Edit section: Bibliography"
[129]: https://en.wikibooks.org/wiki/Computer_Programming/Design_by_Contract "b:Computer Programming/Design by Contract"
[130]: https://dx.doi.org/10.1145/1811147.1811150
[131]: /w/index.php?title=Design_by_contract&action=edit§ion=11 "Edit section: External links"

