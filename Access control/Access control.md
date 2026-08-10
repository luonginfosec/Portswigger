# Access control vulnerabilities and privilege escalation
> Lỗ hổng trong kiểm soát và leo thang đặc quyền

In this section, we describe:
> Trong phần này, chúng ta mô tả:
- Privilege escalation.
> Leo thang đặc quyền 
- The types of vulnerabilities that can arise with access control.
> Các loại lỗ hổng có thể phát sinh với kiểm soát truy cập 
- How to prevent access control vulnerabilities.
> Làm thế nào để ngăn chặn các lỗ hổng kiểm soát truy cập

# What is access control? 
> Kiểm soát truy cập là gì 


Access control is the application of constraints on who or what is authorized to perform actions or access resources. In the context of web applications, access control is dependent on authentication and session management:
> Kiểm soát truy cập là việc áp dụng các ràng buộc về ai hoặc cái gì được ủy quyền để thực hiện các hành động và truy cập tài nguyên. Trong web, kiểm soát truy cập phụ thuộc vào xác thực và quản lý phiên: 

- Authentication confirms that the user is who they say they are.
> Xác thực xác nhận rằng người dùng chính là người dùng mà họ nói. 
- Session management identifies which subsequent HTTP requests are being made by that same user.
> Quản lý phiên xác định những yêu cầu HTTP tiếp theo đang thực hiện bởi người dùng.
- Access control determines whether the user is allowed to carry out the action that they are attempting to perform.
> Kiểm soát truy cập xác định xem người dùng có được phép thực hiện hành động mà họ đang cố gắng thực hiện hay không. 

Broken access controls are common and often present a critical security vulnerability. Design and management of access controls is a complex and dynamic problem that applies business, organizational, and legal constraints to a technical implementation. Access control design decisions have to be made by humans so the potential for errors is high.
> Lỗi kiểm soát truy cập là phổ biến và thường gây ra lỗ hổng nghiêm trọng. Thiết kết và quản lý các biện pháp truy cập là một vấn đề phức tạp và luôn biến động, đòi hỏi phải áp dụng các ràng buộc về kinh doanh, tổ chức và pháp lý vào một hệ thống triển khai kỹ thuật. Các quyết định thiết kế kiểm soát truy cập phải do con người đưa ra, vì vậy khả năng mắc lỗi là cao.   

[Access control security models](<Access control security models.md>)

# Vertical access controls
> Kiểm soát truy cập theo chiều dọc 

Vertical access controls are mechanisms that restrict access to sensitive functionality to specific types of users.
> Kiểm soát truy cập theo chiều dọc là cơ chế gới hạn quyền truy cập theo chức năng nhạy cảm, chỉ dành riêng cho một số loại người dùng nhất định.

With vertical access controls, different types of users have access to different application functions. For example, an administrator might be able to modify or delete any user's account, while an ordinary user has no access to these actions. Vertical access controls can be more fine-grained implementations of security models designed to enforce business policies such as separation of duties and least privilege.
> Theo đó, các loại người dùng khác nhau sẽ có quyền truy cập vào những chức năng khác nhau của ứng dụng. Ví dụ quản trị viên có thể sửa đổi hoặc xóa tài khoản của bất kỳ người dùng nào, trong khi người dùng bình thường không được thực hiện thao tác này. Kiểm soát truy cập theo chiều dọc có thể được triển khai ở mức độ chi tiết cao hơn, dựa trên các mô hình bảo mật được thiết kế nhằm thực thi các chính sách nghiệp vụ như phân chia trách nhiệm và nguyên tắc đặc quyền tối thiểu.


# Horizontal access controls
> Kiểm soát truy cập theo chiều ngang 

Horizontal access controls are mechanisms that restrict access to resources to specific users.
> Kiểm soát truy cập theo chiều ngang là các cơ chế hạn chế quyền truy cập vào tài nguyên với những người dùng cụ thể.

