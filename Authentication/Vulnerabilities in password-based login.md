# Vulnerabilities in password-based login
> Lỗ hổng trong đăng nhập dựa trên mật khẩu

For websites that adopt a password-based login process, users either register for an account themselves or they are assigned an account by an administrator. This account is associated with a unique username and a secret password, which the user enters in a login form to authenticate themselves.
> Đối với các trang web áp dụng quy trình đăng nhập dựa trên mật khẩu, người dùng tự đăng ký tìa khoản hoặc họ được quản trị viên chỉ định tài khoản. Tài khoản này đưuọc liên kết với một tên người dùng duy nhất và mật khẩu bí mật mà người dùng nhập vào biểu mẫu đăng nhập để xác thực bản thân.

In this scenario, the fact that they know the secret password is taken as sufficient proof of the user's identity. This means that the security of the website is compromised if an attacker is able to either obtain or guess the login credentials of another user. 
> Trong trường hợp này, việc họ biết mật khẩu bí mật được gọi là bằng chứng đầy đủ về danh tính của người dùng. Điều này có nghĩa là tính bảo mật của trang web bị xâm phạm nếu kể tấn công có thể lấy hoặc đoán thông tin đăng nhập của người dùng khác.

This can be achieved in a number of ways. The following sections show how an attackers can use brute-force attacks, and some of the flaws in brute-force protection. You''ll also learn about the vulnerabilities in HTTP basic authentication. 
> Điều này có thể đạt được theo nhiều cách. Các phần sau đây cho thấy cách kẻ tấn công có thể sử dụng các cuộc tấn công brute-force và một số lỗ hổng trong bảo vệ khỏi tấn công brute-force. Và một số lỗ hổng trong xác thực cơ HTTP cơ bản.

## Brute-force attacks
> Tấn công brute-force 

A brute-force attack is when an attacker uses a system of trial and error to guess valid user credentials. These attacks are typically automated using wordlists of usernames and passwords. Automating this process, especially using dedicated tools, potentially enables an attacker to make vast numbers of login attempts at high speed.
> Tấn công brute-force là khi kẻ tấn công sử dụng hệ thống thử nhiều lần để đoán thông tin đăng nhập người dùng hợp lệ. Các cuộc tấn công này thường được tự động hóa bằng các công cụ chuyên dụng, có khả năng cho phép kẻ tấn công thực hiện một số lượng lớn đăng nhập với tốc độ cao.

Brute-forcing is not always just a case of making completely random guesses at username and passwords. By also using basic logic or publicly available knowledge, attackers can fine-tune brute-force attacks to make much more educated guesses. This considerably increases the efficiency of such attacks. Websites that rely on password-based login as their sole method of authenticating users can be highly vulnerable if they do not implement sufficient brute-force protection. 
> Tấn công brute-force không phải lúc nào cũng chỉ là trường hợp hoàn toàn ngẫu nghiên về đoán hoàn toàn tên người dùng và mật khẩu. Bằng cách sử dụng logic cơ bản hoặc kiến thức có sẵn công khai, những kẻ tấn công có thể tinh chỉnh các cuộc tấn công brute-force để đưa ra các phỏng đoán khoa học hơn nhiều. Điều này làm tăng đáng kể hiệu quả của các cuộc tấn công này. Các trang web dựa vào đăng nhập dựa trên mật khẩu làm phương pháp duy nhất để xác thực người dùng có thể rất dễ bị tấn công nếu họ không thực hiện đủ phương pháp bảo vệ tấn công brute-force.

## Brute-forcing usernames
> Tấn công brute-force tên người dùng

