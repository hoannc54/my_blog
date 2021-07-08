[Source](https://mariadb.com/kb/en/library/compound-composite-indexes/ "Permalink to Compound (Composite) Indexes - MariaDB Knowledge Base")

# Compound (Composite) Indexes - MariaDB Knowledge Base

## Một bài học nhỏ trong "compound indexes" ("composite indexes")

Đây là tài liệu ban đầu có vẻ là tầm thường và có lẽ nhàm chán, nhưng tạo nên nhiều thông tin thú vị, có lẽ những thứ bạn không nhận ra về cách làm việc index của MariaDB và MySQL.

Điều này cũng giải thích [lệnh Explain][1] (cho vài phạm vi nhất định)

(Hầu hết những điều này cũng áp dụng cho các nhánh cơ sở dữ liệu không phải MySQl)

## Thảo luận về câu truy vấn

Câu hỏi đặt ra là "Andrew Johnson làm tổng thống Mỹ khi nào?".

Có sẵn bảng `Presidents` giống như sau:
    
    
    +-----+------------+----------------+-----------+
    | seq | last_name  | first_name     | term      |
    +-----+------------+----------------+-----------+
    |   1 | Washington | George         | 1789-1797 |
    |   2 | Adams      | John           | 1797-1801 |
    ...
    |   7 | Jackson    | Andrew         | 1829-1837 |
    ...
    |  17 | Johnson    | Andrew         | 1865-1869 |
    ...
    |  36 | Johnson    | Lyndon B.      | 1963-1969 |
    ...
    

("Andrew Johnson" được lấy trong bài học vì chúng bị trùng lặp)

(Các) chỉ mục nào sẽ tốt nhất cho  câu hỏi trên? Cụ thể hơn, tốt nhất cho
    
    
        SELECT  term
            FROM  Presidents
            WHERE  last_name = 'Johnson'
              AND  first_name = 'Andrew';
    

Một số INDEXes để thử...

* No indexes 
* INDEX(first_name), INDEX(last_name) (two separate indexes) 
* "Index Merge Intersect" 
* INDEX(last_name, first_name) (a "compound" index) 
* INDEX(last_name, first_name, term) (a "covering" index) 
* Variants 

## Không indexes

Xem nào, tôi đang tào lao 1 chút ở đây. Tôi có một PRIMARY KEY là `seq`, nhưng cái đó không có lợi trong câu truy vấn mà chúng ta đang học.
    
    
    mysql>  SHOW CREATE TABLE Presidents G
    CREATE TABLE `presidents` (
      `seq` tinyint(3) unsigned NOT NULL AUTO_INCREMENT,
      `last_name` varchar(30) NOT NULL,
      `first_name` varchar(30) NOT NULL,
      `term` varchar(9) NOT NULL,
      PRIMARY KEY (`seq`)
    ) ENGINE=InnoDB AUTO_INCREMENT=45 DEFAULT CHARSET=utf8
    
    mysql>  EXPLAIN  SELECT  term
                FROM  Presidents
                WHERE  last_name = 'Johnson'
                  AND  first_name = 'Andrew';
    +----+-------------+------------+------+---------------+------+---------+------+------+-------------+
    | id | select_type | table      | type | possible_keys | key  | key_len | ref  | rows | Extra       |
    +----+-------------+------------+------+---------------+------+---------+------+------+-------------+
    |  1 | SIMPLE      | Presidents | ALL  | NULL          | NULL | NULL    | NULL |   44 | Using where |
    +----+-------------+------------+------+---------------+------+---------+------+------+-------------+
    
    # Or, using the other form of display:  EXPLAIN ... G
               id: 1
      select_type: SIMPLE
            table: Presidents
             type: ALL        <-- Implies table scan
    possible_keys: NULL
              key: NULL       <-- Implies that no index is useful, hence table scan
          key_len: NULL
              ref: NULL
             rows: 44         <-- That's about how many rows in the table, so table scan
            Extra: Using where
    

## Thực hiện chi tiết 

Đầu tiên, hãy mô tả cách mà InnoDB lưu trữ và dùng index.

* Dữ liệu và PRIMARY KEY  được "nhóm lại" cùng nhau trên BTree. 
* Một BTree tìm kiếm thì khá nhanh và hiệu quả. Đối với một triệu dòng trong bảng có thể có 3 cấp trong BTree, và trên 2 câp là có lẽ được cache. 
* Mỗi index phụ thì nằm trong một index khác BTree, với PRIMARY KEY ở lá.
* Nạp 'liên tục' (theo index) các phần tử từ một BTree thì rất hiệu quả vì chúng được lưu trữ liên tục.
* Với mục đích đơn giản, chúng ta có thể đếm mỗi tìm kiếm BTree như một đơn vị của công việc và bỏ qua quét các phần tử liên tiếp. Điều này xấp xỉ số lần truy cập ổ đĩa cho 1 bảng lớn trong một hệ thống bận rộn.

Với MyISAM, PRIMARY KEY thì không được lưu trữ với dữ liệu, vì vậy hãy coi nó như là khoá thứ cấp (quá đơn giản).

## INDEX(first_name), INDEX(last_name)

Người mới, một khi anh ấy học về index, quyết định index nhiều cột, mỗi cột một lần. Nhưng ...

MySQL hiếm khi sử dụng nhiều hơn một index trong 1 lần của 1 câu truy vấn, Vậy nên, nó sẽ phân tích các index khả thi.

* first_name -- có 2 dòng khả dụng (một BTree tìm kiếm, sau đó quét liên tục) 
* last_name -- có 2 dòng khả dụng. Giả sử nó chọn last_name. Đây là bước để thực hiện SELECT: 
1\. sử dụng INDEX(last_name), tìm 2 index đầu vào với last_name = 'Johnson'. 
2\. Lấy PRIMARY KEY (Được bổ sung cho mỗi index phụ trong InnoDB); lấy (17, 36). 
3\. Tiếp cận dữ liệu bằng cách dùng seq = (17, 36) để lấy các dòng cho  Andrew Johnson và Lyndon B. Johnson. 
4\. Sử dụng phần còn lại của mệnh đề WHERE lọc ra tất cả nhưng là dòng mong muốn. 
5\. Đưa ra câu trả lời (1865-1869). 
    
    ```
    mysql>  EXPLAIN  SELECT  term
                FROM  Presidents
                WHERE  last_name = 'Johnson'
                  AND  first_name = 'Andrew'  G
      select_type: SIMPLE
            table: Presidents
             type: ref
    possible_keys: last_name, first_name
              key: last_name
          key_len: 92                 <-- VARCHAR(30) utf8 may need 2+3*30 bytes
              ref: const
             rows: 2                  <-- Two 'Johnson's
            Extra: Using where
    ```

## "Index Merge Intersect"

OK, để bạn thực sự thông minh và quyết định rằng MySQL nên đủ thông mình để sủ dụng tên index để lấy câu trả lời. Đó được gọi là "Giao nhau".
1\. Sử dụng INDEX(last_name), tìm 2 index đầu vào với last_name = 'Johnson'; lấy (7, 17) 
2\. Sử dụng INDEX(first_name), find 2 index đầu vào với first_name = 'Andrew'; lấy (17, 36) 
3\. "And" 2 danh sách được liệt kê cùng nhau (7,17) & (17,36) = (17) 
4\. Tiếp cận dữ liệu bằng cách sử dụng seq = (17) để lấy dòng dữ liệu cho  Andrew Johnson. 
5\. Đưa ra câu trả lời (1865-1869).
    
    ```
               id: 1
      select_type: SIMPLE
            table: Presidents
             type: index_merge
    possible_keys: first_name,last_name
              key: first_name,last_name
          key_len: 92,92
              ref: NULL
             rows: 1
            Extra: Using intersect(first_name,last_name); Using where
    ```

EXPLAIN không thể cung cấp cho các chi tiết của số dòng dữ liệu được tập hợp từ mỗi index.

## INDEX(last_name, first_name)

Nó được gọi là một "trộn" hoặc "hỗn hợp" index vì nó có nhiều hơn một cột 
1\. Đi sâu vào BTree cho index để lấy các dòng index chính xác cho Johnson+Andrew; get seq = (17). 
2\. Tiếp cận dữ liệu bằng cách sử dụng seq = (17) để lấy dòng cho  Andrew Johnson. 
3\. Đưa ra câu trả lời (1865-1869). Thế này tốt hơn. Trong thực tế điều này thường là tốt nhất.
    
    ```
        ALTER TABLE Presidents
            (drop old indexes and...)
            ADD INDEX compound(last_name, first_name);
    
               id: 1
      select_type: SIMPLE
            table: Presidents
             type: ref
    possible_keys: compound
              key: compound
          key_len: 184             <-- The length of both fields
              ref: const,const     <-- The WHERE clause gave constants for both
             rows: 1               <-- Goodie!  It homed in on the one row.
            Extra: Using where
    ```

## "Covering": INDEX(last_name, first_name, term)

Ồ! chúng ta thực sự có thể làm tốt hơn một chút. Một "covering" index là một trong tất cả các trường của SELECT được tìm kiếm theo index. Nó được thêm không cần tiếp cận vào "dữ liệu" để kết thúc nhiệm vụ. 
1\. Đi sâu vào BTree cho index để lấy các dòng index chính xác của Johnson+Andrew; lấy seq = (17). 
2\. Đưa ra câu trả lời (1865-1869)."dữ liệu" BTree không được bắt, nó là một cải thiện dựa trên "compound".
    
    ```
        ... ADD INDEX covering(last_name, first_name, term);
    
               id: 1
      select_type: SIMPLE
            table: Presidents
             type: ref
    possible_keys: covering
              key: covering
          key_len: 184
              ref: const,const
             rows: 1
            Extra: Using where; Using index   <-- Note
    ```

Mọi thứ đều tương tự như sử dụng "compound", ngoại trừ việc thêm "Using index".

## Các biến thể

* Cái gì sẽ xảy ra nếu bạn xáo trộn các trường trong mệnh đề WHERE? Câu trả lời: Thứ tự của những phép AND thì không quan trọng. 
* Cái gì xảy ra nếu bạn xáo trộn các INDEX? Câu trả lời: Nó có thể tạo ra sự khác biệt lớn. Nhiều hơn một phút. 
* Điều gì xảy ra nếu thêm các trường vào cuối ? Câu trả lời: 1 chút ảnh hưởng nhỏ, có thể rất nhiều cái tốtu. 
* Dự phòng? Đó là nếu bạn có cả: INDEX(a), INDEX(a,b)? Trả lời: Chi phí thừa cho INSERTs; nó hiếm khi hữu ích cho SELECTs. 
* Tiền tố? Đó là, INDEX(last_name(5). first_name(5)) Trả lời: Đừng bận tâm , nó hiếm khi giúp ích và thường làm hại. (Chit tiết trong chủ đề khác.) 

## Thêm ví dụ:

    
 
        INDEX(last, first)
        ... WHERE last = '...' -- good (even though `first` is unused)
        ... WHERE first = '...' -- index is useless
    
        INDEX(first, last), INDEX(last, first)
        ... WHERE first = '...' -- 1st index is used
        ... WHERE last = '...' -- 2nd index is used
        ... WHERE first = '...' AND last = '...' -- either could be used equally well
    
        INDEX(last, first)
        Both of these are handled by that one INDEX:
        ... WHERE last = '...'
        ... WHERE last = '...' AND first = '...'
    
        INDEX(last), INDEX(last, first)
        In light of the above example, don't bother including INDEX(last).
    
## Postlog

Refreshed -- Oct, 2012; more links -- Nov 2016
ILLDIRankAgel0+1whoissourceRankMore dataSummary reportDiagnosisDensity00n/a