With horizontal access controls, different users have access to a subset of resources of the same type. For example, a banking application will allow a user to view transactions and make payments from their own accounts, but not the accounts of any other user.
> Với kiểm soát truy cập theo chiều ngang, những người dùng khác nhau truy cập vào một tập hợp con các tài nguyên cùng loại. Ví dụ, ứng dụng ngân hàng sẽ cho phép người dùng xem giao dịch và thực hiện thanh toán từ tài khoản của mình, nhưng không được truy cập vào tài khoản của bất kỳ người dùng nào khác.

# Context-dependent access controls
> Kiểm soát truy cập phụ thuộc vào ngữ cảnh

Context-dependent access controls restrict access to functionality and resources based upon the state of the application or the user's interaction with it.
> Kiểm soát truy cập phụ thuộc vào ngữ cảnh hạn chế quyền truy cập vào chức năng và tài nguyên dựa trên trạng thái của ứng dụng hoặc tương tác của người dùng với nó.

Context-dependent access controls prevent a user performing actions in the wrong order. For example, a retail website might prevent users from modifying the contents of their shopping cart after they have made payment.
> Kiểm soát truy cập phụ thuộc vào ngữ cảnh ngăn cản người dùng thực hiện hành động theo trình tự sai. Ví dụ, một trang web bán lẻ có thể ngăn người dùng sửa đổi nội dung giỏ hàng của họ sau khi đã thanh toán.

# Examples of broken access controls
> Ví dụ về lỗ hổng kiểm soát truy cập

Broken access control vulnerabilities exist when a user can access resources or perform actions that they are not supposed to be able to.
> Lổ hổng kiểm soát truy cập phát sinh khi người dùng có thể truy cập vào tài nguyên hoặc thực hiện các hành động mà họ không được phép làm.

# Vertical privilege escalation
> Leo thang đặc quyền theo chiều dọc

If a user can gain access to functionality that they are not permitted to access then this is vertical privilege escalation. For example, if a non-administrative user can gain access to an admin page where they can delete user accounts, then this is vertical privilege escalation.
> Nếu người dùng có thể truy cập vào các chức năng mà họ không có quyền sử dụng, thì đó được gọi là leo thang đặc quyền theo chiều dọc.

# Unprotected functionality
> Chức năng không được bảo vệ 

At its most basic, vertical privilege escalation arises where an application does not enforce any protection for sensitive functionality. For example, administrative functions might be linked from an administrator's welcome page but not from a user's welcome page. However, a user might be able to access the administrative functions by browsing to the relevant admin URL.
> Ở mức cơ bản nhất, lỗ hổng leo thang đặc quyền theo chiều dọc xảy ra khi ứng dụng không áp dụng bất kì biện pháp bảo vệ nào với chức năng nhạy cảm. Ví dụ, các chức năng quản trị có thể được hiện thị dưới dạng liên kết trên trang chào mừng của quản trị viên nhưng không có liên kết trên trang chào mừng của người dùng. Tuy nhiên, người dùng vẫn có thể truy cập vào chức năng quản trị bằng cách truy cập URL tương ứng. 

For example, a website might host sensitive functionality at the following URL:
Ví dụ: một trang web có thể lưu trữ chức năng nhạy cảm tại URL sau:

```https://insecure-website.com/admin```

This might be accessible by any user, not only administrative users who have a link to the functionality in their user interface. In some cases, the administrative URL might be disclosed in other locations, such as the robots.txt file:
> URL có thể truy cập bởi bất cứ người dùng nào, không chỉ người dùng quản trị mà những người dùng thông thông có link vẫn có thể truy cập vào chức năng nhạy cảm. Thậm chí, URL quản trị có thể bị lộ ở các nơi khác, ví dụ như file robots.txt.       

```https://insecure-website.com/robots.txt```

Even if the URL isn't disclosed anywhere, an attacker may be able to use a wordlist to brute-force the location of the sensitive functionality.
> Ngay cả khi URL không bị lộ ở bất kỳ đâu, kẻ tấn công vẫn có thể sử dụng wordlist để dò tìm vị trí của các chức năng nhạy cảm đó.

