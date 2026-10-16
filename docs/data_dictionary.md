# DATA DICTIONARY — E-COMMERCE MARKETPLACE

## 1. User

**Description:** Stores information about users in the system.

| Field | Data Type | Constraints | Description |
|---|---|---|---|
| id | BIGINT | PK | Unique identifier of the user |
| full_name | VARCHAR(100) | NOT NULL | Full name of the user |
| email | VARCHAR(255) | UNIQUE, NOT NULL | User's email address used for login |
| password_hash | VARCHAR(255) | NOT NULL | Hashed password |
| phone | VARCHAR(20) | NULL | User's phone number |
| status | VARCHAR(20) | NOT NULL | Account status |
| created_at | DATETIME | NOT NULL | Account creation timestamp |
| updated_at | DATETIME | NOT NULL | Last update timestamp |

## 2. Role

**Description:** Stores the roles available in the system.

| Field | Data Type | Constraints | Description |
|---|---|---|---|
| id | BIGINT | PK | Unique identifier of the role |
| name | VARCHAR(50) | UNIQUE, NOT NULL | Role name: CUSTOMER or ADMIN |
| description | VARCHAR(255) | NULL | Description of the role |

## 3. User_Role

**Description:** Associates users with their assigned roles.

| Field | Data Type | Constraints | Description |
|---|---|---|---|
| user_id | BIGINT | PK, FK → User.id | User assigned to the role |
| role_id | BIGINT | PK, FK → Role.id | Role assigned to the user |

**Constraints:**
- The combination of `user_id` and `role_id` must be unique.
- A user can have one or more roles, depending on the system's authorization design.
- The marketplace has two roles: `CUSTOMER` and `ADMIN`.
- A customer can also become a seller by creating and owning a shop. A separate SELLER role is not required.

## 4. Shop

**Description:** Stores information about shops owned by users.

| Field | Data Type | Constraints | Description |
|---|---|---|---|
| id | BIGINT | PK | Unique identifier of the shop |
| owner_id | BIGINT | FK → User.id, NOT NULL | User who owns the shop |
| name | VARCHAR(150) | NOT NULL | Shop name |
| description | TEXT | NULL | Shop description |
| status | VARCHAR(20) | NOT NULL | Shop status, such as PENDING, ACTIVE, or SUSPENDED |
| created_at | DATETIME | NOT NULL | Shop creation timestamp |
| updated_at | DATETIME | NOT NULL | Last update timestamp |

**Business Rules:**
- A user can own one or more shops, depending on the marketplace's policies.
- Only the shop owner or an authorized administrator can manage the shop.
- A user does not need a separate SELLER role to create a shop.

## 5. Category

**Description:** Stores product categories and supports hierarchical categorization.

| Field | Data Type | Constraints | Description |
|---|---|---|---|
| id | BIGINT | PK | Unique identifier of the category |
| parent_id | BIGINT | FK → Category.id, NULL | Parent category; NULL if it is a root category |
| name | VARCHAR(100) | NOT NULL | Category name |
| description | VARCHAR(255) | NULL | Category description |
| status | VARCHAR(20) | NOT NULL | Category status |
| created_at | DATETIME | NOT NULL | Category creation timestamp |

**Business Rules:**
- A category can have multiple subcategories.
- A category can contain multiple products.
- A root category has no parent category.

## 6. Product

**Description:** Stores general information about products listed by shops.

| Field | Data Type | Constraints | Description |
|---|---|---|---|
| id | BIGINT | PK | Unique identifier of the product |
| shop_id | BIGINT | FK → Shop.id, NOT NULL | Shop that lists the product |
| category_id | BIGINT | FK → Category.id, NOT NULL | Category to which the product belongs |
| name | VARCHAR(200) | NOT NULL | Product name |
| description | TEXT | NULL | Product description |
| status | VARCHAR(20) | NOT NULL | Product status, such as DRAFT, ACTIVE, or HIDDEN |
| created_at | DATETIME | NOT NULL | Product creation timestamp |
| updated_at | DATETIME | NOT NULL | Last update timestamp |