Usernames are especially easy to guess if they conform to a recognizable pattern, such as an email address. For example, it is very commom to see business logins in the format firstname.lastname@somecompany.com. However, even if there is no obbious pattern, sometimes even high-privileged account are created using predictable usernames, such as admin or administrator.
> Tên người dùng đặc biệt dễ đoán nếu chúng phù hợp với một mẫu dễ nhận biết, chẳng hạn như địa chỉ email. Ví dụ rất phổ biến để để thấy thông tin đăng nhập doanh nghiệp ở định dạng firstname.lastname@somecompany.com. Tuy nhiên ngay cả khi không có mẫu rõ ràng, đôi khi việc đoán các tài khoản có đặc quyền cao cũng được tạo bằng tên người dùng có thể dự đoán được chẳng hạn như admin hoặc administrator. 

During auditing, check whether the website discloses potential usernames publicly. For example, are you able to access user profiles without logging in? Even if the actual content of the profiles is hidden, the name used in the profile is sometimes the same as the login username. You should also check HTTP response to see if any email addresses are disclosed. Occsionally, reponses contain email addresses of high-privileged users, such as administrators or IT support.
> Trong quá trình kiểm tra, hãy kiểm tra xem trang web có tiết lộ công khai tên người dùng hay không. Ví dụ có thể kiểm tra truy cập hồ sơ người dùng mà không cần đăng nhập không? Ngay cả khi nội dung thực tế của hồ sơ người dùng bị ẩn, tên được sử dụng trong hồ sơ đôi khi giống với tên người dùng đăng nhập. Cũng nên kiểm tra phản hồi HTTP để xem có địa chỉ nào bị tiết lộ hay không. Đôi khi, phản hồi chứa địa chỉ email của người dùng có đặc quyền cao chẳng hạn như quản trị viên hoặc bộ phận hỗ trợ CNTT.

## Brute-forcing passwords
> Tấn công brute-force mật khẩu

Passwords can similary be brute-forced, with the difficulty varying based on the strength of the password. Many websites adopt some form of password policy, which forces users to create high-entropy passwords that are, theoretically at least, harder to crack using brute-force alone. This typicallly involves enforcing password with: 
- A minium number of characters 
- A mixture of lower and uppercase letters
- At least one special character 
> Mật khẩu cũng có thể bị tấn công brute-force tương tự, với độ khó thay đổi dựa trên chiều dài của mật khẩu. Nhiều trang web áp dụng một số chính sách về mật khẩu, buộc người dùng phải tạo mật khẩu có hàm lượng entropy cao, ít nhất về mặt lý thyết, khó bẻ khóa hơn chỉ dựa vào cách sử dụng brute-force. Điều này thường bao gồm việc thực thi mật khẩu với:
- Số ký tự tối thiểu 
- Hỗn hợp chữ thường và chữ hoa 
- Ít nhất một ký tự đặc biệt

However, while high-entropy passwords are difficult for computers alone to crack, we can use a basic knowledge of human behaviour to exploit vulnerabilities that users unwittingly introduce to this system. Rather than creating a strong password with a random combination of characters, users often take a password that they can remember and try to crowbar it into fifting the password policy. For example, if mypassword is not allowed, users may try something like Mypassword1! or Myp4$$w0rd instead.    
> Tuy nhiên, trong khi mật khẩu có độ entropy cao khó có thể bẻ khóa chỉ bằng máy tính, chúng ta có thể sử dụng kiến thức cơ bản về hành vi con người để khai thác lỗ hổng mà người dùng vô tình tạo ra cho hệ thống. Thay vì tạo một mật khẩu mạnh với sự kết hợp ngẫu nhiên của các ký tự, người dùng thường lấy một mật khẩu mà họ có thê rnhớ và cố gắng làm cho nó phù hợp với chính sách mật khẩu. Ví dụ nếu mypassword không được phép, người dùng có thể thử một cái gì đó như Mypassword1! hoặc Myp4$$w0rd thay thế.

This knowledge of likely credentials and predictable patterns means that brute-force attacks can often be uch more sophisticated, and thereforce effective, than simple iterating through every possible combination of characters.
> Kiến thức này về thông tin đăng nhập có khả năng và các mẫu có thể dự đoán được có nghĩa là các cuộc tấn công brute-force thường có thể tinh vi hơn và do đó hiệu quả hơn so với việc lặp lại qua mọi sự kết hợp có thể có của các ký tự. 

