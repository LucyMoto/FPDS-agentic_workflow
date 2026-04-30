# Semantic Layer - Music Streaming App

This folder contains the authoritative definitions for your database schema, organized by table for easy maintenance and scalability.

## File Structure

```
semantic_layer/
├── README.md                    ← You are here
├── relationships.yaml           ← Master index + join paths
├── golden_queries.yaml          ← Pre-validated query templates
├── users.yaml                   ← User master data
├── subscriptions.yaml           ← Subscription plans & status
├── sessions.yaml                ← User activity sessions
└── payments.yaml                ← Payment transactions
```

## Table Definitions

| File | Table | Purpose |
|------|-------|---------|
| `users.yaml` | users | User master data (signup_date, country, device_type) |
| `subscriptions.yaml` | subscriptions | Subscription records (plan: annual/free/monthly, status: active/canceled/expired) |
| `sessions.yaml` | sessions | User session activity (activity_type: browse/listen/read) |
| `payments.yaml` | payments | Payment transactions (method: apple_pay/card/google_pay/paypal) |

## Relationships

All join paths are defined in `relationships.yaml`:
- `subscriptions.user_id` → `users.user_id` (many-to-one)
- `sessions.user_id` → `users.user_id` (many-to-one)
- `payments.subscription_id` → `subscriptions.subscription_id` (many-to-one)

## Golden Queries

Pre-built, tested queries in `golden_queries.yaml`:
1. `revenue_by_plan_monthly` — Revenue by plan tier per month
2. `active_users_by_country` — Active subscriber count by country
3. `session_activity_summary` — Session metrics by activity type
4. `top_payment_methods` — Payment method popularity and revenue
5. `user_lifetime_value` — Total spend per user

## How to Use with SQL Assistant

### Option 1: Feed All Files to the Assistant
Copy the content of all YAML files and paste into the SQL Assistant prompt context. The assistant will use them as the single source of truth.

### Option 2: Copy Individual Files
If you modify one table (e.g., adding a new column to `payments.yaml`), copy just that file to the assistant context.

### Option 3: Use as Reference
Keep these files in your project folder and reference them when the assistant generates queries. Verify the generated SQL matches your actual schema.

## Maintenance

When adding a new table:
1. Create `new_table.yaml` following the same structure
2. Add foreign key references in `relationships.yaml`
3. Add new golden queries in `golden_queries.yaml` (optional)

When modifying an existing table:
1. Open the corresponding `.yaml` file
2. Add/remove dimensions or measures
3. Update `relationships.yaml` if foreign keys change

## Key Principles

✅ **No inference** — Only values that actually exist in the database
✅ **Modular** — Each table is independent; easy to find what needs changing
✅ **Version-control friendly** — Small, focused files with clear diffs
✅ **Scalable** — Easy to add new tables as your schema grows
✅ **Single source of truth** — All queries validate against these definitions
