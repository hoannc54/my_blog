# Bắt đầu nhanh
Trang này cung cấp một giới thiệu nhanh về Guzzle và ví dụ giới thiệu. Nếu bạn chưa cài đặt, Guzzle, đến trang [Cài đặt][1].

## Tạo một Request

Bạn có thể gửi request với Guzzle sử dụng một đối tượng `GuzzleHttpClientInterface` .

### Tạo một Client
    
    
    use GuzzleHttpClient;
    
    $client = new Client([
        // Base URI is used with relative requests
        'base_uri' => 'http://httpbin.org',
        // You can set any number of default request options.
        'timeout'  => 2.0,
    ]);
    
	
Client không thay đổi trong Guzzle 6, có nghĩa là bạn không thể thay đổi các mặc định được sử dụng bởi client sau khi nó được tạo ra.

Client khởi tạo chấp nhận một mảng kết hợp các option:

`base_uri`
: 

(string|UriInterface) Base URI của client được gộp với các URL tương đối. Có thể là một string hoặc một thể hiện của đối tượng UriInterface. Khi một URI tương đối được cung cấp đến một client, client sẽ kết hợp base URI với URL tương đối sử dụng theo quy tắc được mô tả trong [RFC 3986, section 2][2].
    
    
    // Create a client with a base URI
    $client = new GuzzleHttpClient(['base_uri' => 'https://foo.com/api/']);
    // Send a request to https://foo.com/api/test
    $response = $client->request('GET', 'test');
    // Send a request to https://foo.com/root
    $response = $client->request('GET', '/root');
    

Không thấy giống đọc RFC 3986? Đây là một vài ví dụ nhanh làm thế nào một how a `base_uri` được giải quyết với URI khác.

| base_uri              | URI              | Result                   |  
| --------------------- | ---------------- | ------------------------ |  
| `http://foo.com`      | `/bar`           | `http://foo.com/bar`     |  
| `http://foo.com/foo`  | `/bar`           | `http://foo.com/bar`     |  
| `http://foo.com/foo`  | `bar`            | `http://foo.com/bar`     |  
| `http://foo.com/foo/` | `bar`            | `http://foo.com/foo/bar` |  
| `http://foo.com`      | `http://baz.com` | `http://baz.com`         |  
| `http://foo.com/?bar` | `bar`            | `http://foo.com/bar`     |  

`handler`
: (callable) hàm chuyển đổi các HTTP request trên đường dẫn. Hàm được gọi với một `Psr7HttpMessageRequestInterface` và mảng các tuỳ chọn chuyển đổi, và phải trả về một `GuzzleHttpPromisePromiseInterface` thỏa mãn `Psr7HttpMessageResponseInterface` khi thành công. `handler` là một hàm khởi tạo tuỳ chọn duy nhất không thể ghi đè trong các tuỳ chọn của per/request.

`...`
: (mixed) Tất cả các tùy chọn khác được truyền vào hàm khởi tạo được sử dụng như các tùy chọn request mặc định với mỗi request được tạo bởi client

###Gửi các Request

Các phương thức magic trên client tạo ra đơn giản để gửi đồng bộ các request:
    
    
    $response = $client->get('http://httpbin.org/get');
    $response = $client->delete('http://httpbin.org/delete');
    $response = $client->head('http://httpbin.org/get');
    $response = $client->options('http://httpbin.org/get');
    $response = $client->patch('http://httpbin.org/patch');
    $response = $client->post('http://httpbin.org/post');
    $response = $client->put('http://httpbin.org/put');
    

Bạn có thể tạo một request và sau đó gửi request với client khi bạn đã sẵn sàng:
    
    
    use GuzzleHttpPsr7Request;
    
    $request = new Request('PUT', 'http://httpbin.org/put');
    $response = $client->send($request, ['timeout' => 2]);
    

Đối tượng client cực kì linh hoạt trong cách request được chuyển đổi bao gồm các tuỳ chọn mặc định request, mặc định ngăn xếp bộ xử lý trung gian được sử dụng bởi mỗi request, và một URI báe cũng cho phép bạn gửi các request với các URI tương đối.

Bạn có thể tìm hiểu thêm về client middleware trong trang [_Handlers and Middleware_][3] của tài liệu.

### Không đồng bộ các request

Bạn có thể gửi các request bất đồng bộ sử dụng các phương thức magic được cung cấp bởi một client:
    
    
    $promise = $client->getAsync('http://httpbin.org/get');
    $promise = $client->deleteAsync('http://httpbin.org/delete');
    $promise = $client->headAsync('http://httpbin.org/get');
    $promise = $client->optionsAsync('http://httpbin.org/get');
    $promise = $client->patchAsync('http://httpbin.org/patch');
    $promise = $client->postAsync('http://httpbin.org/post');
    $promise = $client->putAsync('http://httpbin.org/put');
    

