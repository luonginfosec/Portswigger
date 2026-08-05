# SQL injection UNION attacks
> Tấn công SQL injection sử dụng UNION

When an application is vulnerable to SQL injection, and the results of the query are returned within the application's responses, you can use the UNION keyword to retrieve data from other tables within the database. This is commonly known as a SQL injection UNION attack.

The UNION keyword enables you to execute one or more additional SELECT queries and append the results to the original query. For example:

```SELECT a, b FROM table1 UNION SELECT c, d FROM table2```

This SQL query returns a single result set with two columns, containing values from columns a and b in table1 and columns c and d in table2.

For a UNION query to work, two key requirements must be met:

- The individual queries must return the same number of columns.
- The data types in each column must be compatible between the individual queries.

To carry out a SQL injection UNION attack, make sure that your attack meets these two requirements. This normally involves finding out:

- How many columns are being returned from the original query.
- Which columns returned from the original query are of a suitable data type to hold the results from the injected query.

> Khi một ứng dụng bị lỗ hổng SQLi và vieiẹc kết quả truy vấn được trả về trong phản hồi của ứng dụng, bạn có thể sử dụng từ khóa UNION để truy xuất dữ liệu từ các bảng khác nhau trong cơ sở dữ liệu. Đây thường gọi là tấn công SQLi dạng UNION.

> Từ khóa UNION cho phép bạn thực thi một hoặc nhiều truy vấn SELECT bổ sung và nối vào kết quả ban đầu ví dụ:

```SELECT a, b FROM table1 UNION SELECT c, d FROM table2```

> Truy vấn SQL này trả về một tập kết quả duy nhất với hai cột, chứa các giá trị từ cột a và b trong bảng table1 cùng với các giá trị từ cột c,d trong bảng table2.

> Để truy vấn UNION hoạt động, cần đáp ứng hai yêu cầu chính: 
- Các truy vấn riêng lẻ phải trả về cùng số lượng cột. 
- Kiểu dữ liệu ở mỗi cột phải tương thích giữa các truy vấn riêng lẻ.

> Để thực hiện tấn công SQLi dạng UNION, bạn cần đảm bảo rằng cuộc tấn công của mình phải đáp ứng hai nhu cầu trên. Điều này thường bao gồm việc tìm ra:
- Truy vấn ban đầu trả về bao nhiêu cột
- Những cột nào trong truy vấn ban đầu có dữ liệu phù hợp để chứa kết quả từ truy vấn được chèn vào.

# Determining the number of columns required
> Xác định số lượng cột cần thiết

When you perform a SQL injection UNION attack, there are two effective methods to determine how many columns are being returned from the original query.
> Khi bạn thực hiện tấn công SQLi dạng UNION, có hai phương pháp hiệu quả để xác định số lượng cột được trả về từ truy vấn ban đầu.

One method involves injecting a series of ORDER BY clauses and incrementing the specified column index until an error occurs. For example, if the injection point is a quoted string within the WHERE clause of the original query, you would submit:
> Một phương pháp liên quan đến việc chèn một chuỗi các mệnh đề ORDER BY và tăng chỉ số cột được chỉ định cho đến khi xảy ra lỗi. Ví dụ, nếu điểm tiêm là một chuỗi ký tự trong mệnh đề WHERE của truy vấn ban đầu, bạn sẽ gửi:

```
' ORDER BY 1--
' ORDER BY 2--
' ORDER BY 3--
etc.
```


This series of payloads modifies the original query to order the results by different columns in the result set. The column in an ORDER BY clause can be specified by its index, so you don't need to know the names of any columns. When the specified column index exceeds the number of actual columns in the result set, the database returns an error, such as:
> Chuỗi payloads này sẽ sửa đổi truy vấn ban đầu để sắp xếp kết quả theo các cột khác nhau trong tập kết quả. Cột trong mệnh đề ORDER BY có thể được chỉ định bằng chỉ mục của nó, vì vậy bạn không cần biết tên của bất kỳ cột nào. Khi chỉ số cột được chỉ định vượt quá số lượng cột thực tế trong tập kết quả, cơ sở dữ liệu sẽ trả về một lỗi, chẳng hạn như:  

```The ORDER BY position number 3 is out of range of the number of items in the select list.```


The application might actually return the database error in its HTTP response, but it may also issue a generic error response. In other cases, it may simply return no results at all. Either way, as long as you can detect some difference in the response, you can infer how many columns are being returned from the query.
> Ứng dụng có thể trả về lỗi SQL thực tế trong phản hồi HTTP của nó, nhưng nó cũng có thể phát ra phản hồi lỗi chung chung. Trong các trường hợp khác, nó có thể đơn giản là không trả về kết quả nào cả. Dù bằng cách nào, miễn là bạn có thể phát hiện một số khác biệt trong phản hồi, bạn có thể suy ra có bao nhiêu cột được trả về từ truy vấn.

