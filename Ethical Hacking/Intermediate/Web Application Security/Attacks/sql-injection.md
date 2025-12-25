# What is an SQL Injection?

An **SQL Injection (SQLi)** is an attack on a web application database server that causes malicious queries to be executed.  
When a web application communicates with a database using input from a user that hasn't been validated properly, there is the potential for an attacker to steal, delete, or alter private and customer data.

## SQL Query Types

SQL queries are used to interact with a database. Below are the different types of SQL queries and their examples:

### SQL Query Types Table

| **SQL Query Type** | **Description** | **Example** |
|--------------------|-----------------|-------------|
| **SELECT**         | Used to retrieve data from a database. | `SELECT * FROM users;` - Retrieves all data from the table `users`. |
|                    |                 | `SELECT * FROM users WHERE username='admin' OR username='jon';` - Retrieves all entries from the table `users` where the username is `admin` OR `jon`. |
| **UNION**          | Combines the results of two or more `SELECT` statements. | `SELECT name, address, city, postcode FROM customers UNION SELECT company, address, city, postcode FROM suppliers;` - Retrieves data for the columns `name`, `address`, `city`, and `postcode` from the table `customers` and retrieves data for the columns `company`, `address`, `city`, and `postcode` from the table `suppliers`. |
| **UPDATE**         | Used to modify existing records in a table. | `UPDATE users SET username='root', password='pass123' WHERE username='admin';` - Changes the username and password for the entry in the `users` table where the username is `admin`. |
| **DELETE**         | Used to remove records from a table. | `DELETE FROM users WHERE username='martin';` - Deletes the entry in the `users` table where the username is `martin`. |

## Types of SQLi Attacks

There are different types of **SQL Injection (SQLi)** attacks. Below are the various types, with examples of how they work:

### SQLi Attack Types Table

| **SQLi Attack Type** | **Description** | **Example** |
|----------------------|-----------------|-------------|
| **In-band**          | The easiest type of attack to detect. The same method of communication is used to exploit the vulnerability and also receive the results. | **Union-Based SQLi**: Uses the `UNION` operator to retrieve data from multiple tables. Example: `SELECT id, name FROM users UNION SELECT id, name FROM orders;` |
|                      |                 | **Error-Based SQLi**: Uses error messages from the database to gather information about the database structure. Example: `SELECT * FROM users WHERE id=1 AND 1=1; --` (may return an error message revealing table or column names). |
| **Blind SQLi**       | When you receive little to no feedback to confirm whether the injected queries were successful. The error messages are disabled or suppressed. | **Authentication Bypass**: Involves bypassing authentication systems like login forms. Example: `admin' OR '1'='1` (bypasses login by always returning true). |
|                      |                 | **Boolean-Based SQLi**: The attacker sends a query that results in a `true` or `false` response, allowing them to infer the correct answer. Example: `SELECT * FROM users WHERE username='admin' AND password='password' AND 1=1;` (true response) or `1=2;` (false response). |
|                      |                 | **Time-Based SQLi**: Similar to Boolean-based, but instead of visual feedback, the attacker uses time delays to determine if the query was successful. Example: `SELECT * FROM users WHERE username='admin' AND IF(1=1, SLEEP(5), 0);` (causes a time delay if true). |

### Explanation of Blind SQLi Types

- **Authentication Bypass**: This attack aims to bypass authentication mechanisms, such as login forms, by injecting queries that return true for all conditions, granting unauthorized access to the system.
  
- **Boolean-Based SQLi**: The attacker manipulates the query in such a way that it returns a `true` or `false` response. Based on this response, they can infer if the query was successful, slowly gaining information about the database structure.

- **Time-Based SQLi**: This method involves injecting queries that cause a time delay (e.g., `SLEEP`) if certain conditions are met. By observing how long the query takes to execute, attackers can determine whether the injected conditions are true or false.

---
