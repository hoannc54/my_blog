# 11 câu hỏi phỏng vấn kinh khủng về Git sẽ khiến bạn bật khóc.

Theo cuộc khảo sát mới nhất của developer Stack Overflow, hơn 70% các developer sử dụng git, giúp nó trở thành một trong những VCS được dùng nhiều nhất trên thế giới. Git được sử dụng phổ biến ở cả phát triển open source và phần mềm thương mại, với nhiều lợi ích cho các cá nhân, nhóm và doanh nghiệp.

## Câu hỏi 1: Git fork là gì ? Sự khác nhau giữa fork, branch và clone là gì ?

* Một fork là một remote, một repository copy của phía server, riêng biệt so với bản gốc. Một fork không là một khái niệm Git thực sự, đó là một ý tưởng chính trị/xã hội. 
* Một clone không là một fork, một clone là một bản copy dưới local của một vài repository trên remote. Khi bạn clone, bạn thực ra đang copy toàn bộ mã nguồn của repository, bao gồm tất cả lịch sử và các nhánh.
* Một nhánh (branch) là một cơ chế để xử lý các thay đổi với một repository riêng để sau đó merge chúng với các code còn lại. Một nhánh hiểu là một vài thứ nằm trong một repository. Về mặt khái niệm, nó đại diện cho một luồng phát triển.

## Câu hỏi 2: Sự khác nhau giữa một "pull request" và một "nhánh" là gì ?

Một **nhánh** chỉ là một phiên bản riêng của code
Một **pull request** là khi ai đó lấy về một repository, tạo chúng trên nhánh của họ, thực hiện một số thay đổi, sau đó cố gắng hợp nhất nhánh đó vào (đẩy những thay đổi của họ vào repository code của người khác)

## Câu hỏi 3: Sự khác nhau giữa 'git pull' và 'git fetch' là gì ?

Theo thuật ngữ đơn giản nhất, `git pull` làm một `git fetch` được theo dõi bởi một `git merge`
* Khi bạn sử dụng `pull` , Git cố gắng để tự động làm các việc cho bạn. Đó là ngữ cảnh nhạy cảm, nên Git sẽ merge bất kỳ các commit nào được pull vào nhánh bạn đang làm việc hiện tại. `pull` tự động merge các commit mà không để bạn xem qua chúng trước khi merge. Nếu bạn không quản lý chặt các nhánh của bạn, bạn có thể thường xuyên gặp những xung đột (conflict).
* Khi bạn `fetch`, Git thu thập bất kỳ commit nào **từ nhánh mục tiêu không tồn tại trong nhánh hiện tại của bạn** và lưu chúng trong repository local của bạn. Điều này đặc biệt hữu ích nếu bạn cần cập nhật repository của mình, nhưng những thứ đang làm việc có thể bị hỏng nếu bạn cập nhật các file của mình. Để tích hợp các commit vào nhánh master, bạn sử dụng `merge`

## Câu hỏi 4: Làm thế nào để revert lại commit trước trong Git ?

Giả sử bạn có cái này, C là nơi con trỏ HEAD của bạn và F là trạng thái của các file của bạn.

```
   (F)
A-B-C
    ↑
  master
``` 




* Để nuke thay đổi trong commit 

```
git reset --hard HEAD~1
```


Bây B là HEAD. Bởi vì bạn đã sử dụng --hard, các file của bạn được reset để trạng thái về tại commit B
* Để huỷ bỏ commit nhưng vẫn giữ thay đổi của mình: 

```
git reset HEAD~1
```

Bây giờ chúng ta nói cho Git để di chuyển con trỏ HEAD quay trở lại một commit B và để các file như chính chúng và `git status` hiển thị các thay đổi đã được kiểm tra trong C

Khi bạn thực hiện `git status`, bạn sẽ thấy các file tương tự như là đã index trước đó.

## Câu hỏi 5: "git cherry-pick" là gì ?

Câu lệnh `git cherry-pick` thường được sử dụng để đặt các commit cụ thể từ một nhánh trong một repository trên một nhánh khác. Sử dụng phổ biến để chuyển tiếp hoặc back-port các commit từ nhánh bảo trì đến nhánh phát triển.

Điều này trái ngược với các cách khác như merge và rebase mà thường được áp dụng cho nhiều commit vào một nhánh khác.

