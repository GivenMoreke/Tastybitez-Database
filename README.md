# TastyBitez Database System

A fully structured relational database system built with **SQL Server** for managing a food ordering business. 

The system handles customers, menu items, inventory, orders, payments, and automated stock management — all backed by stored procedures, triggers, views, indexes, and role-based access control.

---

## Database Schema

The database consists of 6 core tables with well-defined relationships and constraints:

| Table | Description |
|-------|-------------|
| `Customers` | Stores customer profiles with unique phone and email |
| `MenuItems` | Food items with price validation (must be > 0) |
| `Inventory` | Tracks stock levels per menu item |
| `Orders` | Customer orders with status tracking (Pending / Completed / Cancelled) |
| `OrderItems` | Line items linking orders to menu items with quantities |
| `Payments` | Payment records linked to orders |

### Entity Relationships
- A **Customer** can place many **Orders** (1:M)
- An **Order** can contain many **OrderItems** (1:M)
- A **MenuItem** can appear in many **OrderItems** (M:N via OrderItems)
- Each **MenuItem** has exactly one **Inventory** entry (1:1)
- Each **Order** can have one **Payment** (1:1)

---

## Stored Procedures

| Procedure | Description |
|-----------|-------------|
| `PlaceOrder` | Places an order with stock validation — rolls back if insufficient stock |
| `RestockItem` | Adds stock to a menu item's inventory |
| `UpdateOrderStatus` | Updates order status with validation (Pending/Completed/Cancelled only) |
| `ProcessPayment` | Records a payment against an order |

---

## Triggers

| Trigger | Description |
|---------|-------------|
| `trg_CreateInventoryEntry` | Auto-creates an inventory entry (stock = 0) when a new menu item is added |
| `trg_UpdateStock` | Automatically decrements inventory when order items are inserted |
| `trg_RestoreStockOnCancel` | Restores inventory when an order status is changed to Cancelled |

---

## Views

| View | Description |
|------|-------------|
| `CustomerOrders` | Shows each customer's orders with status |
| `OrderDetails` | Shows order line items with item name, quantity, price, and total |
| `CustomerOrderHistory` | Shows total amount spent per customer per order |

---

## Indexes

```sql
CREATE NONCLUSTERED INDEX idx_customer_email ON Customers (Email);
CREATE NONCLUSTERED INDEX idx_order_status ON Orders (Status);
CREATE NONCLUSTERED INDEX idx_menu_price ON MenuItems (Price);
```

Indexes added on frequently queried columns to improve query performance.

---

## Role-Based Access Control

A `StaffRole` is defined with restricted permissions to enforce data security:

-  Can SELECT and INSERT on Orders and OrderItems
- Cannot UPDATE or DELETE Orders or OrderItems
-  No access to Payments table

---

## Key Constraints & Data Integrity

- `Price` must be greater than 0
- `StockLevel` cannot go below 0
- `Quantity` in OrderItems must be greater than 0
- `Status` in Orders is restricted to: `Pending`, `Completed`, `Cancelled`
- `PlaceOrder` uses transactions with TRY/CATCH and ROLLBACK to ensure data integrity
- Cascading deletes on Orders and Inventory when a Customer or MenuItem is removed

---

## Getting Started

### Prerequisites
- SQL Server (2019 or later recommended)
- SQL Server Management Studio (SSMS)

### Setup

1. Clone the repository
2. Open SSMS and connect to your local SQL Server instance
3. Open `CREATE DATABASE TastyBitez.sql`
4. Run the script — it will:
   - Create the database
   - Create all tables, indexes, triggers, stored procedures, and views
   - Insert sample data
   - Execute sample orders and payments

---

## Sample Data Included

- 3 customers
- 3 menu items (Burger R50, Kota R40, Grilled Meat R100)
- Initial stock restocked via `RestockItem`
- 2 sample orders placed via `PlaceOrder`
- 2 payments processed via `ProcessPayment`

---

## Key Concepts Demonstrated

- Relational database design and normalisation
- Primary keys, foreign keys, unique constraints, and check constraints
- Stored procedures with transaction management (BEGIN/COMMIT/ROLLBACK)
- Triggers for automated business logic
- Views for simplified data access
- Nonclustered indexes for query optimisation
- Role-based access control and permission management
- Cascading deletes and referential integrity
- SQL Server backup

---

## Author
Reitumetse Given Moreke

**Reitumetse Given Moreke**
Diploma in Information Technology — Belgium Campus iTVersity
[LinkedIn](#) | [GitHub](https://github.com/GivenMoreke)
