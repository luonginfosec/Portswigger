# Access control security models
> Mô hình bảo mật kiểm soát truy cập 

In this section we explain what access control security models are and we discuss the most commonly encountered models.
> Trong phần này, chúng ta giải thích các mô hình bảo mật kiểm soát truy cập là gì và chúng ta thảo luận về các mô hình thường gặp nhất.

What are access control security models?
> Các mô hình bảo mật kiểm soát truy cập là gì?

An access control security model is a formally defined definition of a set of access control rules that is independent of technology or implementation platform. Access control security models are implemented within operating systems, networks, database management systems and back office, application and web server software. Various access control security models have been devised over the years to match access control policies to business or organizational rules and changes in technology.
> Một mô hình kiểm soát truy cập là một định nghĩa xác thực chính thức về bộ quy tắc kiểm soát truy cập với công nghệ hoặc nền tảng triển khai. Các mô hình bảo mật kiểm soát truy cập được triển khai trong các hệ điều hành, mạng hệ quản trị cơ sở dữ liệu và phần mềm văn phòng, và ứng dụng máy chủ web. Nhiều mô hình bảo mật kiểm soát truy cập khác nhau đã được nghĩ ra trong nhiều năm qua để phù hợp với các chính sách kiểm soát truy cập và sự thay đổi của công nghệ của doanh nghiệp hoặc tổ chức. 

# Programmatic access control
> Kiểm soát truy cập theo chương trình

With programmatic access control, a matrix of user privileges is stored in a database or similar and access controls are applied programmatically with reference to this matrix. This approach to access control can include roles or groups or individual users, collections or workflows of processes and can be highly granular.
> Với kiểm soát truy cập theo chương trình, một ma trận quyền hạn người dùng được lưu trữ trong một cơ sở dữ liệu hoặc cái gì đó tương tự và các quyền tham chiếu đến dữ liệu. Cách tiếp cận kiểm soát truy cập này có thể bao gồm các vai trò hoặc nhóm hoặc người dùng cá nhân, bộ sưu tập hoặc quy trình này có thể rất chi tiết.

# Discretionary access control (DAC)
> Kiểm soát truy cập tùy ý (DAC)

With discretionary access control, access to resources or functions is constrained based upon users or named groups of users. Owners of resources or functions have the ability to assign or delegate access permissions to users. This model is highly granular with access rights defined to an individual resource or function and user. Consequently the model can become very complex to design and manage.
> Với kiểm soát truy cập tùy ý, quyền truy cập tài nguyên vào tài nguyên hoặc chức năng bị giới hạn dựa trên người dùng hoặc nhóm người dùng cụ thể. Chủ sở hữu tài nguyên hoặc chức năng có khả năng gán hoặc ủy quyền truy cập cho người dùng. Mô hình này có mức độ chi tiết rất cao, với các quyền truy cập dược xác định cho từng tài nguyên, chức năng và người dùng cụ thể. Do đó mô hình trở nên rất phức tạp trong thiết kế và quản lý. 

# Mandatory access control (MAC)
> Kiểm soát truy cập bắt buộc

Mandatory access control is a centrally controlled system of access control in which access to some object (a file or other resource) by a subject is constrained. Significantly, unlike DAC the users and owners of resources have no capability to delegate or modify access rights for their resources. This model is often associated with military clearance-based systems.
> Kiểm soát truy cập bắt buộc là một hệ thống kiểm soát truy cập được kiểm soát tập trung trong đó quyền truy cập vào một đối tượng (tệp hoặc tài nguyên khác) của một chủ thể bị hạn chế. Điều đáng chủ ý là, không giống như DAC, người dùng và chủ sở hữu tài nguyên không có khả năng ủy quyền hoặc sửa đổi quyền truy cập cho tài nguyên của họ. Mô hình này thường liên quan đến các hệ thống dựa trên mức độ bảo mật quân sự.

# Role-based access control (RBAC)
> Kiểm soát truy cập dựa trên vai trò (RBAC) 

With role-based access control, named roles are defined to which access privileges are assigned. Users are then assigned to single or multiple roles. RBAC provides enhanced management over other access control models and if properly designed sufficient granularity to provide manageable access control in complex applications. For example, the purchase clerk might be defined as a role with access permissions for a subset of purchase ledger functionality and resources. As employees leave or join an organization then access control management is simplified to defining or revoking membership of the purchases clerk role.
RBAC is most effective when there are sufficient roles to properly invoke access controls but not so many as to make the model excessively complex and unwieldy to manage.

> Trong mô hình kiểm soát truy cập dựa trên vai trò, các vai trò cụ thể sẽ được định nghĩa, và các quyền truy cập sẽ được gán cho các vai trò đó. Sau đó, người dùng được chỉ định vào một hoặc nhiều vai trò. RBAC mang lại khả năng quản lý tốt hơn so với các mô hình kiểm soát truy cập khác, và nếu được thiết kế đúng đắn, mô hình này cung cấp mức độ chi tiết đảm bảo kiểm soát truy cập có thể quản lý được ngay cả trong hệ thống phức tạp. Ví dụ vai trò "nhân viên mua hàng" có thể được định nghĩa là quyền truy cập vào một phần nhất định của chức năng và tài nguyên của sổ cái mua hàng. Khi nhân viên nghỉ việc hoặc gia nhập vào công ty, công tác quản lý và kiểm soát truy cập trở nên đơn giản hơn, chỉ cần cấp hoặc thu hồi tư cách thành viên vai trò của nhân viên mua hàng đó.

> RBAC phát huy hiệu quả cao khi số lượng vai trò đủ nhiều để thực thi đúng các quyền kiểm soát, nhưng không nhiều để làm cho mô hình trở nên phức tạp, cồng kềnh, khó quản lý.












