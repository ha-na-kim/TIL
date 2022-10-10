## **601. Human Traffic of Stadium**

<br>
<br>

### **문제**

<br>

- ID가 연속으로 3개 이상이고, 방문자수가 100명 이상인 경우 출력
- visit_date 순으로 오름차순 정렬

<br>

#### **오답 문제풀이**

<br>

- WITH문으로 방문자수가 100 이상인 테이블을 만들고
- WITH문으로 만든 테이블을 2번 셀프조인하여 id가 3 이상으로 연속적인 경우만 출력되도록 쿼리를 작성
- 그러나 output에는 연속적인 id가 2번 밖에 출력되지 않았음

<br>

    WITH visit_100 AS(
        SELECT *
        FROM Stadium
        WHERE people >= 100
    )
    SELECT v1.*
    FROM visit_100 v1
        INNER JOIN visit_100 v2 ON v1.id + 1 = v2.id
        INNER JOIN visit_100 v3 ON v2.id + 1 = v3.id

<br>

#### **정답 문제풀이**

<br>

- id가 3 이상으로 연속적인 경우를 어떻게 추출할 수 있을지 고민했으나 지속적인 오류와 해답을 찾지 못하고 구글링을 통해 문제풀이를 해결하였음
- WITH문에 방문자수가 100명 이상, id 기준 ROW_NUM을 붙인 테이블을 만들기
- 테이블에서 만든 ROW_NUM인 grp가 3 이상인 것만 추출하기

<br>

    WITH visit_100 AS (
        SELECT *
            , id - row_number() OVER(ORDER BY id) AS grp
        FROM Stadium
        WHERE people >= 100
    )
    SELECT id, visit_date, people
    FROM visit_100
    WHERE grp IN (
        SELECT grp
        FROM visit_100
        GROUP BY grp
        HAVING COUNT(*) >=3
    )