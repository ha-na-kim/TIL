## **184. Department Highest Salary**

<br>
<br>

###  부서별로 어떤 사람이 가장 high salary인지 찾기

<br>

- Employee 테이블과 부서별로 가장 높은 임금을 받는 서브쿼리 테이블을 INNER JOIN
- 부서명이 Department 테이블에 명시되어 있기 때문에 INNER JOIN

<br>

    SELECT d.name AS Department
        , e.name AS Employee
        , e.salary
    FROM Employee e
    INNER JOIN (
        SELECT departmentId, MAX(salary) AS max_salary
        FROM Employee
        GROUP BY departmentId
        ) dh ON e.departmentId = dh.departmentId AND e.salary = dh.max_salary
    INNER JOIN Department d ON e.departmentId = d.id