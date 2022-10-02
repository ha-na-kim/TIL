## **185. Department Top Three Salaries**

<br>
<br>

### **부서별 Top Three 연봉자 출력하기**

<br>

- 높은 연봉순으로 순위를 부여할 때 중복되는 근로자가 있다면 그 다음 순위는 DENSE_RANK로 할 것
- Window 함수를 이용해서 높은 연봉순으로 순위를 매김
- Window 함수를 이용한 결과 테이블을 From절에 서브쿼리로 넣어주고 부서명을 붙여주기 위해 Department 테이블과 INNER JOIN
- Top Three 연봉자를 출력하라고 했으므로 WHERE절에 순위가 3 이하인 근로자만 선택하는 조건 설정

<br>

    SELECT d.name AS Department
        , dense_sal.name AS Employee
        , dense_sal.salary AS Salary
    FROM (
        SELECT departmentID
            , name
            , salary
            , DENSE_RANK() OVER (PARTITION BY departmentId ORDER BY salary DESC) AS denserank_salary
        FROM Employee
    ) dense_sal
        INNER JOIN Department d ON dense_sal.departmentId = d.id
    WHERE denserank_salary <= 3