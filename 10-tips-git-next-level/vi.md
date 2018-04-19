#10 Lời khuyên giúp nâng kỹ năng Git của bạn lên tầm cao mới

Gần đây chúng tôi đã xuất bản một vài hướng dẫn giúp bạn làm quen với Git cơ bản và sử dụng Git trong môi trường một nhóm. Các câu lệnh mà chúng ta đã thảo luận về chúng đủ để giúp một developer tồn tại trong thế giới của Git. Trong bài viết này, tôi sẽ thử đưa ra làm thế nào bạn có thể quản lý thời gian một cách hiệu quả và tận dụng các tính năng mà Git cung cấp.

Chú ý: Một số lệnh trong bài viết này bao gồm một phần của lệnh trong dấu ngoặc vuông (ví dụ: git add -p [file_name]). Trong những ví dụ này, bạn nên thêm một số, định danh cần thiết, không có dấu ngoặc vuông.

##1. Git Auto Completion

Nếu bạn chạy câu lệnh Git thông qua command line, thì nó là một nhiệm vụ khó khăn khi mỗi lần gõ lệnh bằng tay. Để giúp đỡ điều này, bạn có thể bật tính năng tự động gợi ý câu lệnh Git chỉ trong 1 vài phút.

Để lấy đoạn mã, chạy những lệnh sau trong hệ thống Unix:

```
cd ~
curl https://raw.github.com/git/git/master/contrib/completion/git-completion.bash -o ~/.git-completion.bash
```

Tiếp theo, thêm các dòng dưới đây vào file ```~/.bash_profile``` của bạn:

```
if [ -f ~/.git-completion.bash ]; then
    . ~/.git-completion.bash
fi
```

Mặc dù tôi đã đề cập đến cái này trước đây, tôi không thể stress it enough: Nếu bạn muốn sử dụng đầy đủ tính năng của Git, bạn nên chuyển sang giao diện dòng lệnh.

##2. Bỏ qua các file trong GIT

Có phải bạn đã mệt mỏi với các file được biên dịch (giống như ```.pyc```) xuất hiện trong Git repository? Hoặc bạn quá chán vì đã thêm chúng vào Git? Không phải tìm đâu xa, có một cách thông qua đó bạn có thể nói với Git để hoàn toàn bỏ qua các file hoặc thư mục. Đơn giản tạo một file với tên ```.gitignore``` và liệt kê các file và thư mục bạn không muốn Git theo dõi. Bạn có thể tạo ra các tạo các trường hợp loại trừ sử dụng dấu chấm than(!).

```
*.pyc
*.exe
my_db_config/

!main.pyc
```

##3.  Ai đã làm hỏng code của tôi?

Đó là một bản năng tự nhiên của con người để đổ lỗi cho người khác khi có lỗi xảy ra. Nếu server của sản phẩm bị hỏng, nó rất dễ dàng tìm ra thủ phạm chỉ cần thực hiện ```git blame```. Đây là câu lệnh hiển thị cho bạn các tác giả của các mỗi dòng code trong file, commit cho thấy sự thay đổi cuối cùng trong dòng đó, và thời gian của commit.

