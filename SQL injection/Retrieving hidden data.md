# Retrieving hidden data
> Trích xuất dữ liệu ẩn 

Imagine a shopping application that displays products in different categories. When the user clicks on the Gifts category, their browser requests the URL:

```https://insecure-website.com/products?category=Gifts```

> Hãy tưởng tượng một ứng dụng mua sắm hiển thị sản phẩm theo các danh mục khác nhau. Khi người dùng nhấp vào danh mục Quà tặng, trình duyệt của họ sẽ yêu cầu URL:

```https://insecure-website.com/products?category=Gifts```

This causes the application to make a SQL query to retrieve details of the relevant products from the database:

```SELECT * FROM products WHERE category = 'Gifts' AND released = 1```

This SQL query asks the database to return:

- all details (*)
- from the products table
- where the category is Gifts
- and released is 1.

> Truy vấn SQL này yêu cầu cơ sở dữ liệu trả về:
- tất cả các trường (*)
- từ bảng products
- trong đó category là Gifts
- và released là 1.

The restriction ```released = 1``` is being used to hide products that are not released. We could assume for unreleased products, ```released = 0```.
> Ràng buộc ```released = 1``` được sử dụng để ẩn các sản phẩm chưa được phát hành. Chúng ta có thể giả sử đối với các sản phẩm chưa được phát hành, ```released = 0``` 

The application doesn't implement any defenses against SQL injection attacks. This means an attacker can construct the following attack, for example:

```https://insecure-website.com/products?category=Gifts'--```

This results in the SQL query:

```SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1```

> Ứng dụng không thực hiện bất kỳ biện pháp bảo vệ nào chống lại các cuộc tấn công SQL injection. Điều này có nghĩa là kẻ tấn công có thể xây dựng cuộc tấn công sau đây, ví dụ:        

```https://insecure-website.com/products?category=Gifts'--```

Điều này dẫn đến truy vấn SQL:  

```SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1```

Crucially, note that -- is a comment indicator in SQL. This means that the rest of the query is interpreted as a comment, effectively removing it. In this example, this means the query no longer includes AND released = 1. As a result, all products are displayed, including those that are not yet released.

You can use a similar attack to cause the application to display all the products in any category, including categories that they don't know about:

```https://insecure-website.com/products?category=Gifts'+OR+1=1--```

This results in the SQL query:

```SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1```

The modified query returns all items where either the category is Gifts, or 1 is equal to 1. As 1=1 is always true, the query returns all items.

> Điều quan trọng cần lưu ý là -- là ký hiệu comment trong SQL. Điều này có nghĩa là phẩn còn lại của câu truy vấn sẽ bị hiểu là chú thích và loại bỏ hoàn toàn khỏi quá trình thực thi. Trong ví dụ này , truy vấn sẽ không còn chứa điều kiện AND released = 1. Do đó , tất cả các sản phẩm sẽ được hiển thị , bao gồm cả những sản phẩm chưa được phát hành.         
Bạn cũng có thể sử dụng một cuộc tấn công tương tự để buộc ứng dụng hiển thị tất cả sản phẩm trong bất kỳ danh mục nào, thậm chí là những danh mục mà ứng dụng không hề biết đến:

```https://insecure-website.com/products?category=Gifts'+OR+1=1--```

> Điều này dẫn đến truy vấn SQL:

```SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1```

> Truy vấn đã bị sửa đổi này sẽ trả về tất cả các mục mà hoặc category = 'Gifts' hoặc 1=1. Vì 1=1 luôn luôn đúng, truy vấn sẽ trả về toàn bộ các mục trong bảng.

Take care when injecting the condition OR 1=1 into a SQL query. Even if it appears to be harmless in the context you're injecting into, it's common for applications to use data from a single request in multiple different queries. If your condition reaches an UPDATE or DELETE statement, for example, it can result in an accidental loss of data.
> Hãy hết sức cẩn trọng khi chèn điều kiện OR 1=1 vào một truy vấn SQL. Ngay cả khi nó có vẻ vô hại trong ngữ cảnh bạn đang khai thác, các ứng dụng thường có xu hướng sử dụng dữ liệu từ một yêu cầu (request) duy nhất cho nhiều truy vấn khác nhau. Ví dụ, nếu điều kiện của bạn lọt vào một câu lệnh UPDATE hoặc DELETE, nó có thể dẫn đến việc mất dữ liệu ngoài ý muốn.