Xem xét: 
`git cherry-pick <commit-hash>`

Nguồn: [stackoverflow.com](https://stackoverflow.com/questions/9339429/what-does-cherry-picking-a-commit-with-git-mean)

## Câu hỏi 6: Giải thích những ưu điểm của Forking Workflow

**Forking Workflow** về cơ bản khác với workflow Git phổ biến khác. Thay vì sử dụng một repository phía máy chủ để hoạt động như một "trung tâm" codebase, nó cung cấp cho mỗi developer repository phía máy chủ riêng của họ. "Forking Workflow" thường thấy nhất trong các dự án open soure được public.

Ưu điểm chính của Forking Workflow là các đóng góp có thể được tích hợp mà không cần mọi người đẩy vào một repository trung tâm duy nhất dẫn đến lịch sử commit của dự án được sạch sẽ. Các developer push lên chính các repository phía server của họ, và chỉ người bảo trì dự án có thể push lên repository chính.

Khi các developer sẵn sàng publish một commit cục bộ, họ push commit đó lên chính repository public của họ, không phải là repository chính. Sau đó, họ tạo một pull request với repository chính, cho phép người bảo trì dự án biết rằng bản cập nhật sẵn sàng được tích hợp.

Nguồn: [stackoverflow.com](https://www.atlassian.com/git/tutorials/comparing-workflows/forking-workflow)

## Câu hỏi 7: Hay nói với tôi sự khác nhau giữa HEAD, làm việc với tree và index trong Git ?

* working tree/working directory/workspace là cây thư mục (mã nguồn) của các file mà bạn có thể nhìn thấy và chỉnh sửa
* index/ staging area là một file đơn lẻ, lớn, nhị phẩn trong /.git/ index, liệt kê tất cả các file trong nhánh hiện tại, sha1 checksum của chúng, time stamps và tên file - nó không phải là một thư mục khác với một bản copy của các file trong đó
* HEAD là một tham chiếu đến commit cuối cùng trong nhánh hiện tại đã được kiểm tra

Nguồn: [stackoverflow.com](https://stackoverflow.com/questions/3689838/whats-the-difference-between-head-working-tree-and-index-in-git)

## Câu hỏi 8: Bạn có thể giải thích về luồng làm việc của Gitflow không ?

Luồng làm việc Gitflow sử dụng hai nhánh chạy song song lâu dài để ghi lại lịch sử của dự án, `master` và `develop` : 

* **Master** luông sẵn sàng được release trên LIVE, với tất cả mọi thứ được kiểm tra đầy đủ và được phê duyệt (sẵn sàng cho production)
    - **Hotfix** nhánh bảo trì hoặc "hotfix" được sử dụng để nhanh chóng vá các bản phát hành của production. Các nhánh hotfix rất giống các nhánh phát hành và các nhánh tính năng trừ khi chúng được dựa trên nhánh  `master` thay vì `develop`
* **Develop** là nhánh để mà tất cả các nhánh tính năng được merge vào và nơi tất cả các test được thực hiện. Chỉ khi mọi thứ được kiểm tra kỹ lưỡng và được fix thì nó mới có thể được mereg với `master`
    - **Feature** mỗi tính năng mới nên được để ở trong nhánh riêng của nó, nó có thể được push lên nhánh `develop` giống nhánh cha của chúng.

![anh](https://res.cloudinary.com/practicaldev/image/fetch/s--pLQxGakq--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://wac-cdn.atlassian.com/dam/jcr:61ccc620-5249-4338-be66-94d563f2843c/05%2520%282%29.svg%3FcdnVersion%3Dji)

Nguồn : [atlassian.com](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)

## Câu hỏi 9: Khi nào tôi nên sử dụng `git stash` ?

Câu lệnh `git stash` thực hiện với các thay đổi chưa được commit của bạn (cả staged và không staged), lưu chúng lại để sử dụng sau này, và sau đó revert chúng từ bản sao làm việc của bạn.

Xem xét

```
$ git status
On branch master
Changes to be committed:
new file: style.css
Changes not staged for commit:
modified: index.html
$ git stash
Saved working directory and index state WIP on master: 5002d47 our new homepage
HEAD is now at 5002d47 our new homepage
$ git status
On branch master
nothing to commit, working tree clean
```

Một nơi chúng ta có thể sử dụng stash là nếu chúng ta phát hiện ra chúng ta đã quên một diều gì đó trong lần commit cuối cùng của chúng ta và bắt đầu làm công việc tiếp theo trong cùng một nhánh

```
# Assume the latest commit was already done
# start working on the next patch, and discovered I was missing something

# stash away the current mess I made
$ git stash save

# some changes in the working dir

# and now add them to the last commit:
$ git add -u
$ git commit --ammend

# back to work!
$ git stash pop
```

Nguồn: [atlassian.com](https://www.atlassian.com/git/tutorials/saving-changes/git-stash)

## Câu hỏi 10: Làm thế làm xoá một file từ git mà không cần xoá nó khỏi hệ thống file của bạn ?

Nếu bạn không cẩn trọng trong một lệnh `git add`, bạn có thể sẽ thêm các file mà bạn không muốn commit. Tuy nhiên, `git rm` sẽ xoá chúng từ cả khu vực stag của bạn(index) , cũng như hệ thống file của bạn (cây làm việc), có thể không phải là thứ bạn muốn

Thay vì đó sử dụng `git reset`:

```
git reset filename          # or
echo filename >> .gitingore # add it to .gitignore to avoid re-adding it
```

Điều này có nghĩa là `git reset <paths>` quay ngược lại `git add <paths>`

Nguồn: [codementor.io](https://www.codementor.io/citizen428/git-tutorial-10-common-git-problems-and-how-to-fix-them-aajv0katd)

## Câu hỏi 11: Khi nào bạn sử dụng "git rebase" thay vì "git merge" ?

Cả 2 lệnh này được thiết kế để tích hợp các thay đổi từ một nhánh này sang một nhánh khác, chúng chỉ thực hiện theo một vài cách khác nhau

Xem xét trước khi merge/rebase

```
A <- B <- C    [master]
^
 \
  D <- E       [branch]
```


Sau đó `git merge master`: 

```
A <- B <- C
^         ^
 \         \
  D <- E <- F
```

Sau đó `git rebase master`

```
A <- B <- C <- D <- E

```

Với rebase bạn có thể sử dụng cho một nhánh khác làm cơ sở mới cho công việc của bạn

Khi nào sử dụng : 

1. Nếu bạn có bất kỳ nghi ngờ, sử dụng merge
2. Sự lựa chọn rebase hoặc merge dựa trên những gì bạn muốn lịch sử trong như thế nào

Nhiều yếu tố cần xem xét: 

1. Nhánh của bạn có đang nhận được các thay đổi từ việc chia sẻ với các develop khác ngoài team của bạn (ví dụ opensoure, public) ? Nếu vậy, đừng rebasse. Rebase sẽ huỷ bỏ các nhánh và các developer khác sẽ bị hỏng / không nhất quán trừ khi bạn sử dụng lệnh `git pull --reabse`
2. **Sự phát triển team của bạn có kỹ năng thế nào** Rebase là một hành động phá hoại. Điều đó có nghĩa, nếu bạn không áp dụng nó một cách chính xác, bạn có thể mất công việc đã commit và hoặc phá vỡ sự thống nhất giữa các repository của các developer.
3. **Bản thân nhánh đó có đại diện cho một thông tin hữu ích không ?** Một số nhóm sử dụng mô hình `branch-per-feature`  nơi mà mỗi nhánh thể hiện một tính năng (hoặc bugfix hoặc tính năng con, ...). Trong mô hình này nhánh giúp xác định mối liên quan giữa các commit. Trong trường hợp mô hình `branch-per-developer` chính nhánh không truyền tải bất cứ thông tin bổ sung nào (commit đã lưu tác giả). Sẽ không có hại gì khi rebase
4. **Bạn có thể muốn revert một merge vì bất kỳ lý do nào ? ** Revert (giống như undo) một rebase là một điều khá khó khăn và không thể (nếu rebase có xung đột) so với revert một merge. Nếu bạn nghĩ rằng có thể có một cơ hội sẽ muốn revert thì sử dụng merge
Nguồn: [stackoverflow.com](https://stackoverflow.com/questions/804115/when-do-you-use-git-rebase-instead-of-git-merge)




