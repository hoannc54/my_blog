[Source](https://www.codediesel.com/php/unpacking-binary-data/ "Permalink to Unpacking binary data in PHP")

# Giải nén dữ liệu nhị phân trong PHP

Làm việc với các file nhị phân trong PHP là một việc hiếm khi được yêu cầu.Tuy nhiên khi cần các tính năng 'pack' và 'unpack
 trong PHP có thể giúp bạn rất nhiều. Để thiết lập giai đoạn, chúng tôi sẽ bắt đầu với một vấn đề trong lập trình, điều này sẽ giúp cho cuộc thảo luận cùng với bối cảnh liên quan với nhau. Vấn đề ở đây là : Chúng ta muốn viết một hàm để lấy một bức ảnh làm đối số và cho chúng ta biết file đó có phải là một ảnh GIF không; mà không liên quan tới extension của file. Chúng ta không sử dụng bất kỳ hàm nào của thư viện GD.  

#### Một header file của ảnh GIF

Với yêu cầu chúng ta không được phép sử sụng bất kỳ hàm nào của đồ hoạ, để giải quyết vấn đề chúng ta cần lấy dự liệu liên quan từ chínhh file ảnh GIF. Không giống như của file HTM hoặc XML hoặc định dạng text khác , một file GIF và hầu hết các định dạng khác của ảnh được lưu trữ dưới một định dạng nhị phân. Hầu hết các file nhị phân mang một header đầu đầu file cá mà cung cấp các thông tin meta về file cụ thể. Chúng ta có thể sử dụng thông tin đó để tìm ra loại file hoặc các thứ khác, giống như chiều cao, chiều rộng trong một file GIF. Một header của GIF thông thường được hiển thị bên dưới, sử dụng một editor cho hex như là [WinHex][1]. 

![][2]

Mô tả chi tiết của header được đưa ra dưới đây.

    
    
    Offset   Length   Contents
      0      3 bytes  "GIF"
      3      3 bytes  "87a" or "89a"
      6      2 bytes  
      8      2 bytes  
     10      1 byte   bit 0:    Global Color Table Flag (GCTF)
                      bit 1..3: Color Resolution
                      bit 4:    Sort Flag to Global Color Table
                      bit 5..7: Size of Global Color Table: 2^(1+n)
     11      1 byte   
     12      1 byte   
     13      ? bytes  
             ? bytes  
             1 bytes   (0x3b)


Vì vậy, để kiểm tra một file ảnh là một file GIF, chúng ta cần kiểm tra bắt đầu từ 3 byte của header, nếu có ký tự đánh dấu 'GIF' và 3 byte tiếp theo, là đưa ra số phiên bản; hay '87a' hoặc '89a'. Đó là nhiệm vụ giống như hàm unpack() mà không thể thiếu . Trước khi chúng ta xem xét giải pháp, chúng ta sẽ xem nhanh hàm unpack().

#### Sử dụng hàm unpack()

[unpack()][3] là sự bổ sung của [pack()][4] – nó chuyển đổi dữ liệu nhị phân thành một mảng kết hợp dựa trên định dạng được chỉ định. Điều này giống với hàm _sprintf_, chuyển đổi dữ liệu ký tự sang một số định dạng. Hai hàm này cho phép chúng ta đọc và ghi bộ đêm cho dữ liệu nhị phân theo định dạng của chuỗi cho trước.. Điều đó dễ dàng cho phép một lập trình viên chuyển đổi dữ liệu với các chương trình được viết bằng ngôn ngữ khác hoặc định dạng khác. Lây một ví dụ nhỏ.

    
    
    $data = unpack('C*', 'codediesel');
    var_dump($data);



Nó sẽ in ra mã thập phân cho 'codediesel' như sau:

    
    
    array
      1 => int 99
      2 => int 111
      3 => int 100
      4 => int 101
      5 => int 100
      6 => int 105
      7 => int 101
      8 => int 115
      9 => int 101
      10 => int 108



Trong ví dụ trên đối số đầu tiên là định dạng chuỗi và thứ hai là dữ liệu thực tế. Chuỗi định dạng sẽ chỉ định cách mà đối dữ liệu nên được parse ra. Trong ví dụ phần đầu của định dạng là 'C', xác định rằng chúng ta sẽ xử lý ký tự đầu tiên của dữ liệu giống như byte không dấu. Phần tiếp theo là ' * ', cho thấy rằng hàm sẽ áp dụng vào định dạng code ở phần chỉ định trước lên tất cả các ký tự còn lại.

Dù điều này trông có vẻ khó hiểu, nhưng phần tiếp theo sẽ cung cấp một ví dụ cụ thể.

#### Lấy dữ liệu header

Dưới đây là giải pháp cho vấn đề GIF của chúng ta sử dụng hàm unpack(). Hàm _is_gif()_ sẽ trả về true nếu file đưa vào là một định dạng GIF.

    
    
    function is_gif($image_file)
    {
     
        /* Open the image file in binary mode */
        if(!$fp = fopen ($image_file, 'rb')) return 0;
     
        /* Read 20 bytes from the top of the file */
        if(!$data = fread ($fp, 20)) return 0;
     
        /* Create a format specifier */
        $header_format = 'A6version';  # Get the first 6 bytes
    
        /* Unpack the header data */
        $header = unpack ($header_format, $data);
     
        $ver = $header['version'];
     
        return ($ver == 'GIF87a' || $ver == 'GIF89a')? true : false;
     
    }
     
    /* Run our example */
    echo is_gif("aboutus.gif");


Dòng code quan trọng cần chú ý là phần khai báo định dạng. Ký tự  'A6' chỉ định hàm unpack() sẽ lấy 6 bytes đầu tiên trong dữ liệu và ngắt chúng ra là một chuỗi. Dữ liệu được lấy ra sau đó lưu vào một mảng liên kết với khoá có tên 'version'.

Ví dụ khác bên dưới. Nó trả về thêm dữ liệu header của file GIF, bao gồm chiều rộng và chiều cao của ảnh.
    
    
    function get_gif_header($image_file)
    {
     
        /* Open the image file in binary mode */
        if(!$fp = fopen ($image_file, 'rb')) return 0;
     
        /* Read 20 bytes from the top of the file */
        if(!$data = fread ($fp, 20)) return 0;
     
        /* Create a format specifier */
        $header_format = 
                'A6Version/' . # Get the first 6 bytes
                'C2Width/' .   # Get the next 2 bytes
                'C2Height/' .  # Get the next 2 bytes
                'C1Flag/' .    # Get the next 1 byte
                '@11/' .       # Jump to the 12th byte
                'C1Aspect';    # Get the next 1 byte
    
        /* Unpack the header data */
        $header = unpack ($header_format, $data);
     
        $ver = $header['Version'];
     
        if($ver == 'GIF87a' || $ver == 'GIF89a') {
            return $header;
        } else {
            return 0;
        }
    }
     
    /* Run our example */
    print_r(get_gif_header("aboutus.gif"));



Ví dụ bên trên sẽ in ra như sau khi chạy.


    
    
    Array
    (
        [Version] => GIF89a
        [Width1] => 97
        [Width2] => 0
        [Height1] => 33
        [Height2] => 0
        [Flag] => 247
        [Aspect] => 0
    )



Bên dưới chúng ta sẽ đi vào chi tiết về cách công cụ về định dạng làm việc. Tôi sẽ chia nhỏ định dạng,  đưa ra các chi tiết của mỗi ký tự.
    
    
    $header_format = 'A6Version/C2Width/C2Height/C1Flag/@11/C1Aspect';

    
    
    A - Đọc một byte và cắt nó thành một chuỗi. 
        Số byte để đọc được đưa vào tiếp theo
    6 - Đọc tổng cộng 6 byte, bắt đầu từ vị trí 0
    Version - Tên của key trong mảng liên kết nơi mà dữ liệu nhận được bởi 'A6' được lưu trữ
     
    / - Bắt đầu một mã định dạng mới
    C - Mô tả dữ liệu tiếp theo dưới dạng byte không dấu
    2 - Đọc tổng cộng 2 byte
    Width - Key trong mảng liên kết
     
    / - Bắt đầu một mã định dạng mới
    C - Mô tả dữ liệu dưới dạng byte không dấu
    2 - Đọc tổng cộng 2 byte
    Height- Key trong mảng liên kết
     
    / - Bắt đầu một mã định dạng mới
    C - Mô tả dữ liệu dưới dạng byte không dấu
    1 - Đọc tổng cộng 2 byte
    Flag - Key trong mảng liên kết
     
    / - Bắt đầu một mã định dạng mới
    @ - Di chuyển các byte offset được chỉ định số sau.
          Nhớ rằng vị trí đầu tiên của chuỗi nhị phân là 0. 
    11 - Di chuyển đến vị trí 11
     
    / - Start a new code format
    C - Mô tả dữ liệu dưới dạng byte không dấu
    1 - Đọc tổng cộng 2 byte
    Aspect - Key trong mảng liên kết

 | 

Thêm các tuỳ chọn định dạng có thể tìm [ở đây][4]. Mặc dù tối chỉ đưa ra một ví dụ nhỏ, pack/unpack có khả năng con làm việc phức tạp hơn nhiều so với ở đây.

Chú ý: Từ phiên bản PHP 7.2.0 lại float và double hỗ trợ cả hai both Big Endian và Little Endian.

[1]: http://www.x-ways.net/winhex/index-m.html
[2]: http://www.codediesel.com/wp-content/uploads/2010/09/winhex.gif "winhex"
[3]: http://php.net/manual/en/function.unpack.php
[4]: http://www.php.net/manual/en/function.pack.php
