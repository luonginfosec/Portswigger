# Vulnerabilities in other authentication mechanisms
> Lỗ hổng trong cơ chế xác thực khác 

In addition to the basic login functionality, most websites provide supplementary functionality to allow users to manage their account. For example, users can typically change their password or reset their password when they forget it. These mechanisms can also introduce vulnerabilities that can be exploited by an attacker.
> Bên cạnh chức năng đăng nhập cơ bản, hầu hết các trang web đều cung cấp chức năng bổ sung cho phép người dùng quản lý tài khoản. Ví dụ, người dùng thường có thể thay đổi mật khẩu hoặc đặt lại mật khẩu khi quên mật khẩu. Những cơ chế này cũng có thể tạo ra các lỗ hổng bị kẻ tấn công khai thác.

Websites usually take care to avoid well-known vulnerabilities in their login pages. But it is easy to overlook the fact that you need to take similar steps to ensure that related functionality is equally as robust. This is especially important in cases where an attacker is able to create their own account and, consequently, has easy access to study these additional pages.
> Các trang web thường cẩn trọng để tránh các lỗ hổng đã biết trên trang đăng nhập của họ. Nhưng rất dễ bỏ qua thực tế rằng bạn cũng cần thực hiện các bước tương tự để đảm bảo chức năng liên quan cũng mạnh như vậy. Điều này đặc biệt quan trọng trong trường hợp kẻ tấn công có thể tạo tài khoản riêng và do đó dễ dàng truy cập để nghiên cứu các trang bổ sung này.

# Keeping users logged in
> Giữ cho người dùng luôn đăng nhập

A commom feature is the option to stay logged in even after closing a browser session. This is usually a simple checkbox labeled something like "Remember me" or "Keep me logged in"
> Một tính năng phổ biến là tùy chọn giữ đăng nhập ngay cả sau khi đóng phiên trình duyệt. Thông thường đây là checkbox "Nhớ tôi" hoặc "Giữ tôi đăng nhập".

This functionality is often implemented by generating a "remember me" token of some kind, which is then stored in a persistent cookie. As possessing this cookie effectively allows you to bypass the entire login process, it is best practice for this cookie to be impractical to guess. However, some websites generate this cookie based on a predictable concatenation of static values, such as the username and a timestamp. Some even use the password as part of the cookie. This approach is particularly dangerous if an attacker is able to create their own account because they can study their own cookie and potentially deduce how it is generated. Once they work out the formula, they can try to brute-force other users' cookies to gain access to their accounts.
> Chức năng này thường được thực hiện bằng cách tạo ra một token "Remember me" nào đó, sau đó được lưu trữ trong cookie lưu trữ lâu dài. Vì việc sở hữu cookie này cho phép bạn bỏ qua toàn bộ quá trình đăng nhập, nên tốt nhất là cookie này không thực tế để đoán. Tuy nhiên, một số trang web tạo cookie này dựa trên sự kết hợp có thể dự đoán được của các giá trị tĩnh, như tên người dùng và dấu thời gian. Một số thậm chí còn sử dụng mật khẩu như một phần của cookie. Cách tiếp cận này đặc biệt nguy hiểm nếu kẻ tấn công có thể tạo tài khoản riêng vì họ có thể nghiên cứu cookie của mình và có thể suy ra cách nó được tạo ra. Khi họ đã tìm ra công thức, họ có thể thử brute force cookie của người dùng khác để truy cập vào tài khoản của mình.

Some websites assume that if the cookie is encrypted in some way it will not be guessable even if it does use static values. While this may be true if done correctly, naively "encrypting" the cookie using a simple two-way encoding like Base64 offers no protection whatsoever. Even using proper encryption with a one-way hash function is not completely bulletproof. If the attacker is able to easily identify the hashing algorithm, and no salt is used, they can potentially brute-force the cookie by simply hashing their wordlists. This method can be used to bypass login attempt limits if a similar limit isn't applied to cookie guesses.
> Một số trang web giả định rằng nếu cookie được mã hóa theo cách nào đó thì sẽ không thể đoán được ngay cả khi sử dụng giá trị tĩnh. Mặc dù điều này có thể đúng nếu làm đúng, việc "mã hóa" cookie một cách đơn giản bằng mã hóa hai chiều đơn giản như Base64 không mang lại bất kỳ sự bảo vệ nào. Ngay cả khi sử dụng mã hóa đúng với hàm băm một chiều cũng không hoàn toàn chắc chắn. Nếu kẻ tấn công có thể dễ dàng nhận diện thuật toán băm và không sử dụng salt, họ có thể brute force cookie chỉ bằng cách băm danh sách từ của mình. Phương pháp này có thể được sử dụng để vượt qua giới hạn số lần đăng nhập nếu không áp dụng giới hạn tương tự cho việc đoán cookie.

