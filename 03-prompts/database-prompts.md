# Database & ORM Prompts

## Purpose
A structured prompt library for database schema design, query optimization, migration generation, and data seeding. These prompts ensure the AI generates highly performant, normalized data structures and safe SQL/ORM operations that align with enterprise data integrity standards.

## When To Use
- Designing new database tables and establishing entity relationships (1:1, 1:N, N:M).
- Writing complex, high-performance database queries or aggregations.
- Generating safe migration scripts for existing production schemas.
- Creating seed scripts with realistic, relationally-accurate mock data.

---

## 1. The Schema Design & Modeling Prompt
*Use this when creating new database models via an ORM (e.g., SQLAlchemy, Prisma) to ensure proper normalization and indexing.*

**Prompt Template:**
```text
Task: Design the database models for [Insert Domain, e.g., the Subscription and Billing module].

Context:
- Database Engine: [e.g., PostgreSQL]
- ORM: [e.g., Prisma / SQLAlchemy]
- Review existing related models in @[Target_Schema_File].

Requirements:
1. Normalization: Design the tables up to the 3rd Normal Form (3NF) to eliminate data redundancy.
2. Relationships: Explicitly define the Foreign Keys, cascaded deletion rules, and bi-directional relationships if required by the ORM.
3. Indexing: Add compound indexes for columns that will frequently be queried together (e.g., `user_id` and `status`).
4. Constraints: Enforce strict constraints at the database level (UNIQUE, NOT NULL, CHECK constraints).

Constraint: Output only the ORM model definitions. Do not write the CRUD service functions yet.

```

---

## 2. The Query Optimization Prompt

*Use this to resolve slow database transactions or fix the N+1 query problem.*

**Prompt Template:**

```text
Task: Optimize a slow database query in @[Target_File].

Context:
- The function @[Function_Name] is causing a performance bottleneck.
- We suspect an N+1 query issue or a missing index on the underlying table.

Requirements:
1. Analysis: Identify why the current query implementation is inefficient (e.g., lazy loading inside a loop, cross-joining large tables).
2. Refactoring: Rewrite the query using explicit eager loading (e.g., `joinedload` in SQLAlchemy or `include` in Prisma) or proper SQL JOINs.
3. Memory Management: Ensure the optimized query filters data at the database level using `WHERE` clauses, rather than pulling all records into application memory to filter them.

Constraint: Maintain the exact same return type and payload structure as the original function to prevent breaking the service layer.

```

---

## 3. The Safe Migration Strategy Prompt

*Use this when altering existing tables that already contain production data.*

**Prompt Template:**

```text
Task: Write a safe database migration strategy to [Insert Action, e.g., change the `status` column from a string to an ENUM].

Context:
- Target Table: [Table Name]
- The table currently contains live production data. Data loss is unacceptable.

Requirements:
1. Zero-Downtime Approach: Detail the multi-step process required to make this change safely (e.g., Add new column -> Backfill data -> Drop old column -> Rename new column).
2. Migration Script: Provide the exact migration code or raw SQL required for the backfill and schema alteration.
3. Rollback Plan: Provide the exact 'down' migration or rollback script in case the deployment fails.

Constraint: Do not suggest destructive operations (like `DROP TABLE`) without an explicit, proven data-preservation pipeline.

```

---

## 4. The Relational Seed Data Prompt

*Use this to generate mock data scripts for local development and testing.*

**Prompt Template:**

```text
Task: Create a data seeding script for the [Insert Domain] models.

Context:
- Target ORM Models: @[Target_Schema_File]
- Programming Language: [e.g., Python / TypeScript]

Requirements:
1. Relational Integrity: Generate records that correctly reference one another. Ensure Parent records (e.g., Users) are created and their IDs are passed to Child records (e.g., Posts).
2. Realism: Use a library like `Faker` to generate realistic, professional mock data (e.g., valid enterprise emails, logical timestamps, realistic text).
3. Idempotency: Ensure the seed script is idempotent. It should clear or safely upsert existing data in the target tables before inserting new records to prevent unique constraint violations on re-runs.

Constraint: Output the executable seed script. Assume database connection logic is already handled in the environment.

```

---

## Best Practices for Database Prompts

* **Always specify the Database Engine:** MySQL, PostgreSQL, and SQLite handle certain data types (like JSONB or ENUMs) very differently. Always prime the AI with the exact engine.
* **Never blind-run Migrations:** Always read the AI-generated migration scripts carefully. AI models sometimes drop columns instead of renaming them.
* **Limit Context:** When optimizing a query, only provide the models involved in that specific query. Giving the AI the entire schema file wastes tokens and confuses the join logic.
