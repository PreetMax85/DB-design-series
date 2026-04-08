# Instagram Thrift Creator Store — Database Design
ER diagram for a small Instagram-based store selling thrifted and handmade fashion items.

## Overview
Designed to support:
* Product catalog with thrifted and handmade item types
* Customer order management
* Payment and shipping tracking

## Entities (Tables)
* `users` — Customers and admin, identified by `role`
* `products` — Unified product table with `productType` field
* `thrifted_products` — Extends `products`: condition, size
* `handmade_products` — Extends `products`: material, dimensions
* `orders` — Customer orders with delivery address
* `order_details` — Junction table, resolves order ↔ product many-to-many
* `payment_details` — Payment method, status, amount, timestamp
* `shipping_details` — Courier info, tracking, delivery status

## Key Design Decisions
* **Supertype-subtype pattern** for products — shared fields in `products`, type-specific fields in child tables (Handmade & Thrifted).
* `order_details` **junction table** handles one order containing multiple products.
* **Single `users` table** with `role` field instead of separate customer/admin tables.
* `shipping_details.status` tracks full lifecycle: `pending` → `shipped` → `out_for_delivery` → `delivered` → `returned`.