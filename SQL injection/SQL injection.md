# What is SQL injection (SQLi)?s
> SQLi là gì ?

SQL injection (SQLi) is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. This can allow an attacker to view data that they are not normally able to retrieve. This might include data that belongs to other users, or any other data that the application can access. In many cases, an attacker can modify or delete this data, causing persistent changes to the application's content or behavior.
> SQLi là một lỗ hổng bảo mật web cho phép can thiệp vào truy vấn mà ứng dụng gửi đến sever. Điều này cho phép kẻ tấn công xem dữ liệu mà họ không thể truy xuất. Điều này bao gồm dữ liệu thuộc về người dùng khác, hoặc bất kì người dùng nào có thể truy cập. Trong nhiều trường hợp, kẻ tấn công có thể sửa đổi hoặc xóa dữ liệu, gây ra các thay đổi liên tục đối với nội dung hoặc hành vi của ứng dụng.

In some situations, an attacker can escalate a SQL injection attack to compromise the underlying server or other back-end infrastructure. It can also enable them to perform denial-of-service attacks.
> Trong một số trường hợp, kẻ tấn công có thẻ leo thang từ cuộc tấn công SQL Injection để xâm phạm máy chủ hoặc các hạ tầng phụ trợ khác. Cũng có thể sử dụng kĩ thuật này để tấn công các cuộc tấn công từ chối dịch vụ/

# What is the impact of a successful SQL injection attack?
> Tác động của một cuộc tấn công SQLi thành công là gì ? 

A successful SQL injection attack can result in unauthorized access to sensitive data, such as:
- Passwords.   
- Credit card details.   
- Personal user information.   

Một cuộc tấn công SQL injection thành công có thể dẫn đến truy cập trái phép vào dữ liệu nhạy cảm, chẳng hạn như:
- Mật khẩu
- Thông tin thẻ tín dụng 
- Thông tin cá nhân người dùng

SQL injection attacks have been used in many high-profile data breaches over the years. These have caused reputational damage and regulatory fines. In some cases, an attacker can obtain a persistent backdoor into an organization's systems, leading to a long-term compromise that can go unnoticed for an extended period.
> Các cuộc tấn công SQLi đã được sử dụng trong nhiều vụ rò rỉ dữ liệu nổi bật qua các năm. Những điều này đã gây tổn hại đến uy tín, cũng như tiền bạc. Trong một số trường hợp kẻ tấn công có thể để lại backdoor lâu dài trong hệ thống của tổ chức, dẫn đến một sự xâm nhập lâu dài có thể không bị phát hiện trong thời gian dài. 

# How to detect SQL injection vulnerabilities
> Cách phát hiện lỗ hổng SQL injection

You can detect SQL injection manually using a systematic set of tests against every entry point in the application. To do this, you would typically submit:
- The single quote character ' and look for errors or other anomalies.
- Some SQL-specific syntax that evaluates to the base (original) value of the entry point, and to a different value, and look for systematic differences in the application responses.
- Boolean conditions such as OR 1=1 and OR 1=2, and look for differences in the application's responses.
- Payloads designed to trigger time delays when executed within a SQL query, and look for differences in the time taken to respond.
- OAST payloads designed to trigger an out-of-band network interaction when executed within a SQL query, and monitor any resulting interactions.

Alternatively, you can find the majority of SQL injection vulnerabilities quickly and reliably using Burp Scanner.

> Bạn có thể phát hiện SQLi theo cách thủ công bằng cách sử dụng một bộ kiểm thử hệ thống với mọi điểm đầu vào trong ứng dụng. Để làm điều này, bạn thường sẽ gửi:
- Kí tự dấu nháy dơn ' và quan sát các lỗi hoặc bất thường khác. 
- Một cú pháp đặc thù của SQL mà khi thực thi sẽ cho ra giá trị bằng với giá trị gốc của điểm đầu vào, và một số cú pháp khác cho ra giá trị khác, sau đó quan sát sự khác biệt có hệ thống trong phản hồi của ứng dụng. 
- Các điều kiện Boolean như OR 1=1 và OR 1=2, sau đó quan sát sự khác biệt trong phản hồi của ứng dụng.

