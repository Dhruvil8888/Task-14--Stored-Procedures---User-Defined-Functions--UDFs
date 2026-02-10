# Task 14: Stored Procedures & User-Defined Functions (UDFs)

## 📌 Project Overview
This task demonstrates how to implement **Stored Procedures** and **User-Defined Functions (UDFs)** in SQL to encapsulate reusable business logic inside the database.  
These techniques are widely used in backend systems to improve performance, maintainability, and consistency.

---

## 🛠 Tools & Technologies
- **Primary:** MySQL Workbench  
- **Alternatives:** PostgreSQL, BigQuery Sandbox, SQLite  

---

## 📂 Repository Structure
- `task14_procedures_functions.sql` → SQL file containing stored procedures and functions  
- `README.md` → Project documentation (this file)

---

## 🎯 Learning Objectives
- Create stored procedures with parameters
- Call procedures and verify results
- Implement error handling inside procedures
- Create user-defined functions for calculations
- Understand differences between procedures and functions
- Optimize reusable database logic
- Map database logic to backend services

---

## 🗄 Tables Used
- **employees**

---

## 🧪 SQL Code Examples

### 1. Stored Procedure: Insert Employee
```sql
DELIMITER $$

CREATE PROCEDURE add_employee (
    IN p_name VARCHAR(60),
    IN p_email VARCHAR(100),
    IN p_department_id INT,
    IN p_salary DECIMAL(10,2)
)
BEGIN
    IF p_salary <= 0 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Salary must be greater than zero';
    END IF;

    INSERT INTO employees (emp_name, email, department_id, salary)
    VALUES (p_name, p_email, p_department_id, p_salary);
END$$

DELIMITER ;
2. Calling the Stored Procedure
CALL add_employee(
    'Suresh Kumar',
    'suresh@gmail.com',
    2,
    52000
);
3. User-Defined Function: Tax Calculation
DELIMITER $$

CREATE FUNCTION calculate_tax (
    salary DECIMAL(10,2)
)
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
    RETURN salary * 0.10;
END$$

DELIMITER ;
4. Using the Function in SELECT
SELECT 
    emp_name,
    salary,
    calculate_tax(salary) AS tax_amount
FROM employees;
5. Bonus Calculation Function
DELIMITER $$

CREATE FUNCTION calculate_bonus (
    salary DECIMAL(10,2)
)
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
    RETURN salary * 0.15;
END$$

DELIMITER ;
⚖️ Stored Procedures vs Functions
Feature	Stored Procedure	Function
Returns value	Optional	Mandatory
Used in SELECT	❌ No	✅ Yes
DML operations	✅ Yes	❌ Limited
Error handling	✅ Yes	❌ No
Best use	Business logic	Calculations
🌍 Real-World Applications
Use Case	SQL Object
Employee onboarding	Stored Procedure
Salary processing	Stored Procedure
Tax calculation	Function
Bonus calculation	Function
Backend APIs	Stored Procedures