## Username enumeration 
> Liệt kê tên người dùng

Username enumeration is when an attacker is able to observe changes in the website's behaviour in order to identify weather a given username is valid.
> Liệt kê tên người dùng là khi kẻ tấn công có thể quan sát những thay đổi trong hành vi của trang web để xác định xem tên người dùng nhất định có hợp lệ hay không.

Username enumeration typically occurs either on the login page, for example, when you enter a valid username but an incorrect password, or on registration forms when you enter a username that is already taken. This greatly reduces the time and effort required to brute-force a login because the attacker is able to quickly generate a shortlist of valid usernames.
> Liệt kê tên người dùng thường xảy ra trên trang đăng nhập, ví dụ: khi bạn nhập tên người dùng hợp lệ nhưng mật khẩu không chính xác, hoặc trên biểu mẫu đăng ký khi bạn nhập tên người dùng đã được sử dụng. Điều này làm giảm đáng kể thời gian và công sức cần thiết để tấn công brute-force có thể nhanh chóng tạo ra một danh sách các tài khoản người dùng hợp lệ.

Status code: During a brute-force attack, the returned HTTP status code is likely to be the same for the vast majority of guesses because most of them will be wrong. If a guess returns a different status code, this is a strong indication that the username was correct. It is best practice for websites to always return the same status code regardless of the outcome, but practice is not always followed.
> Mã trạng thái: Trong một cuộc tấn công brute-force, mã trạng thái HTTP được trả về có thể gần giống nhau đối với phần lớn các dự đoán vì hầu hết chúng sẽ sai. Nếu dự đoán trả về mã trạng thái khác, đây là dấu hiệu rõ ràng cho thấy tên người dùng là chính xác. Cách tốt nhất là các trang web luôn trả về cùng một mã trạng thái bất kể kết quả như thế nào, nhưng thực tiễn điều này không phải lúc nào cũng được tuân thủ.

Error messages: Sometime the returned error message is different depending on whether both the username AND password are incorrect or only the password is incorrect. It is best practice for websites to use identical, generic messages in both cases, but small typing errors sometimes creep in. Just one character out of place makes the two messages distinct, even in the cases where the character is not visible on the rendered page.
> Thông báo lỗi: Thỉnh thoảng thông báo lỗi được trả về khác nhau tùy thuộc vào việc cả tên người dùng và mật khẩu đều sai hay chỉ có mật khẩu sai. Cách tốt nhất là các trang web sử dụng các thông báo chung chung, giống nhau trong cả hai trường hợp, nhưng đôi khi có thể có những lỗi nhỏ trong quá trình gõ. Chỉ cần một kí tự không đúng vị trí sẽ làm cho hai thông điệp trở nên khác biệt, ngay cả trong trường hợp không hiển thị trên trang được hiện thị.

Response times: If most of the requests were handled in a similar response time, any that deviate from this suggest that something diffrent was happening behind the scenes. This is another indication that the guessed username might be correct. For example, a website might only check whether the password is correct if the username is valid. This extra step might cause a slight increase in the reponse time. This may be subtle, but an attacker can make this delay more obvious by entering an excessively long password that the websites takes noticeably longer to handle.
> Thời gian phản hồi: Nếu hầu hết các yêu cầu được xử lý với thời gian tương tự, thì bất kỳ yêu cầu nào khác với điều này cho thấy có điều gì đó khác biệt đang xảy ra phía sau. Đây là một dấu hiệu cho thấy tên người dùng được đoán có thể đúng. Ví dụ một trang web có thể chỉ kiểm tra mật khẩu nếu tên người dùng hợp lệ. Bước bổ sung này có thể làm tăng nhẹ thời gian phản hồi. Điều này có thể ít rõ ràng, nhưng kẻ tấn công có thể làm cho nó rõ ràng hơn bằng cách mật mật khẩu thật dài để trang web mất nhiều thời gian đáng kể hơn để xử lý.

