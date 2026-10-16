# ERD — E-commerce Marketplace

## 1. User
Lưu thông tin tài khoản người dùng.

Quan hệ:
- Một User có nhiều User_Role.
- Một User có thể sở hữu nhiều Shop.
- Một User có một Cart đang hoạt động.
- Một User có nhiều Order.

## 2. Role
Lưu các vai trò truy cập của hệ thống.

Các role ban đầu:
- CUSTOMER
- ADMIN

Quan hệ:
- Một Role có thể được gán cho nhiều User thông qua User_Role.

## 3. User_Role
Bảng trung gian gán vai trò cho tài khoản.

Thuộc tính chính:
- user_id
- role_id

Ràng buộc:
- Cặp (user_id, role_id) phải duy nhất.

## 4. Shop
Đại diện cho cửa hàng của người bán.

Quan hệ:
- Một Shop thuộc về một User.
- Một Shop có nhiều Product.
- Một Shop có nhiều SellerOrder.

## 5. Category
Phân loại sản phẩm theo cấu trúc phân cấp.

Quan hệ:
- Một Category có thể có một Category cha.
- Một Category có nhiều Category con.
- Một Category có nhiều Product.

## 6. Product
Lưu thông tin chung của sản phẩm.

Quan hệ:
- Một Product thuộc một Shop.
- Một Product thuộc một Category.
- Một Product có nhiều ProductVariant.

## 7. ProductVariant
Lưu các phiên bản cụ thể của sản phẩm.

Thông tin chính:
- SKU
- Thuộc tính phiên bản
- Giá bán
- Tồn kho

Quan hệ:
- Một ProductVariant thuộc một Product.
- Một ProductVariant có thể xuất hiện trong nhiều CartItem.
- Một ProductVariant có thể xuất hiện trong nhiều OrderItem.

## 8. Cart
Đại diện cho giỏ hàng của User.

Quan hệ:
- Một Cart thuộc một User.
- Một Cart có nhiều CartItem.

## 9. CartItem
Lưu từng dòng sản phẩm trong giỏ hàng.

Thông tin chính:
- cart_id
- product_variant_id
- quantity

Quan hệ:
- Mỗi CartItem thuộc một Cart.
- Mỗi CartItem tham chiếu đến một ProductVariant.

## 10. Order
Đại diện cho một lần đặt hàng của khách hàng.

Quan hệ:
- Một Order thuộc một User.
- Một Order có nhiều SellerOrder.

Thông tin chính:
- Mã đơn hàng
- Tổng tiền
- Địa chỉ giao hàng
- Trạng thái
- Thời điểm đặt hàng

## 11. SellerOrder
Đại diện cho phần đơn hàng do một Shop xử lý.

Quan hệ:
- Mỗi SellerOrder thuộc một Order.
- Mỗi SellerOrder thuộc một Shop.
- Một SellerOrder có nhiều OrderItem.

## 12. OrderItem
Lưu chi tiết từng dòng sản phẩm đã mua.

Thông tin chính:
- seller_order_id
- product_variant_id
- product_name_snapshot
- sku_snapshot
- unit_price
- quantity

Quan hệ:
- Mỗi OrderItem thuộc một SellerOrder.
- Mỗi OrderItem tham chiếu đến một ProductVariant.
- Giá và thông tin snapshot được lưu để bảo toàn lịch sử mua hàng.

## Quy tắc nghiệp vụ
- Customer có thể vừa mua hàng vừa sở hữu Shop.
- Customer chỉ được quản lý các Shop thuộc quyền sở hữu của mình.
- Mỗi Product thuộc một Shop.
- Một Order có thể chứa sản phẩm từ nhiều Shop.
- Mỗi Shop xử lý phần đơn hàng của mình thông qua SellerOrder.
- Hệ thống kiểm tra giá và tồn kho trước khi xác nhận đặt hàng.