Even if the attacker is not able to create their own account, they may still be able to exploit this vulnerability. Using the usual techniques, such as XSS, an attacker could steal another user's "remember me" cookie and deduce how the cookie is constructed from that. If the website was built using an open-source framework, the key details of the cookie construction may even be publicly documented.
> Ngay cả khi kẻ tấn công không thể tạo tài khoản riêng, họ vẫn có thể khai thác lỗ hổng này. Sử dụng các kỹ thuật thông thường như XSS, kẻ tấn công có thể đánh cắp cookie "Duy trì đăng nhập" của người dùng khác và suy luận cách cookie được xây dựng từ đó. Nếu trang web được xây dựng bằng một framework mã nguồn mở, các chi tiết chính của việc xây dựng cookie thậm chí có thể được công khai hóa.

In some rare cases, it may be possible to obtain a user's actual password in cleartext from a cookie, even if it is hashed. Hashed versions of well-known password lists are available online, so if the user's password appears in one of these lists, decrypting the hash can occasionally be as trivial as just pasting the hash into a search engine. This demonstrates the importance of salt in effective encryption.
> Trong một số trường hợp hiếm, có thể lấy được mật khẩu thực sự của người dùng dưới dạng văn bản rõ ràng từ cookie, ngay cả khi nó bị băm. Các phiên bản băm của danh sách mật khẩu nổi tiếng có sẵn trực tuyến, nên nếu mật khẩu của người dùng xuất hiện trong một trong các danh sách này, việc giải mã băm đôi khi chỉ đơn giản là dán mã băm vào công cụ tìm kiếm. Điều này cho thấy tầm quan trọng của salt trong mã hóa hiệu quả.

# Resetting user passwords
> Đặt lại mật khẩu người dùng

In practice some users will forget their password, so it is common to have a way for them to reset it. As the usual password-based authentication is obviously impossible in this scenario, websites have to rely on alternative methods to make sure that the real user is resetting their own password. For this reason, the password reset functionality is inherently dangerous and needs to be implemented securely.
> Trên thực tế, một số người dùng sẽ quên mật khẩu, nên việc có cách đặt lại mật khẩu là điều phổ biến. Vì xác thực dựa trên mật khẩu thông thường rõ ràng là không thể trong trường hợp này, các trang web phải dựa vào các phương pháp thay thế để đảm bảo người dùng thật đang đặt lại mật khẩu của chính họ. Vì lý do này, chức năng đặt lại mật khẩu vốn dĩ rất nguy hiểm và cần được triển khai một cách an toàn.

There are a few different ways that this feature is commonly implemented, with varying degrees of vulnerability.
> Có một vài cách khác nhau mà tính năng này thường được triển khai, với mức độ dễ bị tấn công khác nhau.

# Sending passwords by email
> Gửi mật khẩu qua email

It should go without saying that sending users their current password should never be possible if a website handles passwords securely in the first place. Instead, some websites generate a new password and send this to the user via email.
> Việc gửi mật khẩu hiện tại cho người dùng sẽ không bao giờ khả thi nếu một trang web đã xử lý mật khẩu một cách an toàn ngay từ đầu. Thay vào đó, một số trang web tạo mật khẩu mới và gửi cho người dùng qua email

Generally speaking, sending persistent passwords over insecure channels is to be avoided. In this case, the security relies on either the generated password expiring after a very short period, or the user changing their password again immediately. Otherwise, this approach is highly susceptible to man-in-the-middle attacks.
> Nói chung, cần tránh gửi mật khẩu lâu dài qua các kênh không an toàn. Trong trường hợp này, bảo mật phụ thuộc vào việc mật khẩu được tạo hết hạn sau một khoảng thời gian rất ngắn, hoặc người dùng thay đổi mật khẩu ngay lập tức. Nếu không, cách tiếp cận này rất dễ bị tấn công kiểu man-in-middle.

Email is also generally not considered secure given that inboxes are both persistent and not really designed for secure storage of confidential information. Many users also automatically sync their inbox between multiple devices across insecure channels.
> Email nói chung cũng không được coi là an toàn vì hộp thư đến vừa có tính lâu dài và không thực sự được thiết kế để lưu trữ thông tin bảo mật. Nhiều người dùng cũng tự động đồng bộ hộp thư đến của họ giữa nhiều thiết bị qua các kênh không an toàn.

Resetting passwords using a URL 
> Đặt lại mật khẩu bằng URL

