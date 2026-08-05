# Subverting application logic 
> Làm sai lệch logic ứng dụng 

Imagine an application that lets users log in with a username and password. If a user submits the username wiener and the password bluecheese, the application checks the credentials by performing the following SQL query:

```SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'```

If the query returns the details of a user, then the login is successful. Otherwise, it is rejected.

In this case, an attacker can log in as any user without the need for a password. They can do this using the SQL comment sequence -- to remove the password check from the WHERE clause of the query. For example, submitting the username administrator'-- and a blank password results in the following query:

```SELECT * FROM users WHERE username = 'administrator'--' AND password = ''```

This query returns the user whose username is administrator and successfully logs the attacker in as that user.

> Hãy tưởng tượng một ứng dụng cho phép người dùng đăng nhập bằng tên đăng nhập và mật khẩu. 

> Nếu người dùng gửi tên đăng nhập 'wiener' và mật khẩu 'bluecheese', ứng dụng sẽ kiểm tra thông tin đăng nhập bằng cách thực hiện truy vấn SQL sau:    

```SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'```

> Nếu truy vấn trả về thông tin của một người dùng, thì việc đăng nhập sẽ thành công. Ngược lại, nó sẽ bị từ chối.

> Trong tình huống này, kẻ tấn công có thể đăng nhập với tư cách bất kì người dùng nào mà không cần mật khẩu. Họ có thể làm điều này bằng cách sử dụng chuỗi bình luận SQL -- để loại bỏ kiểm tra mật khẩu khỏi mệnh đề WHERE của truy vấn. Ví dụ, gửi tên đăng nhập administrator'-- và mật khẩu trống sẽ dẫn đến truy vấn sau:    

```SELECT * FROM users WHERE username = 'administrator'--' AND password = ''```

> Truy vấn này trả về người dùng có tên đăng nhập là administrator và đăng nhập thành công với tư cách người dùng đó.   

