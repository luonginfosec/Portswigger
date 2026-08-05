# Examining the database in SQL injection attacks
> Xem xét cơ sở dữ liệu trong tấn công SQL injection    

To exploit SQL injection vulnerabilities, it's often necessary to find information about the database. This includes:

- The type and version of the database software.
- The tables and columns that the database contains.

> Để khai thác các lỗ hổng SQL Injection, thường cần phải thu thập thông tin về cơ sở dữ liệu. Điều này bao gồm:
> - Loại và phiên bản của phần mềm cơ sở dữ liệu. 
> - Các bảng và cột mà cơ sở dữ liệu đó chứa.

# Querying the database type and version
You can potentially identify both the database type and version by injecting provider-specific queries to see if one works
> Bạn có thể xác định loại và phiên bản cơ sở dữ liệu bằng cách chèn các truy vấn cụ thể của nhà cung cấp và xem truy vấn nào hoạt động

The following are some queries to determine the database version for some popular database types:
> Dưới đây là một số truy vấn để xác định phiên bản cơ sở dữ liệu cho một số loại cơ sở dữ liệu phổ biến:

| Database type | Query |
|---|---|
| Microsoft, MySQL | SELECT @@version |
| Oracle | SELECT * FROM v$version |
| PostgreSQL | SELECT version() |

For example, you could use a UNION attack with the following input:
> Ví dụ, bạn có thể sử dụng tấn công UNION với đầu vào sau:

```' UNION SELECT @@version--```

This might return the following output. In this case, you can confirm that the database is Microsoft SQL Server and see the version used:
> Điều này có thể trả về kết quả sau. Trong trường hợp này, bạn có thể xác nhận rằng cơ sở dữ liệu là Microsoft SQL Server và xem phiên bản được sử dụng:

```
Microsoft SQL Server 2016 (SP2) (KB4052908) - 13.0.5026.0 (X64)
Mar 18 2018 09:11:49
Copyright (c) Microsoft Corporation
Standard Edition (64-bit) on Windows Server 2016 Standard 10.0 <X64> (Build 14393: ) (Hypervisor)
```

# Listing the contents of the database
> Liệt kê nội dung của cơ sở dữ liệu 

Most database types (except Oracle) have a set of views called the information schema. This provides information about the database.
> Hầu hết các loại cơ sở dữ liệu (ngoại trừ Oracle) có một tập hợp các view gọi là information schema. Cung cấp thông tin về cơ sở dữ liệu. 

For example, you can query information_schema.tables to list the tables in the database:
> Ví dụ, bạn có thể query information_schema.tables để liệt kê các bảng trong cơ sở dữ liệu:

```SELECT * FROM information_schema.tables```

This returns output like the following:
> Kết quả trả về như sau:

| TABLE_CATALOG  | TABLE_SCHEMA | TABLE_NAME | TABLE_TYPE |
|---|---|---|---|
| MyDatabase     | dbo          | Products  | BASE TABLE |
| MyDatabase     | dbo          | Users     | BASE TABLE |
| MyDatabase     | dbo          | Feedback  | BASE TABLE |

This output indicates that there are three tables, called Products, Users, and Feedback.
> Kết quả này cho biết có ba bảng tên là Products, Users và Feedback.

You can then query information_schema.columns to list the columns in individual tables:
> Sau đó, bạn có thể truy vấn information_schema.columns để liệt kê các cột trong từng bảng:

```SELECT * FROM information_schema.columns WHERE table_name = 'Users'```

This returns output like the following:

| TABLE_CATALOG  | TABLE_SCHEMA | TABLE_NAME | COLUMN_NAME  | DATA_TYPE |
|---|---|---|---|---|
| MyDatabase     | dbo          | Users     | UserId       | int       |
| MyDatabase     | dbo          | Users     | Username     | varchar   |
| MyDatabase     | dbo          | Users     | Password     | varchar   |

This output shows the columns in the specified table and the data type of each column.
> Kết quả này cho thấy các cột trong bảng được chỉ định và kiểu dữ liệu của mỗi cột.

# Listing the contents of an Oracle database
> Liệt kê nội dung của cơ sở dữ liệu Oracle

On Oracle, you can find the same information as follows:
> Trên Oracle, bạn có thể tìm thấy thông tin tương tự như sau:

You can list tables by querying all_tables:
> Bạn có thể liệt kê các bảng bằng cách query all_tables:

```SELECT * FROM all_tables```

You can list columns by querying all_tab_columns:
> Bạn có thể liệt kê các cột bằng cách query all_tab_columns:

```SELECT * FROM all_tab_columns WHERE table_name = 'USERS'```