```
git blame [file_name]

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946443git-ninja-01.png)

Và đây là ảnh chụp ảnh màn hình bên dưới, bạn có thể thấy làm thế nào câu lệnh theo dõi một repoitory lớn hơn.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946441git-ninja-02.png)

##4. Xem lại lịch sử của Repository

Trong một hướng dẫn trước, chúng ta đã thấy có thể sử dụng ```git log``` , tuy nhiên, có 3 tuỳ chọn, bạn nên biết về chúng.

* ```--oneline``` – Nén thông tin hiển thị bên cạnh các commit để giảm bớt các mã hash của commit và message của commit, tất cả hiển thị trên 1 dòng.
* ```--graph``` – Tuỳ chọn này vẽ ra một đồ thị dựa trên các ký tự để biểu diễn lịch sử trên thanh bên trái của kết quả hiển thị ra. Không sử dụng nếu bạn đang xem lịch sử trên một nhánh.
* ```--all``` – Hiển thị lịch sử của tất cả các nhánh.

Đây là kết quả của việc hợp các tuỳ chọn trên, giống như sau:

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946444git-ninja-03.png)

##5. Không bao giờ mất sự theo dõi của một commit

Giả sử bạn đã commit một cái gì đó bạn không muốn và khôg kết thúc khi thực hiện một reset hard trở về trước của trạng thái trước đó. Sau đó, bạn nhận ra bạn mất một số thông tin khác trong quá trình và bạn muốn lấy lại nó hoặc ít nhất xem được nó. Đây là lúc ```git reflog``` có thể giúp được bạn.

Một câu lệnh đơn giản ```git log``` hiển thị cho bạn commit mới nhất, đó là cha,  cha của cha, và hơn thế nữa. Tuy nhiên, ```git reflog``` là một danh sách của các commit mà ở head trỏ đến. Nhớ rằng nó là cục bộ trên hệ thống của bạn, nó không phải một phần repository của bạn và không bao gồm các push và các merge.

Nếu tôi chạy ```git log```, tôi sẽ lấy được các commit đó là một phần repository của tôi:

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946446git-ninja-04.png)

Tuy nhiên, một ```git reflog``` hiển thị một commit (```b1b0ee9 – HEAD@{4}```) tức là nó đã bị mất khi bạn reset hard:

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946447git-ninja-05.png)

##6. Staging một phần thay đã thay đổi cho một Commit

Nói chung đây là cách tốt để thực hiện các commit với các tính năng cơ bản, mỗi commit phải đại diện cho một tính năng hoặc một việc sửa lỗi. Xem xét điều gì xảy ra nếu bạn đã sửa 2 lỗi hoặc thêm nhiều tính năng mà không commit sự thay đổi. Trong mỗi trường hợp bạn có thể đặt các thay đổi trong một commit. Nhưng có một cách tốt hơn: Đặt các file riêng lẻ và commit chúng một cách riêng biệt.

Giả sử, bạn đã thực hiện nhiều sự thay đổi trong một file và muốn chúng được đặt trong các commit riêng rẽ. Trong trường hợp đó, chúng ta có thể thêm các file bằng tiền tố ```-p``` vào các câu lệnh add.

```

git add -p [file_name]

```

Hãy thử chứng minh điều tương tự. Tôi có 3 dòng mới được thêm vào ```file_name``` và tôi chỉ muốn dòng đầu tiên và dòng thứ 3 được thêm vào commit của tôi. hãy xem một lệnh ```git diff``` hiển thị chúng.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946449git-ninja-06.png)

Và hãy xem điều gì xảy ra khi chúng ta thêm ```-p``` vào lệnh ```add```.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946450git-ninja-07.png)

Có vẻ  Git cho rằng tất cả các thay đổi là một phần của ý tưởng giống nhau, do đó có thể nhóm thành một khối lớn. Bạn có các lựa chọn sau:

* Nhập y để stage that hunk
* Nhập n để không stage that hunk
* Nhập e để sửa tay the hunk
* Nhập d để thoát ra hoặc đi đến file tiếp.
* Nhập s để cắt the hunk.

Trong trường hợp của chúng tôi, chúng tôi chắc chắn muốn chia nhỏ nó thành các phần nhỏ hơn để lựa chọn thêm một số hoặc bỏ qua số còn lại.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946452git-ninja-08.png)

Giống như bạn có thể nhìn thấy, chúng tôi đã thêm dòng đầu tiên và dòng thứ ba và bỏ qua dòng thứ hai, Bạn có thể xem trạng thái của repository và thực hiện một commit.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946454git-ninja-09.png)

##7. Squash nhiều Commit

Khi bạn submit code của bạn để xem và tạo một pull request (điều đó xảy ra thường xuyên trong một dự án mã nguồn mở), bạn có thể được yêu cầu tạo ra một thay đổi cho code của bạn trước khi nó được chấp nhận. Bạn thực hiện thay đổi, chỉ được yêu cầu thay đổi nó nó lại trong lần kiểm tra tiếp theo. Trước khi bạn biết nó, bạn có một vài commit nữa. Tốt nhất, bạn có thể squash chúng vào thành một sử dụng câu lệnh ```rebase```.

```

git rebase -i HEAD~[number_of_commits]

```

Nếu bạn muốn squash hai commit gần nhất, bạn có thể chạy theo câu lệnh sau.

```

git rebase -i HEAD~2

