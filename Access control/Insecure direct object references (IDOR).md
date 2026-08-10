# Insecure direct object references (IDOR)

# What are insecure direct object references (IDOR)?

Insecure direct object references (IDOR) are a type of access control vulnerability that arises when an application uses user-supplied input to access objects directly. The term IDOR was popularized by its appearance in the OWASP 2007 Top Ten. However, it is just one example of many access control implementation mistakes that can lead to access controls being circumvented. IDOR vulnerabilities are most commonly associated with horizontal privilege escalation, but they can also arise in relation to vertical privilege escalation.
> Tham chiếu đối tượng trực tiếp không an toàn (IDOR) là một dạng lổ hổng kiểm soát truy cập, phát sinh khi ứng dụng sử dụng dữ liệu từ đầu vào do người dùng cung cấp để thực hiện kiểm soát truy cập trực tiếp vào các đối tượng. Thuật ngữ IDOR trở nên phổ biến nhờ được đưa vào danh sách Top Ten năm 2007. Tuy nhiên, đây chỉ là một trong số rất nhiều lỗi triển khai kiểm soát truy cấp dẫn đến việc kiểm soát truy cập bị vượt qua. Lổ hổng IDOR thường được liên kết với leo quyền theo chiều ngang, tuy nhiên vẫn có thể liên quan đến leo quyền theo chiều dọc. 

# IDOR examples

There are many examples of access control vulnerabilities where user-controlled parameter values are used to access resources or functions directly.
> Có rất nhiều ví dụ về kiểm soát truy cập, trong đó các giá trị tham số do người dùng kiểm soát trực tiếp truy cập vào tài nguyên hoặc chức năng.

# IDOR vulnerability with direct reference to database objects

Consider a website that uses the following URL to access the customer account page, by retrieving information from the back-end database:
> Hãy xem xét một trang web sử dụng URL sau để truy cập tài khoản khách hàng bằng cách truy xuất thông tin từ cơ sở dữ liệu

`https://insecure-website.com/customer_account?customer_number=132355`

Here, the customer number is used directly as a record index in queries that are performed on the back-end database. If no other controls are in place, an attacker can simply modify the customer_number value, bypassing access controls to view the records of other customers. This is an example of an IDOR vulnerability leading to horizontal privilege escalation.
> Ở đây, mã số khách hàng được sử dụng trực tiếp làm chỉ mục bản ghi trong các truy vấn thực hiện trên cơ sở dữ liệu. Nếu không có kiểm soát truy cập nào được triển khai, kẻ tấn công có thể đơn giản là thay đổi giá trị của customer_number, vượt qua kiểm soát truy cập để xem hồ sơ của khách hàng khác. Đây là một ví dụ về lỗ hổng IDOR dẫn đến leo quyền theo chiều ngang.  

An attacker might be able to perform horizontal and vertical privilege escalation by altering the user to one with additional privileges while bypassing access controls. Other possibilities include exploiting password leakage or modifying parameters once the attacker has landed in the user's accounts page, for example.
> Kẻ tấn công có thể thực hiện leo thang đặc quyền ngang và dọc bằng cách thay đổi định danh người dùng thành một người dùng có đặc quyền cao hơn, đồng thời vượt qua các cơ chế kiểm soát truy cập. Các khả năng khác bao gồm khai thác rò rỉ mật khẩu hoặc sửa đổi các tham số sau khi kẻ tấn công đã truy cập được vào trang tài khoản của người dùng.



