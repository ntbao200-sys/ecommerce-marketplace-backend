# Requirements — E-commerce Marketplace

## 1. Mục tiêu dự án
Xây dựng nền tảng thương mại điện tử marketplace cho phép nhiều người bán kinh doanh sản phẩm trên cùng một hệ thống.

## 2. Vai trò người dùng
- **CUSTOMER:** Đăng ký, đăng nhập, tìm kiếm sản phẩm, quản lý giỏ hàng, đặt hàng và xem lịch sử mua hàng. Customer cũng có thể tạo shop, đăng bán và quản lý sản phẩm thuộc shop của mình.
- **ADMIN:** Quản lý tài khoản người dùng, kiểm duyệt shop và sản phẩm, quản lý hoạt động của hệ thống.

## 3. Chức năng chính
- Xác thực và phân quyền người dùng.
- Quản lý shop và sản phẩm.
- Quản lý danh mục sản phẩm.
- Quản lý phiên bản sản phẩm, giá bán và tồn kho.
- Quản lý giỏ hàng.
- Đặt hàng và theo dõi trạng thái đơn hàng.
- Quản lý đơn hàng theo từng shop.
- Quản lý tài khoản và kiểm duyệt nội dung bởi Admin.

## 4. Quy tắc nghiệp vụ
- Một tài khoản có thể vừa mua hàng vừa bán hàng.
- Một sản phẩm thuộc một shop.
- Khách hàng có thể mua sản phẩm từ nhiều shop trong cùng một lần đặt hàng.
- Một đơn hàng tổng được chia thành các đơn hàng con theo từng shop.
- Mỗi shop chỉ được quản lý bởi chủ sở hữu hợp lệ.
- Hệ thống phải kiểm tra giá và tồn kho trước khi xác nhận đặt hàng.
- Đơn hàng phải lưu giá sản phẩm tại thời điểm mua để đảm bảo lịch sử giao dịch chính xác.

## 5. Phạm vi ban đầu (MVP)
Ưu tiên xây dựng xác thực, phân quyền, quản lý shop, quản lý sản phẩm, giỏ hàng và quy trình đặt hàng. Các chức năng nâng cao sẽ được bổ sung sau.
