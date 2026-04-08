# Fitness Influencer Coaching Platform - Database Design

## ER Diagram
* Or click this image link: [ER Diagram](https://ibb.co/NgXV50w4)

---

## Key Design Decisions

### 1. The "Enrollment Hub" Architecture
Instead of linking payments and check-ins directly to a client, they are linked to the `client_plan_enrollments` table. This correctly models the reality of online coaching: a check-in report or a payment is tied to a **specific instance** of a client taking a **specific plan** over a **specific time period**, not just the user's general account.

### 2. Entity Subtyping for Coaching Plans
To accommodate the prompt's requirement to handle both Diet and Workout plans abstractly without cluttering a single table, the design uses a subtyping pattern.
* `coaching_plans` acts as the parent table holding shared attributes (name, category, price, duration).
* `fitness_plan_details` and `diet_plan_details` hold the specific variants.

### 3. Consolidated Progress & Check-ins
Rather than fragmenting physical metrics (weight/BMI) into a separate table from behavioral metrics (energy levels/sleep), all asynchronous weekly reporting was merged into a single, powerful `check_in_reports` table. This aligns with real-world coaching workflows where clients submit a unified weekly form.

### 4. Lean, Domain-Specific User Modeling
Because the prompt explicitly states this is an online coaching ecosystem (Insta DMs and Meet calls) and **not** a gym management system, physical address tables and fragmented medical histories were intentionally avoided. The `clients` table was kept lean, retaining only what is necessary for online coaching (e.g., timezone/country for scheduling, basic fitness goals).

### 5. Decoupled Live Sessions
Live video calls (sessions) are correctly decoupled from async `check_in_reports`. A session tracks real-time meetings between a trainer and client, complete with meeting links and statuses, distinct from the client's weekly progress updates.