Ngoài ra có thể nhanh chóng tìm hầu hết các lỗ hổng SQLi bằng Burp Scanner .    

# SQL injection in different parts of the query
> SQLi ở các phần khác nhau của câu truy vấn

Most SQL injection vulnerabilities occur within the WHERE clause of a SELECT query. Most experienced testers are familiar with this type of SQL injection.
> Hầu hết các lỗ hổng SQLi xảy ra trong mệnh đề WHERE của câu lệnh SELECT. Những tester có kinh nghiệm đều quen thuộc với loại SQLi này.

However, SQL injection vulnerabilities can occur at any location within the query, and within different query types. Some other common locations where SQL injection arises are:

- In UPDATE statements, within the updated values or the WHERE clause.
- In INSERT statements, within the inserted values.
- In SELECT statements, within the table or column name.
- In SELECT statements, within the ORDER BY clause.

> Tuy nhiên, các lỗ hổng SQLi có thể xuất hiện ở bất kỳ vị trí nào trong truy vấn, và trong các loại truy vấn khác nhau. Một số vị trí phổ biến mà SQLi xuất hiện là: 

- Trong câu lệnh UPDATE, ở phần giá trị được gán hoặc trong mệnh đề WHERE
- Trong câu lệnh INSERT, ở phần giá trị được chèn
- Trong các câu lệnh SELECT, ở tên bảng hoặc cột
- Trong các câu lệnh SELECT, ở mệnh đề ORDER BY 

# SQL injection examples
> Ví dụ về SQL injection

There are lots of SQL injection vulnerabilities, attacks, and techniques, that occur in different situations. Some common SQL injection examples include:
- Retrieving hidden data, where you can modify a SQL query to return additional results.
- Subverting application logic, where you can change a query to interfere with the application's logic.
- UNION attacks, where you can retrieve data from different database tables.
- Blind SQL injection, where the results of a query you control are not returned in the application's responses.

> Có rất nhiều lỗ hổng, kỹ thuật và phương thức tấn công SQLi tồn tại trong nhiều bối cảnh khác nhau. Một số ví dụ SQLi phổ biến bao gồm:
- Khai thác dữ liệu ẩn - trong đó bạn có thể sửa đổi truy vấn SQL để trả thêm các kết quả bổ sung.
- Làm sai lệch logic ứng dụng - trong đó bạn thay đổi truy vấn nhằm can thiệp vào luồng xử lý logic của ứng dụng
- Tấn công UNION - cho phép bạn truy xuất dữ liệu từ các bảng khác nhau trong database
- SQLi mù - trong kết quả của truy vấn SQL sẽ không được trả về trực tiếp trong phản hồi của ứng dụng.

[Retrieving hidden data](<Retrieving hidden data.md>)

[Subverting application logic](<Subverting application logic.md>)

[Retrieving data from other database tables](<Retrieving data from other database tables.md>)

# Blind SQL injection vulnerabilities
> Lỗ hổng SQLi mù 

[Blind SQL injection](<Blind SQL injection.md>)

Many instances of SQL injection are blind vulnerabilities. This means that the application does not return the results of the SQL query or the details of any database errors within its responses. Blind vulnerabilities can still be exploited to access unauthorized data, but the techniques involved are generally more complicated and difficult to perform.
> Nhiều trường hợp SQL injection là các lỗ hổng dạng mù. Điều này có nghĩa là ứng dụng không trả về kết quả của truy vấn SQL hoặc chi tiết về bất kỳ lỗi cơ sở dữ liệu nào trong phản hồi của nó. Các lỗ hổng dạng mù vẫn có thể bị khai thác để truy cập dữ liệu trái phép, nhưng các kỹ thuật liên quan thường phức tạp hơn và khó thực hiện hơn.

The following techniques can be used to exploit blind SQL injection vulnerabilities, depending on the nature of the vulnerability and the database involved:

