---
layout: default
title: UPDATE (Alpha)
nav_exclude: true
search_exclude: true
description: Reference and syntax for the UPDATE command.
parent: SQL commands
---

# UPDATE (Alpha)
{: .no_toc}

Updates rows in the specified table.

{: .caution}
>**Alpha Release** 
>
>As we learn more from you, we may change the behavior and add new features. We will communicate any such changes. Your engagement and feedback are vital. 

* Topic ToC
{:toc}

## Syntax

```sql
UPDATE <table_name> SET <col1> = <expr1> [, <col2> = <expr2> ...] WHERE <condition>
```

| Parameter | Description|
| :---------| :----------|
| `<table_name>`| The table to update rows in. |
| `<col>`       | Name of the column to be updated. |
| `<expr>`      | Expression which computes new value to populate the column. It can reference any column from the row being updated.
| `<condition>` | A Boolean expression. Only rows for which this expression returns `true` will be updated. Condition can have subqueries doing semijoin with other table(s). |

### Example with WHERE

Apply discount for products which have excessive inventory.

```sql
UPDATE product SET price = price * 0.9 WHERE quantity > 10
```

Table before:

```
product
+------------+--------+----------+
| name       | price  | quantity |
+---------------------+----------+
| wand       |    120 |        9 |
| broomstick |    270 |       15 |
| robe       |     80 |        1 |
| cauldron   |     20 |      112 |
+------------+--------+----------+
```

Table after:

```
product
+------------+--------+----------+
| name       | price  | quantity |
+---------------------+----------+
| wand       |    120 |        9 |
| broomstick |    243 |       15 |
| robe       |     80 |        1 |
| cauldron   |     18 |      112 |
+------------+--------+----------+
```

### Example updating multiple columns

Apply discount for products which have excessive inventory.

```sql
UPDATE product SET price = 15, quantity = 100 WHERE name = 'quaffle'
```

Table before:

```
product
+------------+--------+----------+
| name       | price  | quantity |
+---------------------+----------+
| wand       |    120 |        9 |
| broomstick |    270 |       15 |
| quaffle    |        |          |
| robe       |     80 |        1 |
| cauldron   |     20 |      112 |
+------------+--------+----------+
```

Table after:

```
product
+------------+--------+----------+
| name       | price  | quantity |
+---------------------+----------+
| wand       |    120 |        9 |
| broomstick |    270 |       15 |
| quaffle    |     15 |      100 |
| robe       |     80 |        1 |
| cauldron   |     20 |      112 |
+------------+--------+----------+
```

### Known limitations

Below are some known limitations of the `UPDATE` command in the alpha release. 

* `UPDATE` cannot be used on tables that have aggregating or join indexes.

  In the alpha phase, `UPDATE` can’t be used on tables that have an aggregating or join index defined. An attempt to issue a `UPDATE` statement on a table with an aggregating or join index defined fails. In order for `UPDATE`s to succeed, table level aggregating or join indexes need to be dropped.

* Only one `UPDATE` will be executed against a table at once.