In some cases, sensitive functionality is concealed by giving it a less predictable URL. This is an example of so-called "security by obscurity". However, hiding sensitive functionality does not provide effective access control because users might discover the obfuscated URL in a number of ways.
> Trong một vài trường hợp, chức năng nhạy cảm được ẩn đi bằng cách đặt một URL khó đoán. Đây được gọi là kỹ thuật "bảo mật che dấu". Tuy nhiên, việc ẩn chức năng nhạy cảm như vậy không cung cấp quyền kiểm soát truy cập hiệu quả vì người dùng có thể phát hiện ra URL ẩn bằng nhiều cách. 


Imagine an application that hosts administrative functions at the following URL:
> Hãy tưởng tượng một ứng dụng lưu trữ các chức năng quản trị tại URL sau:

```https://insecure-website.com/administrator-panel-yb556```

This might not be directly guessable by an attacker. However, the application might still leak the URL to users. The URL might be disclosed in JavaScript that constructs the user interface based on the user's role:
> Kẻ tấn công có thể không đoán trực tiếp ra URL này. Tuy nhiên ứng dụng có thể bị dò rỉ URL này cho người dùng. URL có thể bị lộ ngay trong mã JavaScript dùng để xây dựng giao diện người dùng dựa trên vai trò của họ  

```JavaScript
<script>
	var isAdmin = false;
	if (isAdmin) {
		...
		var adminPanelTag = document.createElement('a');
		adminPanelTag.setAttribute('href', 'https://insecure-website.com/administrator-panel-yb556');
		adminPanelTag.innerText = 'Admin panel';
		...
	}
</script>
```

This script adds a link to the user's UI if they are an admin user. However, the script containing the URL is visible to all users regardless of their role.
> Tập lệnh này thêm liên kết vào giao diện người dùng nếu là người dùng quản trị. Tuy nhiên, tập lệnh chứa URL có thể hiển thị cho tất cả người dùng bất kể vai trò của họ.

# Parameter-based access control methods
> Phương pháp kiểm soát truy cập dựa trên tham số.

Some applications determine the user's access rights or role at login, and then store this information in a user-controllable location. This could be:
> Một số ứng dụng xác định quyền truy cập hoặc vai trò của người dùng khi đăng nhập, sau đó lưu trữ thông tin này tại một vị trí mà người dùng có thể kiểm soát. Vị trí này có thể này:

- A hidden field.
> Một trường ẩn .

- A cookie.
> Một cookie.

- A preset query string parameter.
> Một tham số chuỗi truy vấn đặt trước.

The application makes access control decisions based on the submitted value. For example:
> Ứng dụng đưa ra quyết định kiểm soát truy cập dựa trên giá trị được gửi. Ví dụ:

```
https://insecure-website.com/login/home.jsp?admin=true
https://insecure-website.com/login/home.jsp?role=1
```
This approach is insecure because a user can modify the value and access functionality they're not authorized to, such as administrative functions.
> Cách tiépe cận này tầm ẩn rủi do vì người dùng có thể sửa đổi các giá trị này để truy cập vào những chức năng mà họ không được phép, chẳng hạn như chức năng quản trị.

# Broken access control resulting from platform misconfiguration
> Lổ hổng kiểm soát truy cập do cấu hình nền tảng sai 

Some applications enforce access controls at the platform layer. they do this by restricting access to specific URLs and HTTP methods based on the user's role. For example, an application might configure a rule as follows:
> Một số ứng dụng áp dụng kiểm soát truy cập ở mức nền tảng. Chúng thực hiện việc này bằng cách hạn chế quyền truy cập vào các URL, phương thức HTTP cụ thể dựa trên vai trò của người dùng. Ví dụ: một ứng dụng có thể cấu hình một quy tắc như sau:


```DENY: POST, /admin/deleteUser, managers```

This rule denies access to the POST method on the URL /admin/deleteUser, for users in the managers group. Various things can go wrong in this situation, leading to access control bypasses.
> Quy tắc này từ chối quyền truy cập đối với phương thức POST trên URL ```/admin/deleteUser``` dành cho người dùng người dùng thuộc nhóm quản lý. Nhiều vấn đề có thể phát sinh trong tình huống này, dẫn đến các lổ hổng cho phép vượt qua kiểm soát ruy cập 