**Business Rules:**
- Each product belongs to one shop.
- Each product belongs to one category.
- A product can have multiple variants.
- Only the shop owner or an authorized administrator can manage the product.

## 7. ProductVariant

**Description:** Stores specific versions of a product, such as different sizes, colors, or capacities.

| Field | Data Type | Constraints | Description |
|---|---|---|---|
| id | BIGINT | PK | Unique identifier of the product variant |
| product_id | BIGINT | FK → Product.id, NOT NULL | Product to which the variant belongs |
| sku | VARCHAR(100) | UNIQUE, NOT NULL | Unique stock-keeping unit code |
| attributes | JSON/TEXT | NULL | Variant attributes, such as color or size |
| price | DECIMAL(12,2) | NOT NULL | Current selling price |
| stock_quantity | INT | NOT NULL | Available inventory quantity |
| status | VARCHAR(20) | NOT NULL | Variant status |
| created_at | DATETIME | NOT NULL | Variant creation timestamp |
| updated_at | DATETIME | NOT NULL | Last update timestamp |

**Business Rules:**
- Each variant belongs to one product.
- A product can have multiple variants.
- Stock quantity must not be negative.
- Price must not be negative.
- Each SKU must be unique.

## 8. Cart

**Description:** Represents a shopping cart belonging to a user.

| Field | Data Type | Constraints | Description |
|---|---|---|---|
| id | BIGINT | PK | Unique identifier of the cart |
| user_id | BIGINT | FK → User.id, UNIQUE, NOT NULL | User who owns the cart |
| created_at | DATETIME | NOT NULL | Cart creation timestamp |
| updated_at | DATETIME | NOT NULL | Last update timestamp |

**Business Rules:**
- Each cart belongs to one user.
- This design assumes each user has at most one active cart.
- A cart can contain multiple cart items.

## 9. CartItem

**Description:** Stores individual product variants and their quantities in a shopping cart.

| Field | Data Type | Constraints | Description |
|---|---|---|---|
| id | BIGINT | PK | Unique identifier of the cart item |
| cart_id | BIGINT | FK → Cart.id, NOT NULL | Cart containing the item |
| product_variant_id | BIGINT | FK → ProductVariant.id, NOT NULL | Selected product variant |
| quantity | INT | NOT NULL | Quantity selected by the user |
| created_at | DATETIME | NOT NULL | Timestamp when the item was added |
| updated_at | DATETIME | NOT NULL | Last update timestamp |

**Constraints and Business Rules:**
- The combination of `cart_id` and `product_variant_id` must be unique.
- Quantity must be greater than zero.
- The cart stores product variants rather than products directly, because different variants may have different prices and stock quantities.

## 10. Order

**Description:** Represents an order placed by a customer. An order may contain products from multiple shops.

| Field | Data Type | Constraints | Description |
|---|---|---|---|
| id | BIGINT | PK | Unique identifier of the order |
| order_number | VARCHAR(50) | UNIQUE, NOT NULL | Human-readable order number |
| user_id | BIGINT | FK → User.id, NOT NULL | Customer who placed the order |
| total_amount | DECIMAL(12,2) | NOT NULL | Total amount of the order |
| status | VARCHAR(30) | NOT NULL | Overall order status |
| shipping_address | VARCHAR(500) | NOT NULL | Delivery address recorded when the order was placed |
| created_at | DATETIME | NOT NULL | Order creation timestamp |
| updated_at | DATETIME | NOT NULL | Last update timestamp |

**Business Rules:**
- Each order belongs to one customer.
- An order can contain products from multiple shops.
- When an order includes products from different shops, it is divided into multiple SellerOrders.
- The order status represents the overall state of the customer's order.

## 11. SellerOrder

