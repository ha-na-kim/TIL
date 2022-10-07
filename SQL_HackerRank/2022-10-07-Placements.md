## **Placements**

<br>
<br>

### **문제**

<br>

- 절친이 본인보다 높은 급여를 제안받았을 때의 본인 이름 출력하기
- 절친이 제공받은 급여로 정렬하기
- 어떤 사람도 동일한 급여를 받지 않음

<br>

#### **문제풀이**

<br>

- 출력될 이름이 있는 Students 테이블을 기준으로 INNER JOIN 진행
- 본인 급여를 알기 위해 Packages 테이블과 ID를 기준으로 INNER JOIN
- 본인의 절친을 알기 위해 Friends 테이블과 ID를 기준으로 INNER JOIN
- 절친의 급여를 알기 위해 Friends 테이블과 Packages 테이블을 Friend_ID, ID로 INNER JOIN

<br>

    SELECT s.Name
    FROM Students s
        INNER JOIN Packages p ON s.ID = p.ID
        INNER JOIN Friends f ON s.ID = f.ID
        INNER JOIN Packages friend_p ON f.Friend_ID = friend_p.ID
    WHERE p.Salary < friend_p.Salary
    ORDER BY friend_p.Salary