The second method involves submitting a series of UNION SELECT payloads specifying a different number of null values:
> Phương pháp thứ hai liên quan đến việc gửi một chuỗi payloads UNION SELECT chỉ định một số lượng giá trị null khác nhau:

```
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
etc.
```

If the number of nulls does not match the number of columns, the database returns an error, such as:
> Nếu số lượng null không khớp với số lượng cột, cơ sở dữ liệu sẽ trả về một lỗi, chẳng hạn như:        

```All queries combined using a UNION, INTERSECT or EXCEPT operator must have an equal number of expressions in their target lists.```
> Tất cả các truy vấn được kết hợp bằng toán tử UNION, INTERSECT hoặc EXCEPT phải có số lượng biểu thức bằng nhau trong danh sách mục tiêu của chúng.

We use NULL as the values returned from the injected SELECT query because the data types in each column must be compatible between the original and the injected queries. NULL is convertible to every common data type, so it maximizes the chance that the payload will succeed when the column count is correct.
> Chúng ta sử dụng NULL làm giá trị được trả về từ truy vấn SELECT được chèn vào vì các kiểu dữ liệu trong mỗi cột phải tương thích giữa truy vấn gốc và truy vấn được chèn vào. NULL có thể chuyển đổi sang mọi kiểu dữ liệu phổ biến, vì vậy nó tối đa hóa cơ hội để payload thành công khi số lượng cột chính xác.

As with the ORDER BY technique, the application might actually return the database error in its HTTP response, but may return a generic error or simply return no results. When the number of nulls matches the number of columns, the database returns an additional row in the result set, containing null values in each column. The effect on the HTTP response depends on the application's code. If you are lucky, you will see some additional content within the response, such as an extra row on an HTML table. Otherwise, the null values might trigger a different error, such as a NullPointerException. In the worst case, the response might look the same as a response caused by an incorrect number of nulls. This would make this method ineffective.
> Giống như kĩ thuật ORDER BY, ứng dụng có thể thực sự trả về lỗi database trong phản hồi HTTP của nó, nhưng có thể trả về một lỗi chung chung hoặc đơn giản là không trả về kết quả nào cả. Khi số lượng null khớp với số lượng cột, cơ sở dữ liệu trả về một hàng bổ sung trong tập kết quả, chứa các giá trị null trong mỗi cột. Ảnh hưởng đến phản hồi HTTP phụ thuộc vào mã code của ứng dụng. Nếu may mắn, bạn sẽ thấy một số nội dung bổ sung trong phản hồi, chẳng hạn như một hàng bổ sung trên bảng HTML. Mặt khác, các giá trị null có thể gây ra lỗi khác, chẳng hạn như NullPointerException. Trường hợp xấu nhất, phản hồi có thể trông giống như phản hồi do số lượng null không chính xác gây ra. Điều này làm cho phương pháp này không hiệu quả. 


# Database-specific syntax
> Cú pháp đặc thù cho từng loại CSDL    

On Oracle, every SELECT query must use the FROM keyword and specify a valid table. There is a built-in table on Oracle called dual which can be used for this purpose. So the injected queries on Oracle would need to look like:
> Với Oracle, mọi truy vấn SELECT đều phải sử dụng từ khóa FROM và chỉ định một bảng hợp lệ. Có một bảng tích hợp sẵn trên Oracle tên là dual có thể được sử dụng cho mục đích này. Vì vậy, các truy vấn được chèn vào trên Oracle sẽ cần trông giống như:

```' UNION SELECT NULL FROM DUAL--```

The payloads described use the double-dash comment sequence -- to comment out the remainder of the original query following the injection point. On MySQL, the double-dash sequence must be followed by a space. Alternatively, the hash character # can be used to identify a comment.
> Các payloads mô tả sử dụng chuỗi comment double-dash -- để comment out phần còn lại của truy vấn gốc theo sau điểm injection. Trên MySQL, chuỗi double-dash phải theo sau bởi một khoảng trắng. Ngoài ra, ký tự hash # có thể được sử dụng để xác định một comment.   

For more details of database-specific syntax, see the SQL injection cheat sheet.

https://portswigger.net/web-security/sql-injection/cheat-sheet


# Finding columns with a useful data type
> Tìm cột có kiểu dữ liệu hữu ích   

A SQL injection UNION attack enables you to retrieve the results from an injected query. The interesting data that you want to retrieve is normally in string form. This means you need to find one or more columns in the original query results whose data type is, or is compatible with, string data.
> Tấn công SQL injection UNION cho phép bạn lấy kết quả từ một truy vấn được chèn vào. Dữ liệu thú vị mà bạn muốn truy xuất thường ở dạng chuỗi. Điều này có nghĩa là bạn cần tìm một hoặc nhiều cột trong kết quả truy vấn ban đầu có kiểu dữ liệu là, hoặc tương thích với, dữ liệu chuỗi.

