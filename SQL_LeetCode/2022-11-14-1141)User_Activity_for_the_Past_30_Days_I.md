## **1141. User Activity for the Past 30 Days I**

<br>

### **문제**

<br>

- 2019-07-27까지 30일 동안의 DAU 찾기
- 사용자가 해당 날짜에 적어도 하나의 활동을 만든 경우 해당 사용자는 언젠가 활성화

<br>

#### **오답 문제풀이**

<br>

- WHERE절 조건을 통해 날짜를 30일간으로 설정
- 적어도 하나의 활동을 의미하는 'open_session'을 한 유저를 조건으로 설정
- 일자별 DAU를 보기 위해 activity_date로 그룹핑

<br>

    SELECT activity_date AS day
        , COUNT(user_id) AS active_users
    FROM Activity
    WHERE activity_date BETWEEN 2019-06-27 AND 2019-07-27
    AND activity_type = 'open_session'
    GROUP BY activity_date

<br>

#### **정답 문제풀이**

<br>

- 위의 오답 쿼리에서 WHERE절의 30일간 날짜를 설정할 때 따옴표 안에 날짜를 넣었어야 함
- 오답 쿼리의 activity_type = 'open_session'을 조건으로 넣어주게 되면 'open_session'을 하지 않고 다른 행동을 한 유저를 집계할 수 없어 제외함 (사용자가 제일 먼저 하는 행동이 open_session일거라 생각했는데 아니었음)

<br>

    SELECT activity_date AS day
        , COUNT(DISTINCT user_id) AS active_users
    FROM Activity
    WHERE activity_date BETWEEN '2019-06-28' AND '2019-07-27'
    GROUP BY activity_date