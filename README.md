# sql-challenge
CREATE TABLE departments (
  dept_no VARCHAR DEFAULT false,
  dept_name VARCHAR NOT NULL
);

DROP TABLE departments

CREATE TABLE departments (
  dept_no VARCHAR NOT NULL,
  dept_name VARCHAR NOT NULL
);

SELECT * FROM departments;

CREATE TABLE dept_emp (
  emp_no VARCHAR NOT NULL,
  dept_no VARCHAR NOT NULL
);

SELECT * FROM dept_emp;

CREATE TABLE dept_manager (
  dept_no VARCHAR NOT NULL,
  emp_no VARCHAR NOT NULL
);

SELECT * FROM dept_manager;

CREATE TABLE employees (
  emp_no VARCHAR NOT NULL,
  emp_title_id VARCHAR NOT NULL,
  birth_date DATE NOT NULL,
  first_name VARCHAR,
  last_name VARCHAR,
  sex VARCHAR,
  hire_date DATE
);

SELECT * FROM employees;

CREATE TABLE salaries (
  emp_no VARCHAR NOT NULL,
  salary INT NOT NULL
);

SELECT * FROM salaries;

CREATE TABLE titles (
  title_id VARCHAR NOT NULL,
  title VARCHAR NOT NULL
);

SELECT * FROM titles;

ALTER TABLE departments ADD primary key (dept_no);

ALTER TABLE employees ADD primary key (emp_no);

Data Analysis
Once you have a complete database, do the following:


--List the following details of each employee: employee number, last name, first name, sex, and salary.
SELECT employees.emp_no, last_name, first_name, sex, salary FROM employees
JOIN (SELECT emp_no, salary FROM salaries) as S
ON employees.emp_no = S.emp_no;

--List first name, last name, and hire date for employees who were hired in 1986.
SELECT first_name, last_name, hire_date FROM employees
WHERE hire_date >= '1986-01-01' AND hire_date <= '1986-12-31';

--List the manager of each department with the following information: 
--department number, department name, the manager's employee number, last name, first name.
SELECT departments.dept_no, departments.dept_name, dept_manager.emp_no, employees.last_name, employees.first_name FROM departments
JOIN dept_manager
ON departments.dept_no = dept_manager.dept_no
JOIN employees 
on dept_manager.emp_no = employees.emp_no;

--List the department of each employee with the following information: employee number, last name, first name, and department name.
select employees.emp_no, employees.last_name, employees.first_name, departments.dept_name
from employees join dept_emp on employees.emp_no = dept_emp.emp_no
join departments on dept_emp.dept_no = departments.dept_no

--List first name, last name, and sex for employees whose first name is "Hercules" and last names begin with "B."
select first_name, last_name, sex from employees
where first_name = 'Hercules' and last_name like 'B%'

--List all employees in the Sales department, including their employee number, last name, first name, and department name.
select employees.emp_no, employees.last_name, employees.first_name, departments.dept_name from employees
join dept_emp on employees.emp_no = dept_emp.emp_no
join departments on dept_emp.dept_no = departments.dept_no
where dept_name = 'Sales'

--List all employees in the Sales and Development departments, including their employee number, last name, first name, and department name.
select employees.emp_no, employees.last_name, employees.first_name, departments.dept_name from employees
join dept_emp on employees.emp_no = dept_emp.emp_no
join departments on dept_emp.dept_no = departments.dept_no
where dept_name = 'Sales' or dept_name = 'Development'

--In descending order, list the frequency count of employee last names, i.e., how many employees share each last name.
select last_name, count(last_name) from employees group by last_name order by count DESC
