# Path traversal

# What is path traversal?
> Path traversal là gì ? 

Path traversal is also known as directory traversal. These vulnerabilities enable an attacker to read arbitrary files on the server that is running an application. This might include:
> Path traversal còn được gọi là directory traversal. Những lỗ hổng này cho phép kẻ tấn công đọc bất kỳ tệp nào trên máy chủ đang chạy ứng dụng. Điều này có thể bao gồm: 

- Application code and data.
> Mã nguồn và dữ liệu của ứng dụng
- Credentials for back-end systems.
> Thông tin xác thực của hệ thống back-end
- Sensitive operating system files.
> Các tệp nhạy cảm của hệ điều hành 

In some cases, an attacker might be able to write to arbitrary files on the server, allowing them to modify application data or behavior, and ultimately take full control of the server.
> Trong một số trường hợp, kẻ tấn công có thể tùy ý ghi file nào trên server, cho phép họ thay đổi dữ liệu hoặc hành vi của ứng dụng, và cuối cùng chiếm toàn quyền kiểm soát máy chủ.  

# Reading arbitrary files via path traversal
> Đọc bất kỳ tệp nào qua path traversal 

Imagine a shopping application that displays images of items for sale. This might load an image using the following HTML:
> Hãy tưởng tượng rằng một ứng dụng mua sắm hiện thị hình ảnh của các mặt hàng đang bán. Ứng dụng này có thể tải một hình ảnh bằng cách sử dụng một HTML như sau:

```<img src="/loadImage?filename=218.png">```

The loadImage URL takes a filename parameter and returns the contents of the specified file. The image files are stored on disk in the location /var/www/images/. To return an image, the application appends the requested filename to this base directory and uses a filesystem API to read the contents of the file. In other words, the application reads from the following file path:
> URL loadImage nhận một tham số filename và trả về nội dung của file được chỉ định. Các file ảnh được lưu trên ổ đĩa tại vị trí `/var/www/images/`. Để trả về ảnh, ứng dụng sẽ nối thêm tên được tệp được request vào đường dẫn cơ sở này và sử dụng một API hệ thống để độc nội dung của tệp. Nói cách khác ứng dụng đọc từ đường dẫn tệp sau:

`/var/www/images/218.png`

This application implements no defenses against path traversal attacks. As a result, an attacker can request the following URL to retrieve the /etc/passwd file from the server's filesystem:
> Ứng dụng này không thực hiện bất kỳ cơ chế phòng thủ nào chống lại các cuộc tấn công path traversal. Kết quả là, kẻ tấn công có thể yêu cầu URL sau để truy xuất tệp /etc/passwd từ hệ thống tệp của máy chủ: 

`https://insecure-website.com/loadImage?filename=../../../etc/passwd`

This causes the application to read from the following file path:
> Điều này khiến ứng dụng đọc từ đường dẫn tệp sau:

`/var/www/images/../../../etc/passwd`

The sequence `../` is valid within a file path, and means to step up one level in the directory structure. The three consecutive `../` sequences step up from `/var/www/images/` to the filesystem root, and so the file that is actually read is:

> Chuỗi `../` là hợp lệ trong một đường dẫn tệp và có ý nghĩa là đi lên một cấp trong cấu trúc thư mục. Ba chuỗi liên tiếp `../` liên tiếp sẽ di chuyển từ `/var/www/images/` lên thư mục gốc (root) của hệ thống tệp, và do đó tệp thực sự được đọc là 

`/etc/passwd`

On Unix-based operating systems, this is a standard file containing details of the users that are registered on the server, but an attacker could retrieve other arbitrary files using the same technique.
> Trên các hệ điều hành dựa trên Unix, đây là một tệp tiêu chuẩn chứa thông tin chi tiết về người dùng đã đăng ký trên máy chủ, nhưng kẻ tấn công có thể truy xuất các tệp tùy ý khác bằng cách sử dụng cùng một kỹ thuật.

On Windows, both `../` and `..\` are valid directory traversal sequences. The following is an example of an equivalent attack against a Windows-based server:
> Trên Windows, cả `../` và `..\` đều là chuỗi thư mục hợp lệ. Dưới đây là một cuộc tấn công tương tự nhắm vào máy chủ Windows:

`https://insecure-website.com/loadImage?filename=..\..\..\windows\win.ini`

# Common obstacles to exploiting path traversal vulnerabilities
> Những trở ngại phổ biến khi khai thác lỗ hổng path traversal  

