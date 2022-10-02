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

<br>

#### **정답 문제풀이 2**

<br>

- Window 함수 이용하기
- 부서별 max_salary를 Window 함수로 표현하고 From절에 서브쿼리로 넣음
- Where절에서 실제 salary와 max_salary가 일치하는 데이터만 뽑아주는 조건 설정

<br>

    SELECT Department
        , Employee
        , max_salary AS Salary
    FROM (
        SELECT d.name AS Department
            , e.name AS Employee
            , e.salary
            , MAX(salary) OVER (PARTITION BY departmentId) max_salary
        FROM Employee e
            INNER JOIN Department d ON e.departmentId = d.id
        ) sal
    WHERE sal.salary = sal.max_salary