## Flawed brute-force protection
> Cơ chế bảo vệ tấn công brute-force tồn tại lỗ hổng.

It is highly that a brute-force attack will involve many failded guesses before the attacker sucessfully compromises an account. Logically, brute-force protection revolves around trying to make it as tricky as possible to automate the process and slow down the rate at which an attacker can attemp logins. The two most commom ways of preventing brute-force attacks are: 

- Locking the account that the remote user is trying to access if they make too many failed login attempts. 

- Blocking the remote user's IP address if they make too many login attempts in quick succession. 

Rất có thể một cuộc tấn công brute-force sẽ bao gồm nhiều lần đoán thất bại trước khi kẻ tấn công xâm nhập thành công vào tài khoản. Về mặt logic, việc bảo vệ khỏi brute-force xoay quanh việc cố gắng làm cho việc tự động hóa quy trình này trở nên khó khăn nhất có thể và làm chậm tốc độ kẻ tấn công có thể đăng nhập. Hai cách phổ biến để ngăn chặn các cuộc tấn công brute-force là:   

- Khóa tài khoản mà người dùng đang cố truy cập nếu họ thực hiện quá nhiều lần đăng nhập thất bại.   
- Chặn địa chỉ IP của người dùng từ xa nếu họ thực hiện quá nhiều lần đăng nhập trong thời gian ngắn.       

Both approaches offer varying degrees of protection, but neither is invulnerable, especially if implemented using flawed logic. 
> Cả hai cách đều cung cấp mức độ bảo vệ khác nhau, nhưng không cách nào là bất khả xâm phạm, đặc biệt nếu được triển khai bằng logic bị lỗi.     

For example, you might sometimes find that your IP is blocked if you fail to log in too many times. In some implementations, the counter for the number of failed attempts resets if the IP owner logs in successfully. This means an attacker would simply have to log in to their own account every few attempts to prevent this limit from ever being reached. 
> Ví dụ đôi khi bạn thấy IP của bạn bị chặn nếu bạn đăng nhập không thành công nhiều lần. Tuy nhiên ở một số cách triển khai, bộ đếm số lần thử sẽ được reset nếu người sỡ hữu IP đăng nhập thành công. Điều này có nghĩa là kẻ tấn công chỉ cần đăng nhập vào tài khoản của chính họ sau vài lần thử để ngăn chặn việc đạt đến giới hạn này.

In this case, merely including you own login credentials at regular intervals throughtout the wordlist is enough to render this defense virtually useless.
> Trong trường hợp này chúng ta chỉ cần thêm thông tin đăng nhập hợp lệ xen kẽ với wordlist là sẽ vượt qua được cơ chế bảo vệ. Làm cho phương pháp này trở nên gần như vô dụng.

## Account locking
> Khóa tài khoản

One way in which websites try to prevent brute-forcing is to lock the account if certain suspicious criteria are met, usually a set number of failed login attempts. Just as with normal login errors, reponses from the server indicating that an account is locked can also help an ttacker to enumerate usernames.
> Một cách mà trang web cố gắng ngăn chặn tấn công brute-force là khóa tài khoản nếu một số tiêu chí đáng ngờ nhất định, thường là số lần đăng nhập thất bại. Cũng giống như các lỗi đăng nhập thông thường, phản hồi từ máy chủ cho biết tài khoản đã bị khóa cũng có thể giúp kẻ tấn công liệt kê tên người dùng.

For example, the following method can be used to work around this kind of protection:
> Ví dụ: phương pháp sau đây có thể sử dụng để giải quyết loại bảo vệ này 

1. Establish a list of candidate usernames that likely to be valid. This could be through username enumeration or simply based on a list of commom usernames.
> Thiết lập danh sách tên người dùng ứng viên có khả năng hợp lệ. Điều này có thể thông qua liệt kê username hoặc đơn giản là dựa vào danh sách username phổ biến.