Bạn cũng có thể sử dụng các phương thức sendAsync()và requestAsync() của client:
    
    
    use GuzzleHttpPsr7Request;
    
    // Create a PSR-7 request object to send
    $headers = ['X-Foo' => 'Bar'];
    $body = 'Hello!';
    $request = new Request('HEAD', 'http://httpbin.org/head', $headers, $body);
    
    // Or, if you don't need to pass in a request instance:
    $promise = $client->requestAsync('GET', 'http://httpbin.org/get');
    

promise được trả về bằng những phương thức thực hiện [Promises/A+ spec][4], được cung cấp bởi [Guzzle promises library][5]. Điều này có nghĩa là bạn có thể  chain `then()` calls off of the promise. Những lời gọi sau đó được thực hiện thành công `PsrHttpMessageResponseInterface` hoặc từ chối với một exception.
    
    
    use PsrHttpMessageResponseInterface;
    use GuzzleHttpExceptionRequestException;
    
    $promise = $client->requestAsync('GET', 'http://httpbin.org/get');
    $promise->then(
        function (ResponseInterface $res) {
            echo $res->getStatusCode() . "n";
        },
        function (RequestException $e) {
            echo $e->getMessage() . "n";
            echo $e->getRequest()->getMethod();
        }
    );
    

### Các request đồng thời

Bạn có thể gửi nhiều request đồng thời sử dụng các promises và asynchronous request.
    
    
    use GuzzleHttpClient;
    use GuzzleHttpPromise;
    
    $client = new Client(['base_uri' => 'http://httpbin.org/']);
    
    // Initiate each request but do not block
    $promises = [
        'image' => $client->getAsync('/image'),
        'png'   => $client->getAsync('/image/png'),
        'jpeg'  => $client->getAsync('/image/jpeg'),
        'webp'  => $client->getAsync('/image/webp')
    ];
    
    // Wait on all of the requests to complete. Throws a ConnectException
    // if any of the requests fail
    $results = Promiseunwrap($promises);
    
    // Wait for the requests to complete, even if some of them fail
    $results = Promisesettle($promises)->wait();
    
    // You can access each result using the key provided to the unwrap
    // function.
    echo $results['image']['value']->getHeader('Content-Length')[0]
    echo $results['png']['value']->getHeader('Content-Length')[0]
    

Bạn có thể sử dụng đối tượng `GuzzleHttpPool` khi bạn có một số lượng không xác định các request bạn muốn gửi.
    
    
    use GuzzleHttpPool;
    use GuzzleHttpClient;
    use GuzzleHttpPsr7Request;
    
    $client = new Client();
    
    $requests = function ($total) {
        $uri = 'http://127.0.0.1:8126/guzzle-server/perf';
        for ($i = 0; $i < $total; $i++) {
            yield new Request('GET', $uri);
        }
    };
    
    $pool = new Pool($client, $requests(100), [
        'concurrency' => 5,
        'fulfilled' => function ($response, $index) {
            // this is delivered each successful response
        },
        'rejected' => function ($reason, $index) {
            // this is delivered each failed request
        },
    ]);
    
    // Initiate the transfers and create a promise
    $promise = $pool->promise();
    
    // Force the pool of requests to complete.
    $promise->wait();
    

Hoặc sử dụng một closure sẽ trả về một promise mỗi khi có lời gọi closure.
    
    
    $client = new Client();
    
    $requests = function ($total) use ($client) {
        $uri = 'http://127.0.0.1:8126/guzzle-server/perf';
        for ($i = 0; $i < $total; $i++) {
            yield function() use ($client, $uri) {
                return $client->getAsync($uri);
            };
        }
    };
    
    $pool = new Pool($client, $requests(100));
    

##Sử dụng Responses

Trong các ví dụ trước, chúng ta nhận được một biến `$response` hoặc chúng ta lấy được một response từ một promise. Đối tượng response thực hiện theo một PSR-7 response, `PsrHttpMessageResponseInterface`,và chứa nhiều thông tin hữu ích.

Bạn có thể lấy code trạng thái và lý do của response:
    
    
    $code = $response->getStatusCode(); // 200
    $reason = $response->getReasonPhrase(); // OK
    

Bạn có thể lấy các header từ response:
    
    
    // Check if a header exists.
    if ($response->hasHeader('Content-Length')) {
        echo "It exists";
    }
    
    // Get a header from the response.
    echo $response->getHeader('Content-Length');
    
    // Get all of the response headers.
    foreach ($response->getHeaders() as $name => $values) {
        echo $name . ': ' . implode(', ', $values) . "rn";
    }
    

