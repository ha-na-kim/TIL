## **626. Exchange Seats**

<br>
<br>

### **문제**

<br>

- 연속된 두 학생의 시트 ID를 교환하는 쿼리를 작성
- 학생 수가 홀수일 경우 마지막 학생의 ID가 교환되지 않음
- ID로 오름차순 정렬

<br>

#### **오답 문제풀이**

<br>

- CASE 구문과 LEAD, LAG 윈도우 함수를 사용
- id의 나머지가 1이고 마지막 학생이 아닐 경우, LEAD로 student 이름을 1칸 뒤로 밀고, id의 나머지가 0일 경우, LAG로 student 이름을 1칸 당길 수 있도록 구문 작성
- 내가 예상한 결과값이 나오지 않고 null값만 나옴..

<br>

    SELECT id
        , CASE
            WHEN MOD(id, 2)=1 AND id NOT LIKE MAX(id) THEN LEAD(student, 1) OVER (ORDER BY id)
            WHEN MOD(id, 2)=0 THEN LAG(student, 1) OVER (ORDER BY id)
            ELSE student
        END AS student
    FROM Seat

<br>

#### **정답 문제풀이**

<br>

- 위의 쿼리에서 id가 마지막 학생이 아닐 경우 그냥 MAX(id) 하는 것이 아닌 서브쿼리롤 MAX(id)를 뽑아준 후 NOT LIKE를 해야 했음

<br>

    SELECT id
        , CASE
            WHEN MOD(id, 2)=1 AND id NOT LIKE (SELECT MAX(id) FROM Seat) THEN LEAD(student, 1) OVER (ORDER BY id)
            WHEN MOD(id, 2)=0 THEN LAG(student, 1) OVER (ORDER BY id)
            ELSE student
        END AS student
    FROM Seat