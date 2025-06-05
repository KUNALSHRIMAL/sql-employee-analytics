# SQL Project – Employee Analytics

This mini project contains SQL queries analyzing employee and department data.

## 📊 Key Analyses:
- Department-wise employee count and average salary
- High-earning employees above department average
- Salary band classification using CASE
- Recently joined employees (last 1 year)
- Departments with no employees

## 📁 Files:
- `employee_analytics.sql` – All SQL queries
- `high_earning_employees.csv` – Sample output from query


## Codes 
### Department Overview
SELECT d.department, COUNT(e.id) AS num_employees, 
       ROUND(AVG(e.salary), 2) AS avg_salary
FROM departments d
LEFT JOIN employees e ON d.department = e.department
GROUP BY d.department;
### High-Earning Employees (above department average)
SELECT e.name, e.department, e.salary
FROM employees e
WHERE salary > (
  SELECT AVG(salary)
  FROM employees
  WHERE department = e.department
);
### Salary Category Report
SELECT name, salary,
  CASE
    WHEN salary >= 60000 THEN 'High'
    WHEN salary BETWEEN 50000 AND 59999 THEN 'Medium'
    ELSE 'Low'
  END AS salary_band
FROM employees
ORDER BY salary DESC;
### Recently Joined Employees (last 1 year)
SELECT name, joining_date
FROM employees
WHERE joining_date >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR);
### Unmatched Departments
SELECT d.department
FROM departments d
LEFT JOIN employees e ON d.department = e.department
WHERE e.id IS NULL;
