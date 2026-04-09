# Comic-Con Parking System - Database Design

## Entity-Relationship Diagram
![Comic-Con ER Diagram](/Comic-Con-Parking-System/comic-con-parking-system.jpeg)

---

## Project Overview
This database design provides a normalized architecture for a multi-zone, high-traffic Comic-Con parking facility. It handles diverse vehicle types, reserved categories, multi-day passes, and historical session tracking.

## Key Architectural Decisions

1. **Infrastructure vs. Transactions:** Separates static physical space (`parking_spots`) from temporal events (`parking_sessions`) to maintain a historical occupancy ledger without overwriting data.
2. **Hierarchical Design:** Modeled as Levels -> Zones -> Spots. Specialized access is managed at the spot level via `spot_access_categories`.
3. **Multi-Entry Pass Logic:** A one-to-many relationship between `parking_tickets` and `parking_sessions` supports multi-day passes, allowing multiple distinct entries per ticket.
4. **Dynamic Pricing & Operations:** `pricing_rules` and `ticket_types` calculate fees dynamically. The `vehicles` table captures make, model, and color to assist operations staff.
5. **Real-Time Availability:** Calculated dynamically rather than using a static boolean. Spots are occupied if a related `parking_sessions` record has a `NULL` exit time.

## Entities

* **`parking_levels`**: Physical floors of the facility.
* **`parking_zones`**: Sub-sections for traffic routing and reservations.
* **`parking_spots`**: Individual, numbered spaces within a zone.
* **`spot_access_categories`**: Defines access requirements (e.g., VIP, EV).
* **`vehicles`**: Vehicle identifying details and owner contact info.
* **`vehicle_categories`**: Classifies vehicles to determine capacity and pricing.
* **`ticket_types`**: Defines the pass duration, type, and base price.
* **`parking_tickets`**: The purchased pass instance tied to a specific vehicle.
* **`pricing_rules`**: Connects zones and vehicle types to exact fee rates.
* **`parking_sessions`**: The core ledger recording entry, exit, spot, and ticket for each physical visit.
* **`payment_records`**: Financial settlement tied to specific parking sessions.