1.1 Inventory Item API
Scenario: Build an API for managing warehouse items.
Requirements:
- Create item: { name, sku, quantity, location }
- SKU must be unique.
- Quantity must be >= 0.
- Updating quantity must be idempotent (PUT sets, PATCH modifies).
- GET /items?location=A1&page=1&pageSize=20
- Return X-Total-Count header for pagination.
Best‑practice focus:
Validation, idempotency, pagination, unique constraints, filtering.

1.2 User Registration API
Scenario: Simple user onboarding.
Requirements:
- POST /users
- Validate email format
- Password must be hashed
- Email must be unique
- GET /users/{id}
- Return 409 Conflict on duplicate email
- Return 201 Created with Location header
Best‑practice focus:
Security basics, hashing, conflict handling, correct status codes.

2. 🔐 Auth + Tenant Isolation
2.1 Multi‑Tenant Notes API
Scenario: Users belong to organizations. Notes must be isolated per tenant.
Requirements:
- JWT contains userId and tenantId
- All queries must filter by tenantId
- Users can only CRUD their own notes
- Admins (role in JWT) can view all notes in their tenant
- Return 403 when accessing another tenant’s data
Best‑practice focus:
AuthZ vs AuthN, tenant isolation, role‑based access, secure filtering.

3. 🔄 Async Workflows + Queues
3.1 Image Processing API
Scenario: User uploads an image to be processed asynchronously.
Requirements:
- POST /images → store metadata, enqueue job
- GET /images/{id} returns:
- status: queued | processing | done | failed
- resultUrl only when done
- Worker simulates processing (resize, watermark, etc.)
- Idempotent job enqueueing (same image hash → same job)
Best‑practice focus:
Async patterns, job deduplication, polling, state transitions.

3.2 Email Notification API
Scenario: Trigger transactional emails.
Requirements:
- POST /notifications/email
- Validate template name
- Validate recipient
- Enqueue email job
- GET /notifications/{id} → job status
- Prevent duplicate sends using a requestId header
Best‑practice focus:
Idempotency keys, queue‑based workflows, validation.

4. 🧮 Data Manipulation + Business Logic
4.1 Order Pricing API
Scenario: Calculate final price for an order.
Requirements:
- POST /orders/price
- Input: { items: [{ productId, qty }], couponCode }
- Business rules:
- Bulk discount: qty ≥ 10 → 5% off that item
- Coupon: SAVE10 → 10% off total
- Tax: 7.5%
- Round to 2 decimals
- Return breakdown: subtotal, discounts, tax, total
Best‑practice focus:
Pure business logic, deterministic calculations, clear response modeling.

4.2 Timesheet Approval API
Scenario: Employees submit hours; managers approve.
Requirements:
- POST /timesheets
- PATCH /timesheets/{id}/approve
- Rules:
- Only manager can approve
- Cannot approve twice
- Cannot approve if hours exceed 60/week
- Return 422 for invalid state transitions
Best‑practice focus:
State machines, domain rules, error semantics.

5. 📄 Search + Filtering + Sorting
5.1 Product Catalog API
Scenario: Query a product catalog.
Requirements:
- GET /products?search=chair&sort=price:asc&minPrice=50&maxPrice=200&page=1&pageSize=10
- Support:
- Full‑text search
- Range filters
- Sort by multiple fields
- Pagination metadata in headers
- Return consistent ordering
Best‑practice focus:
Search semantics, sorting, pagination, query parsing.

6. 🧱 API Boundaries + Versioning
6.1 v1 → v2 Migration Challenge
Scenario: You have an existing API:
GET /v1/profile returns:
{ "firstName": "Kilian", "lastName": "Tavares", "age": 27 }


You must create /v2/profile with:
{ "fullName": "Kilian Tavares", "birthYear": 1999 }


Requirements:
- Maintain backward compatibility
- Add versioning via URL or header
- Reuse underlying domain model
- Deprecate v1 with a warning header
Best‑practice focus:
Versioning, backward compatibility, API evolution.

7. 🧪 Error Handling + Observability
7.1 Fault‑Injection API
Scenario: Build an API that intentionally triggers errors for testing.
Requirements:
- GET /test/timeout → simulate long-running task
- GET /test/error → throw controlled exception
- GET /test/validation → return structured validation errors
- Log correlationId from headers
- Return traceId in response
Best‑practice focus:
Structured errors, logging, correlation IDs, observability patterns.

8. 🧩 Edge Cases + Concurrency
8.1 Wallet Balance API
Scenario: Users can deposit and withdraw.
Requirements:
- POST /wallet/deposit
- POST /wallet/withdraw
- Prevent race conditions (use transactions or optimistic locking)
- Prevent negative balance
- Return updated balance atomically
Best‑practice focus:
Concurrency control, atomic operations, transactional integrity.

9. 🧼 Clean Architecture + Separation of Concerns
9.1 Book Library API
Scenario: CRUD books + borrow/return.
Requirements:
- Domain layer: book, user, loan
- Application layer: services
- API layer: controllers
- Borrowing rules:
- Cannot borrow if already borrowed
- Cannot return if not borrowed
- Return domain‑friendly errors
Best‑practice focus:
Layering, domain logic isolation, clean architecture.

Want me to turn these into…
- Daily drills (10–15 min each)
- A full interview prep roadmap
- A GitHub repo template
- A checklist for what “good” looks like for each challenge
Just tell me the format you want and I’ll shape it into something you can practice repeatedly until it’s second nature.
