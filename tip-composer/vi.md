# 17 Mẹo sử dụng Composer hiệu quả

Mặc dù hầu hết các lập trình viên PHP biết làm thế nào sử dụng Composer, nhưng không phải tất cả trong số họ đang sử dụng nó hiệu quả hoặc cách hợp lý nhất. Vì vậy tôi quyết định tóm tắt những thứ quan trọng trong quy trình làm việc mỗi ngày của tôi.

Triết lý của hầu hết các mẹo là "chơi an toàn", nghĩa là nếu có nhiều cách để xử lý cái gì đó, tôi sẽ sử dụng cách tiếp cận ít bị lỗi nhất. 

### Mẹo 1: Đọc tài liệu 

Tôi thực sự thấy nó ý nghĩa. Tài liệu của composer là rất tốt và chỉ mất vài tiếng để đọc nó sẽ tiết kiệm nhiều thời gian về sau. Bạn sẽ ngạc nhiên cách mà nhiều thứ của composer có thể làm. 

### Mẹo 2: Hãy nhận biết sự khác nhau giữa một dự án và một thư viện

Điều quan trọng là phải biết, liệu bạn đang tạo một dự án hay một thư viện. Mỗi loại trong số chúng yêu cầu cách thiết lập thực hiện riêng biệt.

Một thư viện là một gói có thể tái sử dụng, ở đó bạn sẽ thêm như là một phụ thuộc, giống như `symfony/symfony`, `doctrine/orm` hoặc `elasticsearch/elasticsearch`

Một dự án thường là một ứng dụng, nó phụ thuộc vào một số thư viện. Nó thì thường không tái sử dụng được (không có dự án khác nào có thể yêu cầu (require) nó như là một phụ thuộc). Ví dụ điển hình là một trang web thương mại điện tử, hệ thống hỗ trợ khách hàng, ....

Tôi sẽ phân biệt giữa thư viện và một dự án trong các mẹo bên dưới. 

### Mẹo 3: Sử dụng các phiên bản cụ thể của phụ thuộc cho các ứng dụng

Nếu bạn đang khởi tạo một ứng dụng, bạn nên sử dụng một phiên bản cụ thể nhất để xác định dependency. Nếu bạn cần parse các file YAML, bạn nên chỉ định dependency này `"symfony/yaml": "4.0.2"`