Some application frameworks support various non-standard HTTP headers that can be used to override the URL in the original request, such as X-Original-URL and X-Rewrite-URL. If a website uses rigorous front-end controls to restrict access based on the URL, but the application allows the URL to be overridden via a request header, then it might be possible to bypass the access controls using a request like the following:
> Một số framework ứng dụng hỗ trợ nhiều header HTTP không tiêu chuẩn khác nhau, có thể sử dụng để ghi đề URL trong yêu cầu ban đầu. chẳng hạn như ```X-Original-URL``` và ```X-Rewrite-URL```. Nếu một trang web sử dụng các biện pháp hạn chế quyền truy cập chặt chẽ dựa trên URL, nhưng ứng dụng cho phép ghi đè URL qua request header, thì có thể vượt qua kiểm soát truy cập bằng cách sử dụng request như sau:  

```
POST / HTTP/1.1
X-Original-URL: /admin/deleteUser
```


An alternative attack relates to the HTTP method used in the request. The front-end controls described in the previous sections restrict access based on the URL and HTTP method. Some websites tolerate different HTTP request methods when performing an action. If an attacker can use the GET (or another) method to perform actions on a restricted URL, they can bypass the access control that is implemented at the platform layer.

> Một phương pháp tấn công khác liên quan đến HTTP method được sử dụng trong request. Các biện pháp kiểm soát ở phía front-end đã được mô tả trong phần trước hạn chế truy cập dựa trên URL và HTTP method. Một số website chấp nhận một số HTTP method khác nhau khi thực hiện một hành động. Nếu có tấn công có thể sử dụng phương thức GET (hoặc cái khác) để thực hiện tấn công trên URL bị hạn chế, chúng có thể vượt qua kiểm soát truy cập được triển khai ở tầng nền tảng.

# Broken access control resulting from URL-matching discrepancies
> Lỗ hổng kiểm soát truy cập phát sinh do sự sai lệch trong các so khớp URL.

Websites can vary in how strictly they match the path of an incoming request to a defined endpoint. For example, they may tolerate inconsistent capitalization, so a request to /ADMIN/DELETEUSER may still be mapped to the /admin/deleteUser endpoint. If the access control mechanism is less tolerant, it may treat these as two different endpoints and fail to enforce the correct restrictions as a result.
> Các trang web có mức độ linh hoạt khác nhau khi so sánh path của request tới một endpoint được định nghĩa. Ví dụ chúng có thể chấp nhận sự không nhất quán về chữ hoa/chữ thường, do đó yêu cầu /ADMIN/DELETEUSER vẫn có thể được ánh xạ đến endpoint /admin/deleteUser. Nếu cơ chế kiểm soát truy cập kém linh hoạt hơn, nó có thể coi đây là hai endpoint khác nhau và dẫn đến việc thất bại trong việc áp đặt các biện pháp kiểm soát truy cập phù hợp.	

Similar discrepancies can arise if developers using the Spring framework have enabled the useSuffixPatternMatch option. This allows paths with an arbitrary file extension to be mapped to an equivalent endpoint with no file extension. In other words, a request to /admin/deleteUser.anything would still match the /admin/deleteUser pattern. Prior to Spring 5.3, this option is enabled by default.
> Sự khác biệt tương tự cũng có thể xảy ra nếu nhà phát triển sử dụng framework Spring và bật tùy chọn useSuffixPatternMatch. Tính năng này cho phép các đường dẫn chứa phần mở rộng bất kỳ vẫn được ánh xạ tới endpoint tương ứng mà không cần phần mở rộng. Nói cách khác, request tới `/admin/deleteUser.anything` vẫn khớp với `/admin/deleteUser`. Cần lưu ý rằng tại Spring 5.3, tùy chọn này là mặc định.