2. Decide a very small shortlist of passwords that you think at least one user is likely to have. Crucially, the number of passwords you select must not exceed the number of login attempts allowed. For example, if you have worked out that limit is 3 attempts, you need to pick a maxium of 3 password guesses.
> Quyết định một danh sách mật khẩu rút gọn rất nhỏ mà bạn nghĩ rằng ít nhất một người dùng có thể có. Điều quan trọng là số lượng mật khẩu bạn chọn không được vượt quá số lần đăng nhập được cho phép. Ví dụ: bạn tính ra giới hạn đó là 3 lần thử, bạn cần chọn tối đa 3 mật khẩu để đoán.

3. Using a tool such as Burp Intruder, try each of the selected passwords with each of the candidate usernames. This way, you can attempt to brute-force every account without triggering the account lock. You only need a single user to use on of the three passwords in order to compromise an account.
> Sử dụng một công cụ như Burp Intruder để thử từng mật khẩu đã chọn với từng tên người dùng trong danh sách nghi vấn. Bằng cách này, bạn có thể thực hiện tấn công brute-force trên tất cả các tài khoản mà không kích hoạt cơ chế khóa tài khoản. Chỉ cần có một người dùng sử dụng một trong ba mật khẩu đó là bạn đã có thể xâm nhập thành công vào một tài khoản.

Account locking also fails to protect against credential stuffing attacks. This involves using a massive dictionary of username:password pairs, composed of genuine login credentials stolen in data breaches. Credendial stuffing relies on the fact that many people reuse the same username and password on multiple websites and, therefore, there is a chance that some of the compromised credentials in the dictionary are also valid on the target website. Account locking does not protect against credential stuffing because each username is only being attempted once. Credential stuffing is particularly dangerous because it can sometimes result in the attacker compromising many different accounts with just a single automated attack.
> Khóa tài khoản cũng không thể bảo vệ khỏi các cuộc tấn công sử dụng thông tin xác thực bị rò rỉ. Điều này liên quan đến việc sử dụng một từ điển khổng lồ gồm các cặp tên người dùng:mật khẩu, bao gồm thông tin đăng nhập chính xác đáng cắp từ các cuộc lộ lọt dữ liệu. Việc sử dụng thông tin đăng nhập bị rò rỉ dựa trên sự thật rằng nhiều người dùng sử dụng cùng một tên đăng nhập và mật khẩu trên nhiều trang web và do đó, có khả năng một số thông tin xác thực cũng hợp lệ trên trang web mục tiêu. Khóa tài khoản cũng không thể bảo vệ lại khỏi cuộc tấn công trong việc sử dụng thông tin xác thực bị dò rỉ vì mỗi tên người dùng và mật khẩu chỉ dùng một lần. Việc sử dụng thông tin xác thực đặc biệt nguy hiểm vì đôi khi kẻ tấn công có thể xâm phạm nhiều tài khoản khác nhau chỉ bằng một cuộc tấn công tự động.

## User rate limiting
> Giới hạn tỷ lệ người dùng

Another way websites try to prevent brute-force attacks is through user rate limiting. In this case, making too many login requests within a short period of time causes your IP address to be blocked. Typically, the IP can only be unblocked in one of the following ways:
> Một cách khác mà trang web cố gắng ngăn chặn tấn công brute-force là thông qua giới hạn người dùng. Trong trường hợp này, việc gửi quá nhiều yêu cầu đăng nhập trong một thời gian ngắn sẽ gây ra việc địa chỉ IP của bạn bị chặn. Thông thường, IP chỉ có thể được bỏ chặn bằng một trong các cách sau:
- Automatically after a certain period of time has elapsed.
> Tự động sau một khoảng thời gian đã trôi qua.
- Manually by an administrator 
> Quản trị viên gỡ bỏ.
- Manually by the user after successfully completing a CAPTCHA
> Người dùng sau khi xác minh xong captcha.