Phần body của một response có thể được lấy bằng cách sử dụng phương thức `getBody`. Body có thể được dùng như một string, cast to a string,hoặc được sử dụng như một stream giống như object.
    
    
    $body = $response->getBody();
    // Implicitly cast the body to a string and echo it
    echo $body;
    // Explicitly cast the body to a string
    $stringBody = (string) $body;
    // Read 10 bytes from the body
    $tenBytes = $body->read(10);
    // Read the remaining contents of the body as a string
    $remainingBytes = $body->getContents();
    

## Query String Parameters

Bạn có thể cung cấp các thông số của chuỗi truy vấn với một request theo một số cách.

Bạn có thể thiết lập thông số của chuỗi truy vấn trong URI của request:
    
    
    $response = $client->request('GET', 'http://httpbin.org?foo=bar');
    

Bạn cũng có thể cụ thể thông số truy vấn sử dụng tuỳ chọn `query` request giống như một mảng.
    
    
    $client->request('GET', 'http://httpbin.org', [
        'query' => ['foo' => 'bar']
    ]);
    

Việc cung cấp các tuỳ chọn như một mảng sẽ sử dụng hàm của PHP `http_build_query` để định dạng lại câu truy vấn.

Và cuối cùng, bạn có thể cung cấp tuỳ chọn request `query` như một string.
    
    
    $client->request('GET', 'http://httpbin.org', ['query' => 'foo=bar']);
    

## Tải dữ liệu lên

Guzzle cung cấp một số phương pháp tải dữ liệu lên.

Bạn có thể gửi các request chứa một stream của dữ liệu thông qua một chuỗi, tài nguyên trả về từ `fopen`,hoặc thể hiện của một `PsrHttpMessageStreamInterface` đến các tuỳ chọn của `body`.
    
    
    // Provide the body as a string.
    $r = $client->request('POST', 'http://httpbin.org/post', [
        'body' => 'raw data'
    ]);
    
    // Provide an fopen resource.
    $body = fopen('/path/to/file', 'r');
    $r = $client->request('POST', 'http://httpbin.org/post', ['body' => $body]);
    
    // Use the stream_for() function to create a PSR-7 stream.
    $body = GuzzleHttpPsr7stream_for('hello!');
    $r = $client->request('POST', 'http://httpbin.org/post', ['body' => $body]);
    

Một cách đơn giản để upload dữ liệu JSON và thiết lập header thích hợp sử dụng tuỳ chọn `json` :
    
    
    $r = $client->request('PUT', 'http://httpbin.org/put', [
        'json' => ['foo' => 'bar']
    ]);
    

### POST/Form Requests

Ngoài việc chỉ định dữ liệu thô của một request sử dụng tuỳ chọn `body`, Guzzle cung cấp các abstraction hữu ích trên gửi dữ liệu POST.

#### Gửi các trường của form

Gửi các request POST `application/x-www-form-urlencoded` yêu cầu bạn chỉ định các trường của POST giống như một mảng trong tuỳ chọn `form_params`.
    
    
    $response = $client->request('POST', 'http://httpbin.org/post', [
        'form_params' => [
            'field_name' => 'abc',
            'other_field' => '123',
            'nested_field' => [
                'nested' => 'hello'
            ]
        ]
    ]);
    

#### Gửi các file form

Bạn có thể gửi các file cùng với một form (`multipart/form-data` POST requests),sử dụng `multipart`. `multipart` chấp nhận một mảng của các mảng kết hợp, nơi mỗi mảng kết hợp chứa các key dưới đây:

* name: (required, string) key để map với tên của trường trong form.
* contents: (required, mixed) cung cấp một chuỗi để gửi các nội dung của file như là một chuỗi, cung cấp một tài nguyên dạng fopen để stream các nội dung từ một strem PHP hoặc cung cấp một `PsrHttpMessageStreamInterface` để stream các nội dung theo  PSR-7 stream.
    
    
    $response = $client->request('POST', 'http://httpbin.org/post', [
        'multipart' => [
            [
                'name'     => 'field_name',
                'contents' => 'abc'
            ],
            [
                'name'     => 'file_name',
                'contents' => fopen('/path/to/file', 'r')
            ],
            [
                'name'     => 'other_file',
                'contents' => 'hello',
                'filename' => 'filename.txt',
                'headers'  => [
                    'X-Foo' => 'this is an extra header to include'
                ]
            ]
        ]
    ]);
    

## Cookies

Guzzle có thể duy trì một cookie session cho bạn nếu sử dụng `cookies` . Khi gửi một request, `cookies` phải được thiết lập một thể hiện của `GuzzleHttpCookieCookieJarInterface`.
    
    
    // Use a specific cookie jar
    $jar = new GuzzleHttpCookieCookieJar;
    $r = $client->request('GET', 'http://httpbin.org/cookies', [
        'cookies' => $jar
    ]);
    