- You can change the logic of the query to trigger a detectable difference in the application's response depending on the truth of a single condition. This might involve injecting a new condition into some Boolean logic, or conditionally triggering an error such as a divide-by-zero.
- You can conditionally trigger a time delay in the processing of the query. This enables you to infer the truth of the condition based on the time that the application takes to respond.
- You can trigger an out-of-band network interaction, using OAST techniques. This technique is extremely powerful and works in situations where the other techniques do not. Often, you can directly exfiltrate data via the out-of-band channel. For example, you can place the data into a DNS lookup for a domain that you control.

> Các kỹ thuật sau đây có thể được sử dụng để khai thác các lỗ hổng SQL injection mù, tùy thuộc vào bản chất của lỗ hổng và cơ sở dữ liệu liên quan:

> - Bạn có thể thay đổi logic của truy vấn để tạo ra sự khác biệt có thể phát hiện được trong phản hồi của ứng dụng, tùy thuộc vào tính đúng đắn của một điều kiện duy nhất. Việc này có thể bao gồm việc tiêm một điều kiện mới vào logic Boolean nào đó, hoặc kích hoạt có điều kiện một lỗi (ví dụ như phép chia cho số không).
> - Bạn có thể kích hoạt có điều kiện một độ trễ thời gian trong quá trình xử lý truy vấn. Điều này cho phép bạn suy ra tính đúng đắn của điều kiện dựa trên thời gian ứng dụng phản hồi.
> - Bạn có thể kích hoạt một tương tác mạng ngoài băng tần (out-of-band), sử dụng các kỹ thuật OAST. Kỹ thuật này cực kỳ mạnh mẽ và hoạt động trong những tình huống mà các kỹ thuật khác không làm được. Thông thường, bạn có thể trích xuất trực tiếp dữ liệu qua kênh ngoài băng tần. Ví dụ: bạn có thể đưa dữ liệu vào một truy vấn DNS cho một tên miền mà bạn kiểm soát.

# SQL injection in different contexts

In the previous labs, you used the query string to inject your malicious SQL payload. However, you can perform SQL injection attacks using any controllable input that is processed as a SQL query by the application. For example, some websites take input in JSON or XML format and use this to query the database.
> Trong các bài thực hành trước, bạn đã sử dụng chuỗi truy vấn để chèn tải trọng SQL độc hại vào. Tuy nhiên, bạn có thể thực hiện các cuộc tấn công tiêm nhiễm SQL bằng cách sử dụng bất kỳ đầu vào có thể kiểm soát nào được ứng dụng xử lý dưới dạng truy vấn SQL. Ví dụ: một số trang web lấy dữ liệu đầu vào ở định dạng JSON hoặc XML và sử dụng dữ liệu này để truy vấn cơ sở dữ liệu.

These different formats may provide different ways for you to obfuscate attacks that are otherwise blocked due to WAFs and other defense mechanisms. Weak implementations often look for common SQL injection keywords within the request, so you may be able to bypass these filters by encoding or escaping characters in the prohibited keywords. For example, the following XML-based SQL injection uses an XML escape sequence to encode the S character in SELECT:
> Các định dạng khác nhau này có thể cung cấp những cách khác nhau để bạn làm xáo trộn các cuộc tấn công bị chặn do WAF và các cơ chế phòng thủ khác. Việc triển khai yếu thường tìm kiếm các từ khóa chèn SQL phổ biến trong yêu cầu, vì vậy bạn có thể bỏ qua các bộ lọc này bằng cách mã hóa hoặc thoát các ký tự trong các từ khóa bị cấm. Ví dụ: nội dung SQL dựa trên XML sau đây sử dụng chuỗi thoát XML để mã hóa ký tự S trong SELECT:

```
<stockCheck>
    <productId>123</productId>
    <storeId>999 &#x53;ELECT * FROM information_schema.tables</storeId>
</stockCheck>
```

This will be decoded server-side before being passed to the SQL interpreter.
> Điều này sẽ được giải mã phía máy chủ trước khi được chuyển tới trình thông dịch SQL.