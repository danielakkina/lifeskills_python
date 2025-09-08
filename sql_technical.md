# Database Concepts — Technical Paper

---

## Abstract

This paper introduces key concepts in database systems that every engineer and computer science student should understand. We cover **ACID properties, CAP theorem, Joins, Aggregations and Filters, Normalization, Indexes, Transactions, Locking mechanisms, Database Isolation Levels, and Triggers**. Examples and diagrams are provided where helpful, with PostgreSQL and SQL syntax references.

---

## Table of Contents

1. ACID Properties
2. CAP Theorem
3. Joins
4. Aggregations and Filters
5. Normalization
6. Indexes
7. Transactions
8. Locking Mechanism
9. Database Isolation Levels
10. Triggers
11. Conclusion

---

## 1. ACID Properties

ACID is a set of four properties that guarantee reliable database transactions:

1. **Atomicity** — All steps in a transaction succeed or none do.
   *Example:* Transferring money: debit from one account and credit to another must both succeed.

2. **Consistency** — Database constraints must always hold before and after a transaction.
   *Example:* Balance cannot become negative if constraints forbid it.

3. **Isolation** — Concurrent transactions should not interfere with each other’s results.
   *Example:* Two users buying the last product simultaneously should be handled correctly.

4. **Durability** — Once a transaction is committed, its data changes persist even after crashes.
   *Example:* After confirming an order, the order record must remain in the database even if the server restarts.

---

## 2. CAP Theorem

The **CAP theorem** applies to distributed databases and states that out of three guarantees, only two can be achieved at the same time:

* **Consistency (C):** Every read receives the most recent write.
* **Availability (A):** Every request receives a response (not necessarily the latest data).
* **Partition Tolerance (P):** The system continues to operate despite network partitions.

**Trade-offs:**

* CP systems: Prioritize consistency and partition tolerance (e.g., HBase, MongoDB with strong consistency).
* AP systems: Prioritize availability and partition tolerance (e.g., Cassandra, DynamoDB).
* CA systems: Possible only in single-node or non-partitioned environments.

---

## 3. Joins

**Joins** combine rows from multiple tables based on a related column.

### Types of Joins

* **INNER JOIN:** Returns matching rows from both tables.

```sql
SELECT u.username, o.order_id
FROM users u
INNER JOIN orders o ON u.user_id = o.user_id;
```

* **LEFT JOIN:** Returns all rows from the left table, with NULLs where there is no match.

```sql
SELECT u.username, o.order_id
FROM users u
LEFT JOIN orders o ON u.user_id = o.user_id;
```

* **RIGHT JOIN:** Returns all rows from the right table, with NULLs where no match exists.
* **FULL OUTER JOIN:** Returns all rows from both tables, with NULLs where no match.

---

## 4. Aggregations and Filters

Aggregation functions summarize data:

* **COUNT, SUM, AVG, MIN, MAX**

Example:

```sql
SELECT user_id, COUNT(*) AS total_orders
FROM orders
GROUP BY user_id
HAVING COUNT(*) > 5;
```

**Filters:**

* `WHERE` filters rows before aggregation.
* `HAVING` filters groups after aggregation.

```sql
SELECT category, AVG(price)
FROM products
WHERE price > 10
GROUP BY category
HAVING AVG(price) > 100;
```

---

## 5. Normalization

**Normalization** is the process of structuring relational databases to reduce redundancy and improve integrity.

* **1NF (First Normal Form):** No repeating groups or arrays; each column has atomic values.
* **2NF (Second Normal Form):** Must be in 1NF and every non-key attribute fully depends on the primary key.
* **3NF (Third Normal Form):** Must be in 2NF and no transitive dependency between non-key attributes.

**Denormalization:** Sometimes used for performance optimization (fewer joins, faster reads).

---

## 6. Indexes

Indexes improve query performance by allowing faster lookups.

**Types:**

* **B-Tree Index:** Default, good for range queries.
* **Hash Index:** Good for equality checks.
* **GIN/GiST Indexes:** Specialized for full-text search and complex data types.

Example:

```sql
CREATE INDEX idx_orders_user_id ON orders(user_id);
```

**Note:** Indexes speed up reads but slow down writes (INSERT/UPDATE/DELETE).

---

## 7. Transactions

A **transaction** is a sequence of one or more SQL operations treated as a single logical unit.

Example:

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

If any step fails, you can roll back:

```sql
ROLLBACK;
```

---

## 8. Locking Mechanism

Databases use locks to ensure data integrity during concurrent access.

**Types of Locks:**

* **Shared Locks (Read Locks):** Multiple transactions can read, but none can write.
* **Exclusive Locks (Write Locks):** Prevents others from reading or writing.

**Deadlock:** When two transactions wait indefinitely for each other’s lock.

**Optimistic vs Pessimistic Locking:**

* **Optimistic:** Assume minimal conflicts, validate at commit.
* **Pessimistic:** Lock resources before using them.

---

## 9. Database Isolation Levels

SQL defines four isolation levels (ANSI SQL standard):

1. **Read Uncommitted:** Allows dirty reads.
2. **Read Committed:** Prevents dirty reads, but non-repeatable reads may occur. (Default in PostgreSQL)
3. **Repeatable Read:** Prevents dirty and non-repeatable reads, but phantom reads may occur.
4. **Serializable:** Highest level; prevents all concurrency anomalies.

Example in PostgreSQL:

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

---

## 10. Triggers

**Triggers** are functions that automatically execute in response to certain events (INSERT, UPDATE, DELETE).

Example:

```sql
CREATE OR REPLACE FUNCTION log_user_update()
RETURNS TRIGGER AS $$
BEGIN
  INSERT INTO audit_log(user_id, changed_at)
  VALUES (NEW.user_id, now());
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_user_update
AFTER UPDATE ON users
FOR EACH ROW EXECUTE FUNCTION log_user_update();
```

---

## 11. Conclusion

Databases rely on strong foundational concepts like **ACID, CAP theorem, Normalization, Indexes, and Isolation levels** to ensure reliability and scalability. Understanding these is crucial for designing robust applications that handle data correctly under concurrent, distributed, and high-performance workloads.

---

## References

* PostgreSQL Documentation ([https://www.postgresql.org/docs/](https://www.postgresql.org/docs/))
* "Database System Concepts" by Silberschatz, Korth, Sudarshan
* "Designing Data-Intensive Applications" by Martin Kleppmann