On other systems, you may encounter discrepancies in whether /admin/deleteUser and /admin/deleteUser/ are treated as distinct endpoints. In this case, you may be able to bypass access controls by appending a trailing slash to the path.
> Trên các hệ thống khác, bạn có thể gặp trường hợp `/admin/deleteUser` và `/admin/deleteUser/` (có gạch chéo cuối) được coi là endpoint riêng biệt. Trong tình huống này kẻ tấn công có thể vượt qua kiểm soát truy cập bằng cách thêm gạch chéo vào cuối path.	

# Horizontal privilege escalation
> Leo thang đặc quyền theo chiều ngang

Horizontal privilege escalation occurs if a user is able to gain access to resources belonging to another user, instead of their own resources of that type. For example, if an employee can access the records of other employees as well as their own, then this is horizontal privilege escalation.
> Leo thang đặc quyền theo chiều ngang xảy ra khi người dùng có thể truy cập tài nguyên của người dùng khác thay vì tài nguyên của chính mình. Ví dụ, nếu một nhân viên có thể truy cập hồ sơ của nhân viên khác cũng như của mình thì đây là leo thang đặc quyền theo chiều ngang.
  
`https://insecure-website.com/myaccount?id=123`

If an attacker modifies the id parameter value to that of another user, they might gain access to another user's account page, and the associated data and functions.
> Nếu kẻ tấn công có thể sửa tham số id thành một giá trị của người dùng khác, chúng có thể truy cập được vào tài khoản của người dùng đó, cùng với dữ liệu liên quan.

Note: 
This is an example of an insecure direct object reference (IDOR) vulnerability. This type of vulnerability arises where user-controller parameter values are used to access resources or functions directly.
> Lưu ý: Đây là một lỗ hổng về tham chiếu đối tượng không an toàn, loại lỗ hổng này phát sinh khi các giá trị tham số do người dùng điều khiển được sử dụng để truy cập trực tiếp tài nguyên hoặc chức năng.	

In some applications, the exploitable parameter does not have a predictable value. For example, instead of an incrementing number, an application might use globally unique identifiers (GUIDs) to identify users. This may prevent an attacker from guessing or predicting another user's identifier. However, the GUIDs belonging to other users might be disclosed elsewhere in the application where users are referenced, such as user messages or reviews.
> Trong một số ứng dụng, tham số có thể bị khai thác không có giá trị có thể dự đoán trước. Ví dụ, thay vì sử dụng số tăng dần, ứng dụng có thể dùng mã định danh duy nhất toàn cầu (GUIDs) để xác định người dùng. Tuy nghiên, GUIDs của những người dùng khác nhau có thể bị lộ ở một số nơi khác trong ứng dụng, nơi người dùng được tham chiếu, chẳng hạn như tin nhắn của người dùng hoặc bài đánh giá.	

# Insecure direct object references
> Tham chiếu đối tượng trực tiếp không an toàn (IDOR)

Insecure direct object references (IDORs) are a subcategory of access control vulnerabilities. IDORs occur if an application uses user-supplied input to access objects directly and an attacker can modify the input to obtain unauthorized access. It was popularized by its appearance in the OWASP 2007 Top Ten. It's just one example of many implementation mistakes that can provide a means to bypass access controls.
> Lỗ hổng tham chiếu đối tượng trực tiếp không an toàn là một dạng con của nhóm lỗ hổng kiểm soát truy cập. IDOR xảy ra khi ứng dụng sử dụng đầu vào do con người điều khiển để truy cập trực tiếp vào các đối tượng và kẻ tấn công có thể sửa đổi đầu vào để truy cập trái phép. Lỗ hổng này trở nên phổ biến rộng rãi nhờ được đưa vào OWASP 2007 Top 10. Đây chỉ là một trong số rất nhiều ví dụ về các lỗi triển khai có thể tạo cơ hội để vượt qua kiểm soát truy cập.

[Insecure direct object references (IDOR)](<Insecure direct object references (IDOR).md>)

# Access control vulnerabilities in multi-step processes

> Lỗ hổng kiểm soát truy cập trong các quy trình nhiều bước