```

Trong khi chạy câu lệnh đó, bạn đã được đưa đến một giao diện liệt kê các commit và hỏi bạn bạn squash cái nào. Tốt nhất, bạn (lấy) ```pick``` commit gần nhất và  ```squash``` những cái cũ.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946455git-ninja-10.png)

Sau đó bạn được yêu cầu cung cấp một message cho commit để tạo commit mới. Quá trình này bản chất là ghi lại vào lịch sử commit của bạn.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946457git-ninja-11.png)

##8. Stash các thay đổi chưa được commit

Giả sử bạn đang làm việc với một lỗi hoặc một tính năng nhất đinh và bạn đột ngột được yêu cầu thể hiện công việc của bạn. Công việc hiện tại đó chưa hoàn thành để commit, và bạn không thể đưa ra một mô tả vào lúc này (Không revert lại các thay đổi). Trong những trường hợp như thể này, ```git stash``` giúp giải quyết trường hợp này. Stash bản chất lấy tất cả các thay đổi của bạn và lưu trữ chúng để sử dụng tiếp. Để stash các thay đổi của bạn, bạn có thể đơn giản chạy câu lệnh như sau:

```

git stash

```

Để kiểm tra danh sách các stash, bạn có thể chạy câu lệnh như sau:

```

git stash list

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946458git-ninja-12.png)

Nếu bạn muốn huỷ bỏ stash và khôi phục các thay đổi chưa được commit bạn có thể apply stash:

```

git stash apply

```

Trong ảnh chụp màn hình cuối cùng, bạn có thể thấy mỗi stash có một định danh, một con số duy nhất (mặc dù chúng ta chỉ có một stash trong trường hợp này) Trong trường hợp bạn muốn apply chỉ các stash được chọn, bạn thêm định danh của stash cho câu lệnh ```apply``` :

```

git stash apply stash@{2}

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946461git-ninja-13.png)


##9. Kiểm tra các commit bị mất

Mặc dù ```reflog``` là một cách kiểm tra các commit bị mất, nó không khả thi trong các repository lớn. Đó là khi câu lệnh ```fsck``` (file hệ thống kiểm tra) vào cuộc .

```

git fsck --lost-found

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946463git-ninja-14.png)

Ở đây bạn có thể thấy commit bị mất, Bạn có thể kiểm tra lại sự thay đổi trong commit bằng cách chạy ```git show [commit_hash]``` hoặc khôi phục lại chúng bằng cách chạy ```git merge [commit_hash]```.

```git fsck``` có lợi hơn ```reflog```. Giả sử bạn đã xoá một nhánh remote và sau đó clone repository đó. Với ```fsck``` bạn có thể tìm kiếm và khôi phục lại nhánh remote đã xoá.


##10. Cherry Pick

Cuối cùng tôi đã lưu lệnh Git thanh lịch nhất. Câu lệnh ```cherry-pick```  là câu lệnh Git yêu thích của tôt, bởi vì về nghĩa đên giống như lợi ích của nó.

Đơn giản, ```cherry-pick``` lấy một commit duy nhất từ một nhánh khác và merge nó với vị trí hiện tại. Nếu bạn đang làm việc song song hai hoặc nhiều nhánh, bạn có thể nhận thấy một lỗi có trong tất cả các nhánh. Nếu bạn giải quyết nó trong một, bạn có thể cherry pick commit vào trong các nhánh khác, mà không ảnh hưởng tới các file hoặc các commit khác.

Hãy xem xét một kịch bản nơi chúng ta có thể áp dụng nó. Tôi có 2 nhánh và tôi muốn ```cherry-pick``` commit có mã ```b20fd14: Cleaned junk``` vào một nhánh khác.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946465git-ninja-15.png)

Tôi đổi sang nhánh mà tôi muốn ```cherry-pick``` commit và chạy nó như sau:

```

git cherry-pick [commit_hash]

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946467git-ninja-16.png)

Mặc dù chúng ta có thể hiểu rõ ```cherry-pick``` lúc này, bạn nên biết bằng lệnh này có thể thường dẫn đến xung động, nên khi sử dụng nó bạn cần cẩn thận.

##Kết luận

Với điều này, chúng ta đến với phần cuối của danh sách các mẹo, tôi nghĩ rằng có thể giúp bạn đến một cấp độ mứoi về kỹ năng Git. Git là một lựa chọn tốt nhất và có thể hoàn thành bất cứ điều gì bạn có thẻ tưởng tượng. Vì thể luôn thử thách chính mình với Git. Rất có thể, bạn sẽ kết thúc việc học cái gì đó mới.

