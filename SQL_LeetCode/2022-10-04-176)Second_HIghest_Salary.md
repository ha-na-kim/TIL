## **176. Second Highest Salary**

<br>
<br>

### **두번째로 높은 급여 구하기**

<br>

#### **오답 문제풀이**

<br>

- salary로 내림차순 정렬을 하고 limit을 사용하여 문제를 품
- 두번째로 높은 급여가 없을 경우, null을 출력하라고 하였으나 해당 쿼리에서는 null 출력 X

<br>

    SELECT salary AS SecondHighestSalary
    FROM Employee
    ORDER BY salary DESC
    limit 1,1

<br>

    SELECT IF(MAX(salary), salary, NULL) AS SecondHighestSalary
    FROM Employee
    WHERE salary < (SELECT MAX(salary) FROM Employee)

<br>

#### **정답 문제풀이**

<br>

- MySQL에서 MAX()는 값이 없을 경우, NULL값을 반환함

<br>

    SELECT MAX(salary) AS SecondHighestSalary
    FROM Employee
    WHERE salary < (SELECT MAX(salary) FROM Employee)