Many websites implement important functions over a series of steps. This is common when:
> Nhiều trang web thực hiện các chức năng quan trọng theo chuỗi các bước. Điều này thường xảy ra khi:

- A variety of inputs or options need to be captured.
- The user needs to review and confirm details before the action is performed.

For example, the administrative function to update user details might involve the following steps:
> Ví dụ, chức năng quản trị người dùng có thể bao gồm các bước sau
1. Load the form that contains details for a specific user.
> Tải biểu mẫu chứa thông tin chi tiết của một người dùng cụ thể
2. Submit the changes.
> Gửi các thay đổi
3. Review the changes and confirm.
> Xem lại và xác nhận 

Sometimes, a website will implement rigorous access controls over some of these steps, but ignore others. Imagine a website where access controls are correctly applied to the first and second steps, but not to the third step. The website assumes that a user will only reach step 3 if they have already completed the first steps, which are properly controlled. An attacker can gain unauthorized access to the function by skipping the first two steps and directly submitting the request for the third step with the required parameters.
> Đôi khi website sẽ áp dụng kiểm soát truy cập một cách chặt chẽ với một số bước trong quy trình này. Hãy tưởng tượng rằng một trang web nơi kiểm soát nơi kiểm soát truy cập được áp dụng đúng cách cho các bước thứ nhất và bước thứ hai, nhưng không được áp dụng cho bước thứ 3. Trang web giả định rằng người dùng sẽ chỉ đến được bước 3 nếu học hoàn thành các bước đầu tiên (vốn đã được kiểm soát đúng mức). Một kẻ tấn công giành quyền truy cập trái phép vào chức năng đó bằng cách bỏ qua 2 bước đầu tiên và gửi yêu cầu trực tiếp cho bước thứ 3 cùng các tham số.

# Referer-based access control
> Kiểm soát truy cập dựa trên Referer

Some websites base access controls on the Referer header submitted in the HTTP request. The Referer header can be added to requests by browsers to indicate which page initiated a request.
> Một số website dựa trên kiểm soát truy cập vào tiêu đề Referer được gửi trong yêu cầu HTTP. Tiêu đề Referer có thể được trình duyệt thêm vào một request để chỉ ra trang nào đã yêu cầu tài nguyên.

For example, an application robustly enforces access control over the main administrative page at /admin, but for sub-pages such as /admin/deleteUser only inspects the Referer header. If the Referer header contains the main /admin URL, then the request is allowed.
> Ví dụ, ứng dụng kiểm soát truy cập chặt chẽ tại trang quản trị chính /admin, nhưng đối với các trang con như /admin/deleteUser nó chỉ kiểm tra Referer header. Nếu Referer header chứa URL /admin thì yêu cầu được chấp nhận. 

In this case, the Referer header can be fully controlled by an attacker. This means that they can forge direct requests to sensitive sub-pages by supplying the required Referer header, and gain unauthorized access.
> Trong trường hợp này, Referer header có thể bị kẻ tấn công kiểm soát hoàn toàn. Điều này có ý nghĩa là chúng có thể giả mạo các yêu cầu trực tiếp đến trang con nhạy cảm bằng cách thêm Referer header cần thiết và dành quyền truy cập trái phép.

# Location-based access control
> Kiểm soát truy cập dựa trên vị trí địa lý

Some websites enforce access controls based on the user's geographical location. This can apply, for example, to banking applications or media services where state legislation or business restrictions apply. These access controls can often be circumvented by the use of web proxies, VPNs, or manipulation of client-side geolocation mechanisms.
> Một số trang web áp dụng kiểm soát truy cập dựa trên vị trí địa lý của người dùng. Điều này có thể áp dụng, ví dụ, cho các ứng dụng ngân hàng hoặc dịch vụ truyền thông, nơi mà luật pháp của từng bang hoặc các hạn chế kinh doanh được áp dụng. Các biện pháp kiểm soát truy cập này thường có thể bị vượt qua bằng cách sử dụng web proxy, VPN hoặc thao túng các cơ chế định vị địa lý phía máy khách.