User rate limiting is sometimes preferred to account locking due to being less prone to username enumeration and denial of service attacks. However, it is still not completely secure. As we saw an example of in earlier lab, there are several ways an attacker can manupulate their apparent IP in order to bypass the block. 
> Cơ chế giới hạn tốc độ theo người dùng đôi khi ưu tiên hơn cơ chế khóa tài khoản vì nó ít tạo cơ hội cho các cuộc tấn công dò tìm username và tấn công từ chối dịch vụ. Tuy nhiên cơ chế này vẫn không hoàn toàn an toàn. Như đã thấy trong lab trước, kẻ tấn công có thể sử dụng nhiều cách để thay đổi hoặc giả mạo địa chỉ IP mà máy chủ nhìn thấy, từ đó vượt qua cơ chế giới hạn này.

As the limit based on the rate of HTTP requests sent from the user's IP address, it is sometimes also possible to bypass this defense if you can work out how to guess multiple passwords with a single request.
> Vì giới hạn dựa trên tốc độ yêu cầu HTTP đươjc gửi đi từ địa chỉ IP của người dùng nên đôi khi bạn cũng thể vượt qua biện pháp này nếu bạn có thể tìm ra cách đoán nhiều mật khẩu bằng một yêu cầu.

## HTTP basic authentication
> Xác thực HTTP cơ bản

Although fairly old, its relative simplicity and ease of implementation means you might sometimes see HTTP basic authentication being used. In HTTP basic authentication, the client receives an authentication token from the server, which is constructed by concatenating the username and password, and encoding it in Base64. This token is stored and managed by the browser, which automatically adds it to the Authorization header of every subsesequent request as follows.
> Mặc dù khá cũ, sự đơn giản trong việc dễ triển khai chúng ta vẫn thấy HTTP basic authentication đôi khi được sử dụng. Trong HTTP basic authentication, client nhận được token xác thực từ server, được tạo bằng cách nối username và password sau đó mã hóa nó bằng Base64. Token này được lưu trữ và quản lý bởi trình duyệt, tự động thêm nó vào header Authorization của mỗi request tiếp theo như sau.

Authorization: Basic base64(username:password)

For a number of reasons, this is generally not considered a secure authentication method. 
Firstly, it involves sending the user's login credentials with every request. Unless the websites also implement HSTS, user credentials are open to being capture in a man-in-the middle attack.
> Vì nhiều lý do, đây thường không được coi là một phương pháp xác thực an toàn. Thứ nhất nó liên quan dến việc liên tục gửi thông tin đăng nhập của ngời dùng với mỗi yêu cầu. Trừ khi trang web triển khai HSTS, thông tin đăng nhập người dùng có thể bị thu giữ trong một cuộc tấn công man-in-the-middle. 

In addition, implementations of HTTP basic authentication often don't support brute-force protection. As the token consists exclusively of statitc values, this can leave it vulnerable to being brute-force. 
> Ngoài ra triển khai việc xác thực HTTP cơ bản thưuòng không hỗ trợ bảo vệ brute-force vì token chỉ bao gồm các giá trị tĩnh, điều này khiến dễ bị brute-force.

HTTP basic authentication is also particularly vulnerable to session-related exploits, notably CSRF, against which it offers no protection on its own.
> Xác thực cơ bản HTTP cũng đặc biệt dễ bị tổn thương trước các lỗ hổng liên quan đến phiên, đặc biệt là CSRF, mà nó không tự bảo vệ được.

In some cases, exploiting vulnerable HTTP basic authentication might only grant an attacker access to a seemingly uninteresting page. However, in addition to providing a further attack surface, the credentials exposed in this way might be reused in other, more confidential contexts.
> Trong một số trường hợp, khai thác xác thực cơ bản HTTP dễ bị tổn thương chỉ giúp kẻ tấn công truy cập vào một trang tưởng chừng không thú vị. Tuy nhiên, ngoài việc tạo tiền đề tấn công, thông tin đăng nhập bị lộ theo cách này có thể được tái sử dụng trong các bối cảnh bảo mật khác.



