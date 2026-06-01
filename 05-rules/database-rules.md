# Database Design & Operations Rules

## Purpose
To establish strict architectural boundaries for database schema design, migration management, and query generation. These rules ensure data integrity, optimal querying performance, and safe evolution of the database schema in production environments.

## 1. Schema & Naming Conventions
- **Naming Standard:** Use `snake_case` strictly for all table names and column names.
- **Primary Keys:** Every table must have a primary key. Prefer UUIDs (`uuid4`) for distributed systems or external-facing records to prevent ID enumeration. Use auto-incrementing integers only for internal pivot tables.
- **Audit Trails:** Every core entity table must include `created_at` and `updated_at` columns, managed automatically by the database engine or ORM triggers.
- **Soft Deletes:** Avoid hard deletes for critical business data. Implement an `is_deleted` boolean or `deleted_at` timestamp column to maintain historical integrity.

## 2. Data Types & Normalization
- **Strict Typing:** Always use the most restrictive data type possible (e.g., `VARCHAR(255)` instead of `TEXT` for short strings, `BOOLEAN` instead of `INTEGER` for flags).
- **Enums:** Use database-level ENUM types for static categorical data to enforce integrity at the lowest level.
- **Normalization:** Design schemas up to the 3rd Normal Form (3NF). Only denormalize data when explicitly instructed for specific read-heavy optimization scenarios.

## 3. Indexing Strategy
- **Foreign Keys:** Every foreign key column must have an associated index.
- **Query Optimization:** Apply indexes to columns frequently used in `WHERE`, `ORDER BY`, or `GROUP BY` clauses (e.g., status, tenant_id).
- **Unique Constraints:** Use `UNIQUE` constraints at the database level to prevent duplicate records. Do not rely solely on application-level validation.

## 4. Migrations & Schema Alterations
- **Immutability:** Never alter a past migration file that has already been executed. Always create a new, forward-rolling migration script.
- **Destructive Actions:** The AI must NEVER generate a migration that drops a table, drops a column, or truncates data without explicit confirmation from the developer.
- **Default Values:** When adding a new non-nullable column to an existing table, always provide a default value to prevent migration crashes on existing rows.

## 5. ORM & Query Generation Constraints
- **Raw SQL Restriction:** Default to using the designated ORM (e.g., Prisma, SQLAlchemy). Only suggest raw SQL for complex analytical queries that the ORM cannot execute efficiently.
- **Eager Loading:** Prevent N+1 queries automatically. When generating service functions that fetch relational data, always include the necessary eager loading instructions.
- **Pagination:** Any query returning a list of records must implement pagination (Limit/Offset or Cursor-based) by default. Never generate queries that fetch entire tables into memory.