A more robust method of resetting passwords is to send a unique URL to users that takes them to a password reset page. Less secure implementations of this method use a URL with an easily guessable parameter to identify which account is being reset, for example:

```
http://vulnerable-website.com/reset-password?user=victim-user
```

> Một phương pháp mạnh mẽ hơn để đặt lại mật khẩu là gửi một URL duy nhất cho người dùng, dẫn họ đến trang đặt lại mật khẩu. Các triển khai kém an toàn hơn của phương pháp này sử dụng URL với tham số dễ đoán để xác định tài khoản nào đang được đặt lại.

In this example, an attacker could change the user parameter to refer to any username they have identified. They would then be taken straight to a page where they can potentially set a new password for this arbitrary user.
> Trong ví dụ này, kẻ tấn công có thể thay đổi tham số người dùng để tham chiếu đến bất kỳ tên người dùng nào họ đã xác định. Sau đó, họ sẽ được đưa thẳng đến một trang nơi có thể đặt mật khẩu mới cho người dùng tùy ý này.

A better implementation of this process is to generate a high-entropy, hard-to-guess token and create the reset URL based on that. In the best case scenario, this URL should provide no hints about which user's password is being reset.

```
http://vulnerable-website.com/reset-password?token=a0ba0d1cb3b63d13822572fcff1a241895d893f659164d4cc550b421ebdd48a8
```
> Cách triển khai tốt hơn cho quá trình này là tạo ra một token có entropy cao, khó đoán và tạo URL reset dựa trên đó. Trong trường hợp tốt nhất, URL này sẽ không cung cấp bất kỳ gợi ý nào về mật khẩu của người dùng nào đang được đặt lại.

When the user visits this URL, the system should check whether this token exists on the back-end and, if so, which user's password it is supposed to reset. This token should expire after a short period of time and be destroyed immediately after the password has been reset.
> Khi người dùng truy cập URL này, hệ thống nên kiểm tra xem token này có tồn tại trên back-end hay không và nếu có, mật khẩu của người dùng nào sẽ được đặt lại. Token này sẽ hết hạn sau một thời gian ngắn và sẽ bị hủy ngay sau khi mật khẩu được đặt lại.

However, some websites fail to also validate the token again when the reset form is submitted. In this case, an attacker could simply visit the reset form from their own account, delete the token, and leverage this page to reset an arbitrary user's password.
> Tuy nhiên, một số trang web không xác thực lại token khi gửi biểu mẫu đặt lại. Trong trường hợp này, kẻ tấn công có thể truy cập vào biểu mẫu đặt lại từ tài khoản của mình, xóa token và tận dụng trang này để đặt lại mật khẩu của một người dùng bất kỳ.

If the URL in the reset email is generated dynamically, this may also be vulnerable to password reset poisoning. In this case, an attacker can potentially steal another user's token and use it change their password. 
> Nếu URL trong email đặt lại được tạo động, điều này cũng có thể dễ bị nhiễm độc đặt lại mật khẩu. Trong trường hợp này, kẻ tấn công có thể đánh cắp token của người dùng khác và dùng nó để thay đổi mật khẩu của họ.

# Changing user passwords
> Thay đổi mật khẩu người dùng

Typically, changing your password involves entering your current password and then the new password twice. These pages fundamentally rely on the same process for checking that usernames and current passwords match as a normal login page does. Therefore, these pages can be vulnerable to the same techniques.
> Thông thường, việc thay đổi mật khẩu bao gồm việc nhập mật khẩu hiện tại và sau đó là mật khẩu mới hai lần. Về cơ bản, các trang này dựa trên cùng một quy trình để kiểm tra xem tên người dùng và mật khẩu hiện tại có khớp với trang đăng nhập thông thường hay không. Do đó, các trang này có thể dễ bị tấn công bởi các kỹ thuật tương tự.

Password change functionality can be particularly dangerous if it allows an attacker to access it directly without being logged in as the victim user. For example, if the username is provided in a hidden field, an attacker might be able to edit this value in the request to target arbitrary users. This can potentially be exploited to enumerate usernames and brute-force passwords.
> Chức năng thay đổi mật khẩu có thể đặc biệt nguy hiểm nếu nó cho phép kẻ tấn công truy cập trực tiếp mà không cần đăng nhập với tư cách là người dùng nạn nhân. Ví dụ: nếu tên người dùng được cung cấp trong trường ẩn, kẻ tấn công có thể chỉnh sửa giá trị này trong yêu cầu nhắm mục tiêu người dùng tùy ý. Điều này có khả năng có thể bị khai thác để liệt kê tên người dùng và mật khẩu mạnh mẽ.