Many applications that place user input into file paths implement defenses against path traversal attacks. These can often be bypassed.
> Rất nhiều ứng dụng đặt input của người vào đường dẫn tệp và thực hiện các cơ chế phòng thủ chống lại các cuộc tấn công path traversal. Tuy nhiên, nhưng cơ chế này có thể bị vượt qua.

If an application strips or blocks directory traversal sequences from the user-supplied filename, it might be possible to bypass the defense using a variety of techniques.
> Nếu ứng dụng loại bỏ hoặc ngăn chặn chuỗi thư mục khỏi input của người dùng, thì kẻ tấn công có thể vượt qua các cơ chế phòng thủ này bằng nhiều kĩ thuật khác nhau.

You might be able to use an absolute path from the filesystem root, such as filename=/etc/passwd, to directly reference a file without using any traversal sequences.
> Bạn có thể sử dụng đường dẫn tuyệt đối từ thư mục gốc của hệ thống tệp, chẳng hạn như filename=/etc/passwd, để tham chiếu trực tiếp đến một tệp mà không cần sử dụng bất kỳ chuỗi thư mục nào.    

You might be able to use nested traversal sequences, such as ....// or ....\/. These revert to simple traversal sequences when the inner sequence is stripped.
> Bạn có thể sử dụng đường dẫn các chuỗi duyệt thư mục lồng nhau, chẳng hạn như ....// hoặc ....\\. Những chuỗi này sẽ những phần chuỗi này sẽ quay lại trở thành các chuỗi duyệt thư mục thông thường khi phần chuỗi bên trong bị loại bỏ.

In some contexts, such as in a URL path or the `filename` parameter of a `multipart/form-data` request, web servers may strip any directory traversal sequences before passing your input to the application. You can sometimes bypass this kind of sanitization by URL encoding, or even double URL encoding, the `../` characters. This results in `%2e%2e%2f` and `%252e%252e%252f` respectively. Various non-standard encodings, such as `..%c0%af` or `..%ef%bc%8f`, may also work.
> Trong một số ngữ cảnh, chẳng hạn như trong đường dẫn URL hoặc tham số `filename` của `multipart/form-data` request, các web server có thể loại bỏ bất kỳ chuỗi traversal nào trước khi chuyển input của người dùng vào ứng dụng. Kẻ tấn công đôi khi có thể vượt qua cơ chế lọc này bằng cách mã hóa URL, hoặc thậm chí có thể là mã hóa URL 2 lần chuỗi `../` thành `%2e%2e%2f` và `%252e%252e%252f`. Một số cách mã hóa không chuẩn, chẳng hạn như `..%c0%af` hoặc `..%ef%bc%8f`, cũng có thể hiệu quả. 

For Burp Suite Professional users, Burp Intruder provides the predefined payload list Fuzzing - path traversal. This contains some encoded path traversal sequences that you can try.
> Đối với người dùng Burp Suite Professional, Burp Intruder cung cấp danh sách tải trọng (payload) được định nghĩa sẵn là Fuzzing - path traversal. Danh sách này chứa một số chuỗi duyệt thư mục đã được mã hóa mà bạn có thể thử.

An application may require the user-supplied filename to start with the expected base folder, such as `/var/www/images`. In this case, it might be possible to include the required base folder followed by suitable traversal sequences. For example: `filename=/var/www/images/../../../etc/passwd`.
> Ứng dụng yêu cầu tên tệp do người dùng cung cấp bắt đầu bằng thư mục cơ sở dự kiến, chẳng hạn như `/var/www/images`. Trong trường hợp này, vẫn có thể thực hiện được bằng cách bao gồm thư mục cơ sở bắt buộc đó theo sau là các chuỗi duyệt path traversal phù hợp.

Ví dụ `filename=/var/www/images/../../../etc/passwd`

An application may require the user-supplied filename to end with an expected file extension, such as `.png`. In this case, it might be possible to use a null byte to effectively terminate the file path before the required extension. For example: `filename=../../../etc/passwd%00.png`.
> Ứng dụng có thể yêu cầu tên tệp do người dùng cung cấp kết thúc bằng phần mở rộng tệp dự kiến, chẳng hạn như `.png`. Trong trường hợp này, có thể sử dụng null byte để loại bỏ hiệu quả đường dẫn tệp trước phần mở rộng cần thiết. Ví dụ: `filename=../../../etc/passwd%00.png`. 
