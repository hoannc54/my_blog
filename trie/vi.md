[Source](https://threads-iiith.quora.com/Tutorial-on-Trie-and-example-problems "Permalink to Tutorial on Trie and example problems - Threads @ IIIT Hyderabad")

# Hướng dẫn các vấn đề trên Trie(cây tiền tố) và ví dụ

Tôi sẽ viết trong bài viết này về Tries và khái niệm tổng quát được sử dụng trong các thao tác nhỏ của vấn đề. Chúng ta sẽ thấy 2-3 vấn đề mà Trie hữu ích.

Đầu tiên chúng ta sẽ xem trie là gì. Trie có thể lưu thông tin về keys/numbers/strings nhỏ gọn trọng một cây.
Trie bao gồm các nút, nơi mỗi node lưu trữ một ký tự hoặc một bit. Chúng ta có thể thêm một chuỗi hoặc số phù hợp.
Đây là một ví dụ trie về chuỗi:

![][1]

  
Nguồn: Wikipedia.

Nhưng chúng ta sẽ xử lý các con số ở đây, và đặc biệt trong các bit nhị phân. Chúng ta sẽ thấy khi chúng ta giải quyết các vấn đề.

**Vấn đề 1**: Cho một mảng các số nguyên, chúng ta phải tìm 2 phần tử mà chỉ dùng XOR.  
**Giải quyết:**  
Giả sử chúng ta có một kiểu dữ liệu có thể đáp ứng hai loại truy vấn:  
1\. Chèn một số X  
2\. Cho một số Y, tìm max XOR của Y vói tất cả số mà đã được thêm đến bây giờ.

Nếu chúng ta có cấu trức dữ liệu này, chúng ta sẽ thêm các số nguyên như chúng ta đã làm, và với truy vấn của loại thứ 2 chúng ta sẽ tìm max XOR.  
Trie là cấu trúc dữ liệu chúng ta sẽ sử dụng. Đầu tiên hãy xem làm thế nào chúng ta thêm phần tử vào trie.

![][2]

  
Vì vậy, chúng ta theo đường của số mà chúng ta phải chền, chúng tôi không phải vẽ lại đường dẫn hiện tại.

Chèn một ký có độ dài N mất O(N) đó là log2(MAX) trong đó MAX là số tối đa số được chèn trong trie, bởi vì có tối đa log2(MAX) bit nhị phân trong số.  
Bằng cách này, chúng ta lưu trữ tất cả dữ liệu về tất cả các số được chèn vào trie đến bây giờ.  
Bây giờ, cho truy vấn của loại 2:  
Giả sử số Y của chúng ta là b1,b2...bn, trong đó b1,b2.. là các bit nhị phân. Chúng ta bắt đầu từ b1. Bây giờ  cho XOR là maximum, chúng ta sẽ cố gặng tạo bit 1 quan trọng nhất sau khi dùng XOR. Vì vậy, nếu b1 là 0, chúng ta sẽ cần 1 và ngược lại. Trong trie, chúng ta đi đến bit được yêu cầu. Nếu tuỳ chọn tốt không được, chúng ta sẽ chuyển sang bên khác. Làm điều này cho i=1 đến n, chúng ta sẽ lấy maximum XOR khả thi.

![][3]

Truy vấn vượt quá log2(MAX).

**Vấn đề 2**: Cho một mảng các số nguyên, tìm mảng con với  maximum XOR.  
**Giải quyết:**  
Giả sử F(L,R) là XOR mảng con từ  L đên R.  
Ở đây chúng ta sử dụng property F(L,R)=F(1,R) XOR F(1,L-1). Làm thế nào?  
Giả sử mảng con với maximum XOR kết thúc đến vị trí i. Bây giờ, chúng ta cần tối đa F(L,i) ie. F(1,i) XOR F(1,L-1) trong đó L<=i. Giả sử, chúng ta đã thêm F(1,L-1) vào trie của chúng ta với mọi L<=i, thì đố là vấn đề 1
    
    
    ans=0
    pre=0
    Trie.insert(0)
    for i=1 to N:
        pre = pre XOR a[i]
        Trie.insert(pre)
        ans=max(ans, Trie.query(pre))
    print ans
    

Bạn có thể thử với vấn đề này ở đây: [ACM-ICPC Live Archive][4]

**Vấn đề 3**: Cho một mảng của các số nguyên dương, bạn phải hiển thị ra số của mảng con có XOR nhỏ hơn K.

**Giải quyết:**  
Điều này sử dụng các khái niệm chúng ta đã đã thấy cho đến bây giờ. Chúng ta sẽ vấn đi đến giống câu hỏi trước.  
Cho mỗi index i=1 to N, chúng ta có thể đếm số lượng các mảng con kết thúc ở vị trí thứ i thoả mãn điều kiện đã cho.  

    
    
    ans=0
    p=0
    for i=1 to N:
        q=p XOR A[i]
        ans += Trie.query(q,k)
        Trie.insert(q)
        p=q
    

  
query(q,k) trả về số lượng các số nguyên đã tồn tại vào cấu trúc mà khi XOR với q trả về một số nguyên nhỏ hơn k.  
Chúng ta so sánh các bit tương ứng của q và k, bắt đầu từ các bit quan trọng nhất. giả sử p và q là các bit tương ứng mà chúng ta đang xem xét.

Nếu q là 1 và p là 0, sau đó chúng ta thực hiện điều này:  

![][5]

Tương tự chúng ta có thể dễ dàng tìm ra 3 trường hợp khác (q=1,p=1), (q=0,p=1) and (q=0,p=1).

Vì vậy, chúng ta cần thay đổi cấu trúc của chúng ta ở đây, chúng tôi cũng giữ số các lá của node có thể truy cập từ nút hiện tại nếu tôi đi về bên trái và tương tự cho bên phải. Bởi vì, nếu không, sự phức tạp sẽ tăng thêm, nếu chúng ta duyệt qua toàn bộ cây lặp đi lặp lại. Chúng ta có thể làm điều này trong khi chèn các số vào cây rất dễ dàng.

Đây là vấn đề đã được thiết lập trong CodeCraft'14. Bạn có thể thực hành ở đây: [SPOJ.com - Problem SUBXOR][6]

Bây giờ, hãy nói về việc triển khai.  
Để thực hiện một trie trong C/CPP chún ta có thẻ giữ các node và trỏ bên trái và bên phải. Chúng ta có thể viết hàm đệ quy.  

    
    
    insert(root, num, level):
        if level==-1: return root
        x=level'th bit of num
        if x==1:
            if root->right is NULL: create root->right
            else: insert(root->right,num,level-1)
        else:
            if root->left is NULL: create root->left
            else: insert(root->left,num,level-1)
    

  
Đối với các truy vấn, chúng ta duyệt theo chiều rộng của cây.

Cập nhật:  
Vấn đề khác sử dụng Trie(yay! :P).  
[Problem - B - Codeforces][7]

**Vấn đề con**: Cho một nhóm các chuỗi không rỗng. Trong trò chơi hai người cùng xây dựng từ cùng nhau, khởi tạo từ là rỗng. Các người chơi chuyển lượt Trog bước đi của anh ta, người chơi phải thêm một chữ cái vào cuối của từ, từ kết quả phải là tiền tố của ít nhất một chuỗi trong nhóm, Một người chơi mất lượt nếu anh ta khổng thể di chuyện.

Chúng ta cần tìm người chơi nào (thứ nhất hoặc thứ 2) có khả năng thắng.

Vì vậy, ý tưởng ở đây là một lần nữa để xây dựng một trie của tất cả các chuỗi. Tại sao ? Bởi vì một lưu trữ thông tin với tất cả tiền tố.
Bây giờ chúng ta sẽ thử đánh giá cho mỗi node nếu người chơi đầu tiên có khả năng thắng hoặc không. Chúng ta có thể thực hiện điều này theo cách đệ quy, Cho a node v, cho mỗi node u sao cho u là con thực hiện của v, nếu người chơi đầu tiên có khả năng thua cho u, sau đó for node v người chơi đầu tiên có khả năng thắng.
Ví dụ, gỉa sử chúng ta có "abc", "abd", "acd".  
trie của chúng ta giống như kia:  

![][8]

  
All leaf nodes have winning strategy.

[1]: https://qph.fs.quoracdn.net/main-qimg-aea28d9cd34aaf2d5783f4cd04e5abbd
[2]: https://qph.fs.quoracdn.net/main-qimg-388217a1992f1b2aac51e9917aa76d9c
[3]: https://qph.fs.quoracdn.net/main-qimg-e5d624e2cd693d713840a30ca9aaa461
[4]: https://icpcarchive.ecs.baylor.edu/index.php?Itemid=8&category=345&option=com_onlinejudge&page=show_problem&problem=2683
[5]: https://qph.fs.quoracdn.net/main-qimg-f24ea5ecf11805e7bcd82a48bb9cad25
[6]: http://www.spoj.com/problems/SUBXOR
[7]: http://codeforces.com/contest/455/problem/B
[8]: https://qph.fs.quoracdn.net/main-qimg-f81def67dffcc9e95306d65b27daa2f7-c
