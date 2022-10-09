## **177. Nth Highest Salary**

<br>
<br>

### **n번째로 높은 급여 출력하기**

<br>

- 만약 n번째로 높은 급여가 없다면 null 출력하기

<br>

#### **오답 문제풀이**

<br>

    CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
    BEGIN
        set N = N - 1;
        RETURN (
            SELECT salary
            FROM Employee
            ORDER BY salary DESC
            LIMIT N, 1
            );
    END

<br>

#### **정답 문제풀이**

<br>

    CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
    BEGIN
        set N = N - 1;
        RETURN (
            SELECT DISTINCT salary
            FROM Employee
            ORDER BY salary DESC
            LIMIT N, 1
            );
    END