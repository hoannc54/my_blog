# Design by contract

Design by contract (DbC), cũng được biết đến như contract programming, programming by contract và design by contract programming, là một cách tiếp cận trong thiết kế phần mềm. Nó quy định rằng các nhà thiết kế phần mềm nên xác định trước formal, precise and verifiable interface specifications for software components, which extend the ordinary definition of abstract data types with preconditions, postconditions and invariants. Những đặc điểm này được gọi là "contracts", phù hợp với nghĩa bóng với điều kiện và nghĩa vụ trong hợp đồng kinh doanh.

Cách tiếp cận DbC giả sửa trong tất cả các thành phần client mà gọi một hoạt động trên một thành phần máy chủ sẽ đáp ứng các điều kiện bạn đầu được chỉ định yêu cầu cho hoạt động đó. Giả định này được coi là quá rủi ro (như trong mô hình client-server đa kênh hoặc tính toán phân tán) cách tiếp cận đối mặt "defensive design" được thực hiện, nghĩa là một thành phần server thử nghiệm (trước hoặc trong quá trình thực hiện request client) tất cả các điều kiện trước có liên quan đều đúng và trả lời với thông báo lỗi phù hợp nếu không đúng.

## Lịch sử 

Thuật ngữ được đặt ra bởi Bertrand Meyer liên quan đến thiết kế ngôn ngữ lập trình Eiffel của anh ấy và được mô tả đầu tiên trong các bài viết khác nhau từ năm 1986 và hai ấn bản liên tiếp (1988, 1997) trong quyển sách Object-Oriented Software Construction của ông ấy. Eiffel Software đăng ký nhãn hiệu cho Design by Contract vào tháng 10, 2003 và nó đã được cấp vào tháng 10, 2004. Chủ sở hữu hiện tại của nhãn hiệu này là Eiffel Software.

Design by contract có nguồn gốc của nó trong công việc  formal verification, formal specification và Hoare logic. Những đóng góp ban đầu bao gồm: 

* Một ẩn dụ rõ ràng để hướng dẫn quá trình thiết kế
* Các ứng dụng để thừa kế, đặc biệt llaf một hình thức cho redefinition và dynamic binding.
* Ứng dụng để xử lý exception
* Kết nối với tài liệu phần mềm tự động

## Mô tả 

Ý tưởng chính của DbC là 