Bạn có thể thiết lập `cookies` là `true` trong khởi tạo của client nếu bạn muốn sử dụng cookie jar cho tất cả các request.
    
    
    // Use a shared client cookie jar
    $client = new GuzzleHttpClient(['cookies' => true]);
    $r = $client->request('GET', 'http://httpbin.org/cookies');
    

## Chuyển hướng

Guzzle sẽ tự động theo các chuyển hướng trừ bạn không cho phép. Bạn có thể tuỳ chỉnh lại hành động chuyển hướng với tuỳ chọn `allow_redirects`.

* Thiết lập `true` để cho phép chuyển hướng bình thường với số lần chuyển hướng tối đa là 5. Nó được cài đặt mặc định.
* Thiết lập `false` để vô hiệu hoá các chuyển hướng.
* Pass an associative array containing the 'max' key to specify the maximum number of redirects and optionally provide a 'strict' key value to specify whether or not to use strict RFC compliant redirects (meaning redirect POST requests with POST requests vs. doing what most browsers do which is redirect POST requests with GET requests).
* chưa dịch nè
    
    
    $response = $client->request('GET', 'http://github.com');
    echo $response->getStatusCode();
    // 200
    

Theo dõi ví dụ đưa ra các chuyển hướng có thể bị vô hiệu hoá.
    
    
    $response = $client->request('GET', 'http://github.com', [
        'allow_redirects' => false
    ]);
    echo $response->getStatusCode();
    // 301
    

## Exceptions

Guzzle ném ra các exception cho các lỗi xảy ra trong quá trình chuyển đổi.

* Trong sự kiện một mạng lỗi(connection timeout, DNS errors, etc.), một `GuzzleHttpExceptionRequestException` được ném ra. Exception này kế thừa từ `GuzzleHttpExceptionTransferException`. Exception này sẽ bắt bất kỳ exception mà có thể được ném ra trong quá trình chuyển đổi các request.
    
        use GuzzleHttpPsr7;
    use GuzzleHttpExceptionRequestException;
    
    try {
        $client->request('GET', 'https://github.com/_abc_123_404');
    } catch (RequestException $e) {
        echo Psr7str($e->getRequest());
        if ($e->hasResponse()) {
            echo Psr7str($e->getResponse());
        }
    }
    

* Một exception `GuzzleHttpExceptionConnectException` được ném ra khi mạng lỗi. Exception này được kế thừa từ `GuzzleHttpExceptionRequestException`.
* Một `GuzzleHttpExceptionClientException` được ném ra cho lỗi 400nếu tuỳ chọn `http_errors` được thiết lập true. Exception này kế thừa từ `GuzzleHttpExceptionBadResponseException`và `GuzzleHttpExceptionBadResponseException` kế thừa từ `GuzzleHttpExceptionRequestException`.
    
        use GuzzleHttpExceptionClientException;
    
    try {
        $client->request('GET', 'https://github.com/_abc_123_404');
    } catch (ClientException $e) {
        echo Psr7str($e->getRequest());
        echo Psr7str($e->getResponse());
    }
    

* Một `GuzzleHttpExceptionServerException` được ném ra chô lỗi 500 nếu tuỳ chọn `http_errors` thiết lập true. Exception kế thừa từ`GuzzleHttpExceptionBadResponseException`.
* MỘt `GuzzleHttpExceptionTooManyRedirectsException` được ném ra khi có quá nhiều chuyển hướng được đi theo. Exception kế thừa từ `GuzzleHttpExceptionRequestException`.

Tất cả exception trên kế thừa từ`GuzzleHttpExceptionTransferException`.

## Biến môi trường

Guzzle đưa ra một vài biến môi trường có thể sử dụng để tuỳ chỉnh thư viện

`GUZZLE_CURL_SELECT_TIMEOUT`
: Kiểm soát thời lượng tính bằng giây một xử lý curl_multi_* sẽ sử dụng khi lựa chọn trên curl handles sử dụng `curl_multi_select()`. Một vài hệ thống có vấn đề với thực thi của `curl_multi_select()` nới gọi hàm này dẫn tới thời gian chờ tối đa bị hết.

`HTTP_PROXY`
: 

Xác định proxy sử dụng khi gửi các request sử dụng phương thức "http".

Chú ý :Bởi vì biến HTTP_PROXY có thể chứa người dùng tuỳ ý khi nhập vào từ môi trường (CGI), biến chỉ được sử dụng trên CLI SAPI. Xem thêm thông tin.

`HTTPS_PROXY`
: Xác định proxy sử dụng khi gửi các request thông qua phương thức "https".

thiếu phần cuối

[1]: http://docs.guzzlephp.org/overview.html#installation
[2]: http://tools.ietf.org/html/rfc3986#section-5.2
[3]: http://docs.guzzlephp.org/handlers-and-middleware.html
[4]: https://promisesaplus.com/
[5]: https://github.com/guzzle/promises

  