After you determine the number of required columns, you can probe each column to test whether it can hold string data. You can submit a series of UNION SELECT payloads that place a string value into each column in turn. For example, if the query returns four columns, you would submit:
> Sau khi xác định số cột cần thiết, bạn có thể thăm dò từng cột để kiểm tra xem nó có thể chứa dữ liệu dạng chuỗi hay không. Bạn có thể gửi một loạt các payloads UNION SELECT đặt một giá trị chuỗi vào từng cột lần lượt. Ví dụ, nếu truy vấn trả về bốn cột, bạn sẽ gửi:

```
' UNION SELECT 'a',NULL,NULL,NULL--
' UNION SELECT NULL,'a',NULL,NULL--
' UNION SELECT NULL,NULL,'a',NULL--
' UNION SELECT NULL,NULL,NULL,'a'--
```
If the column data type is not compatible with string data, the injected query will cause a database error, such as:
> Nếu kiểu dữ liệu cột không tương thích với dữ liệu chuỗi, truy vấn được chèn vào sẽ gây ra lỗi cơ sở dữ liệu, chẳng hạn như:

```Conversion failed when converting the varchar value 'a' to data type int.``` 

If an error does not occur, and the application's response contains some additional content including the injected string value, then the relevant column is suitable for retrieving string data.
> Nếu không có lỗi xảy ra, và phản hồi của ứng dụng chứa một số nội dung bổ sung bao gồm cả giá trị chuỗi được chèn vào, thì cột liên quan phù hợp để truy xuất dữ liệu chuỗi.      

# Using a SQL injection UNION attack to retrieve interesting data
> Sử dụng SQLi UNION để truy xuất dữ liệu thú vị    

When you have determined the number of columns returned by the original query and found which columns can hold string data, you are in a position to retrieve interesting data.
> Khi bạn đã xác định được số lượng cột được trả về bởi truy vấn ban đầu và tìm thấy các cột có thể chứa dữ liệu chuỗi, bạn đang ở vị trí để truy xuất dữ liệu thú vị.  

Suppose that:
- The original query returns two columns, both of which can hold string data.
- The injection point is a quoted string within the WHERE clause.
- The database contains a table called users with the columns username and password.
> Giả sử rằng: 
> - Truy vấn ban đầu trả về hai cột, cả hai đều có thể chứa dữ liệu chuỗi. 
> - Điểm tiêm nhiễm là một chuỗi được trích dẫn trong mệnh đề WHERE. 
> - Cơ sở dữ liệu chứa một bảng tên là users với các cột username và password.      

In this example, you can retrieve the contents of the users table by submitting the input:

```' UNION SELECT username, password FROM users--```

In order to perform this attack, you need to know that there is a table called users with two columns called username and password. Without this information, you would have to guess the names of the tables and columns. All modern databases provide ways to examine the database structure, and determine what tables and columns they contain.

> Để thực hiện cuộc tấn công này, bạn cần biết rằng có một bảng tên là users với hai cột tên là username và password. Nếu không có thông tin này, bạn sẽ phải đoán tên của các bảng và cột. Tất cả các cơ sở dữ liệu hiện đại đều cung cấp các cách để kiểm tra cấu trúc của cơ sở dữ liệu, từ đó xác định những bảng và cột mà chúng chứa.

[Examining the database in SQL injection attacks](<Examining the database in SQL injection attacks.md>)

# Retrieving multiple values within a single column
> Truy xuất nhiều giá trị trong một cột duy nhất

In some cases the query in the previous example may only return a single column.
> Trong một số trường hợp, truy vấn trong ví dụ trước có thể chỉ trả về một cột duy nhất.

You can retrieve multiple values together within this single column by concatenating the values together. You can include a separator to let you distinguish the combined values. For example, on Oracle you could submit the input:

```' UNION SELECT username || '~' || password FROM users--```

> Trong trường hợp này, bạn có thể truy xuất nhiều giá trị cùng nhau trong một cột duy nhất bằng cách nối các giá trị lại với nhau. Bạn có thể bao gồm một dấu phân cách để cho phép phân biệt các giá trị kết hợp. Ví dụ, trên Oracle bạn có thể gửi đầu vào:

```' UNION SELECT username || '~' || password FROM users--```

The results from the query contain all the usernames and passwords, for example:
> Kết quả từ truy vấn bao gồm tất cả tên người dùng và mật khẩu, ví dụ:

```
...
administrator~s3cure
wiener~peter
carlos~montoya
...
```

Different databases use different syntax to perform string concatenation. For more details, see the SQL injection cheat sheet.
> Các cơ sở dữ liệu khác nhau sử dụng cú pháp khác nhau để thực hiện nối chuỗi. Để biết thêm chi tiết, xem bảng tóm tắt SQL injection.

https://portswigger.net/web-security/sql-injection/cheat-sheet