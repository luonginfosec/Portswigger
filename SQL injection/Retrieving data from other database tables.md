# Retrieving data from other database tables
> Lấy dữ liệu từ các bảng khác trong database.  

In cases where the application responds with the results of a SQL query, an attacker can use a SQL injection vulnerability to retrieve data from other tables within the database. You can use the UNION keyword to execute an additional SELECT query and append the results to the original query.
> Trong trường hợp ứng dụng phản hồi kết quả câu truy vấn, kẻ tấn công có thể sử dụng SQL injection để lấy dữ liệu từ các bảng khác trong database. Kẻ tấn công có thể sử dụng UNION keyword để thực hiện thêm một câu lệnh SELECT và nối kết quả vào câu truy vấn ban đầu.
    
For example, if an application executes the following query containing the user input Gifts:

```SELECT name, description FROM products WHERE category = 'Gifts'```

> Ví dụ, nếu ứng dụng thực thi truy vấn sau chứa input Gifts của người dùng:

An attacker can submit the input:

```' UNION SELECT username, password FROM users--```

> Kẻ tấn công có thể gửi input:

This causes the application to return all usernames and passwords along with the names and descriptions of products.
> Điều này làm cho ứng dụng trả về tất cả tên người dùng và mật khẩu cùng với tên và mô tả sản phẩm.  

[SQL injection UNION attacks](<SQL injection UNION attacks.md>)