# window_functions

```
-- selecting maximum salary from each department
select department_name, max(salary)as max_salary
from `data`.hr h 
group by department_name 

-- selecting all records in the table and lso adding the max salary of all values column at the end
select h.*,
MAX(salary) over() as max_salary
from `data`.hr h 

-- selecting all records while adding a column at the ned showing maximum salary for each department department
select h.*,
max(salary) over(PARTITION by department_name) as max_salary
FROM `data`.hr h 


-- fetching  records and sorting by salary 
SELECT h.*,
ROW_NUMBER ()over() as rn
from `data`.hr h 

-- ranking salary by department
SELECT h.*,
ROW_NUMBER ()over(PARTITION by department_name  ORDER by salary desc) as rn 
FROM `data`.hr h 

-- fetch top two employees from each department
select * from(
  SELECT h.*,
  ROW_NUMBER ()over(PARTITION by department_name  ORDER by salary desc) as rn 
  FROM `data`.hr h ) x
WHERE x.rn<3




    --   RANK  ( skips a value when it finds two rows that are similar)
-- FETCHING TOP 3 EMPLOYEES IN EACH DEPARTMENT EARNING MAX SALARY
SELECT * from(select h.*,
RANK ()over(PARTITION by department_name ORDER by salary desc)as rn1
from `data`.hr h ) x
where x.rn1 <3

     -- USING DENSE RANK ( does not skip a value when it dinds two values that are simlar)
select h.*,
RANK ()over(PARTITION by department_name ORDER by salary desc)as rn1,
DENSE_RANK ()over(PARTITION by department_name ORDER by salary desc)as rn2

from `data`.hr h 


         -- LAG
-- fetches a query to display if salary of an employee is higher,lower or equal to previous employee in the same department
SELECT h.*,
lag(salary) over (PARTITION by department_name order by employee_id ) prev_emp_sal
from `data`.hr h 

        -- LEAD
-- fetches a query to display if salary of an employee is higher,lower or equal to next employee in the same department
SELECT h.*,
LEAD (salary) over (PARTITION by department_name order by employee_id ) prev_emp_sal
from `data`.hr h 
```