**Description:** Represents the portion of an order that is handled by a specific shop.

| Field | Data Type | Constraints | Description |
|---|---|---|---|
| id | BIGINT | PK | Unique identifier of the seller order |
| order_id | BIGINT | FK → Order.id, NOT NULL | Parent order |
| shop_id | BIGINT | FK → Shop.id, NOT NULL | Shop responsible for processing the order |
| subtotal | DECIMAL(12,2) | NOT NULL | Total product amount for this shop |
| shipping_fee | DECIMAL(12,2) | NOT NULL | Shipping fee for this seller order |
| status | VARCHAR(30) | NOT NULL | Seller order processing status |
| created_at | DATETIME | NOT NULL | Seller order creation timestamp |
| updated_at | DATETIME | NOT NULL | Last update timestamp |

**Constraints and Business Rules:**
- The combination of `order_id` and `shop_id` must be unique.
- Each SellerOrder belongs to one Order and one Shop.
- Each SellerOrder can contain multiple OrderItems.
- Each shop manages only the products and fulfillment associated with its own SellerOrder.
- SellerOrder status can be updated independently of other seller orders belonging to the same Order.

## 12. OrderItem

**Description:** Stores individual product lines purchased as part of a SellerOrder.

| Field | Data Type | Constraints | Description |
|---|---|---|---|
| id | BIGINT | PK | Unique identifier of the order item |
| seller_order_id | BIGINT | FK → SellerOrder.id, NOT NULL | Seller order containing the item |
| product_variant_id | BIGINT | FK → ProductVariant.id, NOT NULL | Product variant purchased |
| product_name_snapshot | VARCHAR(200) | NOT NULL | Product name at the time of purchase |
| sku_snapshot | VARCHAR(100) | NOT NULL | SKU at the time of purchase |
| unit_price | DECIMAL(12,2) | NOT NULL | Unit price at the time of purchase |
| quantity | INT | NOT NULL | Quantity purchased |
| subtotal | DECIMAL(12,2) | NOT NULL | Total price for this order item |

**Business Rules:**
- Each OrderItem belongs to one SellerOrder.
- Quantity must be greater than zero.
- `unit_price` stores the purchase price, which may differ from the product's current price.
- Snapshot fields preserve product information at the time of purchase, even if the seller later updates the product.
- The subtotal is calculated as `unit_price × quantity`.

## 13. General Conventions

- **PK (Primary Key):** Uniquely identifies each record in a table.
- **FK (Foreign Key):** References a record in another table.
- **UNIQUE:** Ensures that a value or combination of values is unique.
- **NOT NULL:** Requires a value to be provided.
- **NULL:** Allows a field to have no value.
- **BIGINT:** Used for identifiers that may require a large numeric range.
- **VARCHAR(n):** Stores variable-length text with a defined maximum length.
- **TEXT:** Stores longer text content.
- **DECIMAL(12,2):** Stores monetary values with two decimal places. It is preferred over floating-point types for money.
- **DATETIME:** Stores date and time information. The exact type may vary by database.
- **JSON/TEXT:** Stores structured variant attributes. Choose the appropriate implementation based on the selected database.

## 14. Important Design Notes

1. The system has two roles: `CUSTOMER` and `ADMIN`. A customer can purchase products and become a seller by owning a shop.
2. Seller permissions should be validated through shop ownership, not by assuming that every seller must have a separate role.
3. `CartItem` is necessary to store the selected product variant and quantity in a cart.
4. `ProductVariant` separates product information from variant-specific prices, SKUs, and inventory.
5. `Order` represents the customer's overall purchase, while `SellerOrder` allows each shop to process its own portion independently.
6. `OrderItem` stores purchase-time prices and product snapshots to preserve historical order details.
7. Data types and status values should be finalized according to the chosen database and application requirements.
8. Foreign keys, indexes, validation rules, and transaction handling should be implemented consistently in the database and Java backend.