Ngay cả khi nếu thư viện tuân theo [Sematic Versioning](https://semver.org/), chúng có thể có khả năng tương thích ngược trong các phiên bản nhỏ và bản vá. Ví dụ, nếu bạn sử dụng `"symfony/symfony": "^3.1"`, có một vài thứ lại không dùng trong bản 3.2, những điều đó có thể làm hỏng các test của ứng dụng. Hoặc có thể có một lỗi được fix trong PHP_CodeSniffer và nó sẽ phát hiện ra các lỗi định dạng trong code của bạn, chúng một lần nữa có thể dẫn đến quá trình xây dựng bị hỏng.

Cập nhật các phụ thuộc nên thận trọng, không ngẫu nhiên. Một trong những mẹo phía dưới thảo luận chi tiết hơn.

Nghe có vẻ hơi quá, nhưng nó sẽ ngăn chặn đồng nghiệp của bạn vô tình cập nhật các phụ thuộc khi thêm một thư viện mới vào dự án (cái mà bạn có thể bỏ lỡ trong khi kiểm tra code).

### Mẹo 4: Sử dụng các khoảng phiên bản cho các thư viện phụ thuộc

Nếu bạn đang tạo một thư viện, bạn nên xác định phạm vi rộng nhất có thể của phiên bản. Nếu bạn tạo một thư viện sử dụng thư viện `symfony/yaml` cho quá trình parse YAML, bạn nên yêu cầu nó như thế này : 

`"symfony/yaml": "^3.0 || ^4.0"`

Có nghĩa là thư viện của bạn có thể sử dụng `symfony/yaml` từ bất kỳ phiên bản Symfony 3.x hoặc 4.x. Điều đó quan trọng, vì ràng buộc này được chuyển đến ứng dụng sử dụng thư viện của bạn.

Trong trường hợp có 2 thư viện với yêu cầu bị xung đột, ví dụ, một yêu cầu phiên bản `~3.1.0` và một cái khác yêu cầu phiên bản `~3.2.0`, thì cài đặt sẽ bị thất bại.

### Mẹo 5: Bạn nên commit `composer.lock` lên git trong ứng dụng

Nếu bạn đang tạo một dự án, bạn chắc chắn muốn commit `composer.lock` lên git. Điều này đảm bảo mọi người, bạn, đồng nghiệp của bạn, máy chủ CI của bạn, máy chủ production của bạn đang chạy ứng dụng với các phiên bản phụ thuộc giống nhau. 

Ngay cái nhìn đầu tiên, nghe có vẻ không cần thiết, bạn đã sử dụng một phiên bản cụ thế trong ràng buộc, giống như được nhắc đến trong Mẹo 3. Nhưng không, cũng có những phụ thuộc của phụ thuộc mà không bị ràng buộc bởi những ràng buộc này(ví dụ: `symfony/console`  phụ thuộc vào `symfony/polyfill-mbstring`). Vì vậy không commit file `composer.lock`, bạn sẽ không nhận được cùng một tập hợp các phụ thuộc. 

### Mẹo 6: Đặt `composer.lock` vào trong `.gitignore` trong các thư viện

Nếu bạn đang tạo một thư viện (được gọi là `acme/my-library`), bạn không nên commit file `composer.lock`. Nó sẽ không có bất bất kỳ ảnh hưởng nào đối với các dự án đang sử dụng thư viện của bạn.

Hãy tưởng tượng rằng `acme/my-library` sử dụng `monolog/monolog` giống như là một phụ thuộc. Nếu bạn đã commit file `composer.lock`, tất cả mọi người đang phát triển `acme/my-library` sẽ sử dụng phiên bản cũ hơn của Monolog. Nhưng khi thư viện hoàn thành, và bạn sử dụng nó trong một dự án thật sự, một phiên bản mới của Monolog có thể được cài đặt và nó có thể không tương thích với thư viện. Nhưng bạn không nhận thấy nó trước đây, vì file `composer.lock`

Tốt nhất là đặt file `composer.lock` vào `.gitignore` của bạn, nên bạn sẽ không vô tình commit nó lên.

Nếu bạn muốn đảm bảo thư viện tương thích với các phiên bản khác nhau của phụ thuộc, nãy đọc mẹo tiếp theo.

### Mẹo 7: Chạy Travis CI xây dựng với các phiên bản khác nhau cho các phụ thuộc

_Mẹo này chỉ áp dụng cho các thư viện (bởi vì bạn sử dụng một phiên bản cụ thể cho các ứng dụng)_

Nếu bạn xây dựng một thư viện open-source, bạn có lẽ sẽ sử dụng Travis CI để chạy các bản build

Mặc định, composer cài đặt phiên bản mới nhất có thể của các phụ thuộc mà được cho phép bởi các ràng buộc trong `composer.json`. Có nghĩa là đối với ràng buộc của phụ thuộc phiên bản `^3.0 || ^4.0`, bản build sẽ luôn luôn sử dụng phiên bản cuối cùng của bản release v4. Vì bản 3.0 chưa bao giờ được kiểm tra, thư viện có thể không tương thích với nó và điều đó làm cho người dùng không thích.

May mắn thay, Composer cung cấp một chuyển đổi để cài đặt các bản phụ thuộc thấp hơn có thể `--prefer-lowest` (nên được sử dụng với `--prefer-stable` để ngăn cài đặt các phiên bản không ổn định)

Cấu hình `.travis.yml` được cập nhật có thể trông giống như sau: 

```
language: php

php:
  - 7.1
  - 7.2

env:
  matrix:
    - PREFER_LOWEST="--prefer-lowest --prefer-stable"
    - PREFER_LOWEST=""

before_script:
  - composer update $PREFER_LOWEST

script:
  - composer ci
```

Xem nó trực tiếp ở trong [my mhujer/fio-api-php library](https://github.com/mhujer/fio-api-php/blob/master/.travis.yml) và [the build matrix on Travis CI
](https://travis-ci.org/mhujer/fio-api-php)

Mặc dù giải pháp này sẽ bắt được hầu hết các trường hợp không tương thích, hãy nhớ rằng có nhiều kết hợp phụ thuộc giữa phiên bản thấp nhất và mới nhất. Và chúng có thể không tương thích với nhau.

### Mẹo 8: Sắp xếp các gói trong require và require-dev theo tên

Đó là một thói quen tốt để giữ các package trong `require` và `require-dev` được sắp xếp theo tên. Nó có thể ngăn chặn xung đột merge không cần thiết khi thực hiện rebase một nhánh. Bởi vì nếu bạn đã thêm một package vào cuối danh sách trong 2 nhánh, sẽ có xung đột merge mọi lúc

Thật không thú vị gì khi cứ phải làm điều đó m, vì vậy tốt nhất là [cấu hình nó](https://getcomposer.org/doc/06-config.md#sort-packages) trong `composer.json`

```
{
...
    "config": {
        "sort-packages": true
    },
…
}

```


Lần tiếp, bạn `require` một package mới, nó sẽ được thêm vào 1 nơi thích hợp (không phải ở cuối cùng)

### Mẹo 9: Không cố gắp merge file `composer.lock` khi rebase hoặc merge

Nếu bạn thêm một phụ thuộc mới vào `composer.json` (và `composer.lock`) và trước đó nhánh của bạn được merge, có một phụ thuộc khác được thêm vào trong nhánh master, bạn cần rebase nhánh của bạn. Và bạn sẽ gặp một xung đột merge trong `composer.lock`

Bạn không bao giờ nên thử giải quyết xung đột này bằng tay, bởi vì file `composer.lock` chứa một mã băm của các phụ thuộc được xác định trong `composer.json` . Vì vậy, ngay cả khi bạn giải quyết xung đột, kết của của file lock sẽ không chính xác.

Điều tốt nhất cần làm là tạo `.gitattribute` trong thư mục gốc của dự án với dòng sau, nó nghĩa là git sẽ không merge `composer.lock`

`/composer.lock -merge`

Bạn có thể khắc phục vấn đề này bằng cách sử dụng các nhánh tính năng ngắn (thời gian tồn tại của nhánh là ngắn) như gợi ý trong [ Trunk Based Development](https://trunkbaseddevelopment.com/) (bạn nên làm những cách này). Khi bạn có một nhánh tính năng ngắn, được merge kịp thời, nguy cơ xung đột merge của file `composer.lock` là nhỏ nhất. Bạn thậm chí có thể tạo một nhánh cho việc thêm một phụ thuộc và merge nó theo cách bên phải. 

Nhưng phải làm gì, nếu bạn gặp một xung đột merge trong `composer.lock` khi rebase ? Giải quyết nó với phiên bản từ master, vì vậy bạn sẽ chỉ có thay đổi trong `composer.json` (package mới đã được thêm vào). Và sau đó chạy `composer update --lock`, nó sẽ cập nhật file `composer.lock` với thay đổi từ `composer.json`. Bây giờ, bạn có thể stage được cập nhật `composer.lock` và tiếp tục với rebase.

### Mẹo 10: Biết sự khác nhau giữa `require` và `require-dev`

Đó là điều quan trọng cần lưu ý về sự khác nhau giữa khối `require` và `require-dev`

Các package mà được require để chạy ứng dụng hoặc thư viện nên được xác định trong `require` (Ví dụ Symfony, Doctrine, Twig, Guzzle, …). Nếu bạn đang tạo một thư viện, hãy cẩn thận những gì bạn đặt vào `require`. Bởi vì mỗi phụ thuộc từ phiên này cũng là một phụ thuộc của ứng dụng, cái mà dùng với thư viện. 

Các package cần thiết cho quá trình phát triển ứng dụng ( hoặc thư viện) nên được xác định trong `require-dev` (ví dụ: PHPUnit, PHP_CodeSniffer, PHPStan)

### Mẹo 11: Cập nhật các dependency một cách an toàn

Tôi đoán chúng ta có thể đồng ý trên thực tế các phụ thuộc đó nên được cập nhật thường xuyên. Cái tôi muốn thảo luận ở đây là các phụ thuộc đó đang cập nhật nên được rõ ràng và thận trọng, không được thực hiện bằng cách làm việc với 1 số công việc khác. Nếu bạn cấu trúc lại cái gì đó và đồng thời cập nhật một số thư viện, bạn không thể dễ dàng nói nếu ứng dụng bị hỏng bởi tái cấu trúc lại hay bởi việc cập nhật. 

Bạn có thể sử dụng câu lệnh `composer outdated` để xem các phụ thuộc nào có thể được cập nhật. Nó là một ý tưởng tốt bao gồm `--direct(-D`  để chỉ liệt kê các phụ thuộc cụ thể trong `composer.json`. Cũng có một lệnh `-m` để chỉ liệt kê các phiên bản phụ cập nhật.

Đối với mỗi phụ thuộc đã cũ, hãy làm theo các bước sau: 

1. Tạo một nhánh mới
2. Cập nhật phiên bản phụ thuộc trong `composer.json` lên phiên bản mới nhất
3. Chạy `composer update phpunit/phpunit --with-dependencies` (thay thế `phpunit/phpunit` với thư viện bạn đang cập nhật)
4. Kiểm tra CHANGELOG trong repo của thư viện trên Github để xem nếu có thay đổi lớn nào không. Nếu có, cập nhật ứng dụng 
5. Kiểm tra ứng dụng cục bộ (Nếu bạn đang sử dụng Symfony, bạn có thể tìm thấy các cảnh báo không dùng nữa trong thanh Debug)
6. Commit các thay đổi (`composer.json`, `composer.lock` và bất cứ điều gì khác cần thiết cho phiên bản mới hoạt động)
7. Chờ cho CI build và kết thúc
8. Thực hiện merge và deploy

Đôi khi việc cập nhật nhiều phụ thuộc ý nghĩa hơn cùng một lúc. Ví dụ khi bạn đang cập nhật Doctrine hoặc Symfony. Trong trường hợp này bạn có thể liệt kê chúng trong câu lệnh cập nhật: 

`composer update symfony/symfony symfony/monolog-bundle --with-dependencies`

Hoặc bạn có thể dùng một ký tự đại diện (dấu \*) để cập nhật tất cả phụ thuộc từ một namespace cụ thể 

`composer update symfony/* --with-dependencies`

Tôi biết rằng tất cả điều này nghe có vẻ tẻ nhạt, nhưng đôi khi bạn sẽ có thể cập nhật các phụ thuộc, vì vậy nó sẽ tăng khả năng an toàn thêm.

Một cách ngắn được chấp nhận để thực hiện cập nhật tất cả phụ thuộc trong `require-dev` một lúc (nếu chúng không yêu cầu thay đổi trong code, nếu không thì tôi sẽ gợi ý sử dụng các nhanh riêng để xem xét lại code dễ dàng)

Mẹo 12: Bạn có thể xác định các loại phụ thuộc khác trong `composer.json`

Ngoài việc xác định các thư viện phụ thuộc, bạn cũng có thể xác định những thứ khác ở đây

Bạn có thể xác định phiên bản PHP nào mà ứng dụng hoặc thư viện hỗ trợ: 

```
"require": {
    "php": "7.1.* || 7.2.*",
},
```

Bạn cũng có thể xác định extension nào được yêu cầu trong ứng dụng hoặc thư viện. Nó rất hữu ích khi bạn thử dockerize cho ứng dụng của bạn hoặc đồng nghiệp của bạn khi cài đặt ứng dụng lần đầu tiên

```
"require": {
    "ext-mbstring": "*",
    "ext-pdo_mysql": "*",
},
```

Bạn nên sử dụng `*` cho phiên bản extension như là [they may be a bit inconsistent](https://getcomposer.org/doc/01-basic-usage.md#platform-packages)

### Mẹo 13: Vaidate `composer.json` trong quá trình build CI

`composer.json` và `composer.lock` nên luôn luôn giữ sự đồng bộ. Do đó bạn nên kiểm tra tự động cho nó. Chỉ cần thêm câu lệnh này vào đoạn script lúc build và nó sẽ đảm bảo `composer.lock` được đồng bộ với `composer.json`

`composer validate --no-check-all --strict`

### Mẹo 14: Sử dụng một plugin của Composer trong PHPstorm

Đây là một [plugin cho composer trong PHPstorm](https://plugins.jetbrains.com/plugin/7631-php-composer-json-support). Nó thêm autocompletion và một vài validation khi thay đổi bằng tay `composer.json`

Nếu bạn sử dụng IDE khác (hoặc một editor code), bạn có thể cài đặt lại validation [JSON schema](https://getcomposer.org/schema.json)

### Mẹo 15: Chỉ định phiên bản PHP cho production trong `composer.json`

Nếu bạn giống tôi và bạn thỉnh thoảng [chạy các phiên bản PHP được phát hành trước ở local](https://blog.martinhujer.cz/php-7-2-is-due-in-november-whats-new/), bạn có nguy cơ cập nhật các phụ thuộc theo phiên bản không hoạt động trong production. Ngay bây giờ, tôi đang sử dụng PHP 7.2.0 nghĩa là tôi có thể cài đặt các thư viện, nó sẽ không hoạt động trên 7.1. Giống như production đang chạy 7.1, cài đặt sẽ bị hỏng.

Nhưng không cần lo lắng, đó là một cách dễ dàng. Hãy chỉ định một phiên bản PHP trong `config` của `composer.json`

```
"config": {
    "platform": {
        "php": "7.1"
    }
}
```

Đừng nhầm với `require`, nó hoạt động khác nhau. Ứng dụng của bạn có thể chạy trên 7.1 hoặc 7.2 và đồng thời chỉ định 7.1 giống như một platform (điều đó nghĩa là các phụ thuộc luôn luôn được cập nhật với một phiên bản tương thích 7.1)

```
"require": {
    "php": "7.1.* || 7.2.*"
},
"config": {
    "platform": {
        "php": "7.1"
    }
},
```


### Mẹo 16: Sử dụng package private từ Gitlab tự lưu trữ

Bạn nên sử dụng `vcs` giống như một loại repository và Composer nên xác định đúng cách lấy. Ví dụ, nếu bạn đang thêm một fork từ Github, nó sẽ sử dụng API của nó để tải xuống tệp .zip thay vì clone từ toàn bộ repository.

Nhưng nó phức tạp hơn cho cài đặt Gitlab private. Nếu bạn sử dụng `vcs` giống như một loại repository, Composer sẽ phát hiện ra nó là một cài đặt Gitlab sẽ thử để lấy về package sử dụng API (nó yêu cầu một API key. Tôi không muốn thiết lập nó, vì vậy tôi đã thiết lập cho cài đặt này (giống như sử dụng SSH cho clone)):

Đầu tiên chỉ định repository với loại `git`:

```
"repositories": [
    {
        "type": "git",
        "url": "git@gitlab.mycompany.cz:package-namespace/package-name.git"
    }
]
```

Sau đó sử dụng package giống như bạn có bình thường 

```
"require": {
    "package-namespace/package-name": "1.0.0"
}
```

### Mẹo 17: Làm thế nào để sử dụng tạm thời một nhánh với bugfix từ fork

Nếu bạn tìm một bug trong một vài thư viện public và bạn fix nó trong fork của bạn trên Github, bạn cần cài đặt thư viện từ repository này thay vì dùng cái chính thức (Cho đến khi bugfix được merge và được version được fix được release)

Điều đó dễ dàng xong với [dòng thay thế](https://getcomposer.org/doc/articles/aliases.md#require-inline-alias)

```
{
    "repositories": [
        {
            "type": "vcs",
            "url": "https://github.com/you/monolog"
        }
    ],
    "require": {
        "symfony/monolog-bundle": "2.0",
        "monolog/monolog": "dev-bugfix as 1.0.x-dev"
    }
}
```

Bạn có thể kiểm tra bugfix trên local trước khi push nó [sử dụng path giống như một loại repository](https://blog.martinhujer.cz/php-7-2-is-due-in-november-whats-new/)

Cập nhật 2018-01-08

Sau khi publish bài báo, tôi đã nhận được đề xuất với một số mẹo khác, chúng là đây: 

### Mẹo 18: Cài đặt prestissimo để tăng tốc cài đặt package

Đây là một plugin của Composer [ hirak/prestissimo](https://github.com/hirak/prestissimo) tăng tốc độ cài đặt phụ thuộc bằng cách tải chúng song song

Và điều tốt nhất là gì? Bạn chỉ cần cài đặt nó một lần, toàn cục và nó sẽ sự động làm việc cho tất cả dự án: 

`composer global require hirak/prestissimo`

### Mẹo 19: Kiểm tra các ràng buộc của phiên bản nếu bạn không chắc chắn.

Viết các ràng buộc của phiên bản chính xác đôi khi có thể khó khăn ngay cả sau khi đọc [tài liệu](https://getcomposer.org/doc/articles/versions.md#writing-version-constraints)

May mắn thay, đây là một package [Packagist Semver Checker](https://semver.mwl.be/) nơi bạn có thể kiểm tra phiên bản nào phù hợp với ràng buộc được chỉ định. Thay vì chỉ phân tích ràng buộc phiên bản, nó tải xuống dữ liệu từ Packagist để hiển thị các phiên bản phát hành thực tế.

Xem [kết quả cho `symfony/symfony:^3.1`](https://semver.mwl.be/#?package=symfony%2Fsymfony&version=%5E3.1&minimum-stability=stable)

### Mẹo 20: Sử dụng bản đồ class có thẩm quyền trong production

Bạn nên [tạo authoritative class map](https://getcomposer.org/doc/articles/autoloader-optimization.md#optimization-level-2-a-authoritative-class-maps) trong production. Nó sẽ tăng tốc load class bằng cách include mọi thứ trong class-map và bỏ qua bất kỳ kiểm tra nào của hệ thống tập tin. 

Bạn có thể làm điều đó bằng cách chạy câu lệnh này như một phần của bản build production: 

`composer dump-autoload --classmap-authoritative`

### Mẹo 21: Cấu hình `autoload-dev` cho test

Bạn không muốn bao gồm các file test trong class map của production (bởi vì kích thước file và bộ nhớ). Bạn có thể hoàn thành bằng cách cấu hình `autoload-dev` (tương tự `autoload`)

```
"autoload": {
    "psr-4": {
        "Acme\\": "src/"
    }
},
"autoload-dev": {
    "psr-4": {
        "Acme\\": "tests/"
    }
},
```

Mẹo 22: Thử Composer script

Composer script là một công cụ nhẹ để tạo các script build. Tôi đã viết [bài riêng về chúng ở đây](https://blog.martinhujer.cz/have-you-tried-composer-scripts/)

### Kết luận: 

Nếu bạn không đồng ý với các mẹo, tôi sẽ rất vui lòng nếu bạn có thể mô tả lý do tại sao bạn lại có ý kiến ( đừng quên đặt số cho ác mẹo ở đó)
c



