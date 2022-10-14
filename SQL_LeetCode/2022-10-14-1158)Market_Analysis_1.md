## **1158. Market Analysis 1**

<br>
<br>

### **문제**

<br>

- 사용자, 가입일, 2019년 구매자 주문 건수를 조회할 수 있는 SQL 쿼리 작성

<br>

#### **오답 문제풀이 1**

- Users, Orders를 INNER JOIN
- buyer_id로 그룹핑 후 가입일, 2019년에 구매한 물건 개수를 조회

<br>

    SELECT o.buyer_id
        , u.join_date
        , COUNT(o.buyer_id)
    FROM Users u
        INNER JOIN Orders o ON u.user_id = o.buyer_id
    WHERE order_date BETWEEN 2019-01-01 AND 2019-12-31
    GROUP BY o.buyer_id

<br>

#### **오답 문제풀이 2**

<br>

- 첫번째 문제풀이에서 WHERE 절에 있던 날짜 조건을 변경
- 2019년에 주문한 건만 뽑아내기 위해 CASE문으로 조건 설정

<br>

    SELECT o.buyer_id
        , u.join_date
        , CASE WHEN o.order_date LIKE '2019%' THEN COUNT(o.buyer_id) END AS orders_in_2019
    FROM Users u
        INNER JOIN Orders o ON u.user_id = o.buyer_id
    GROUP BY o.buyer_id

<br>

#### **정답 문제풀이**

<br>

- 정답 예시의 buyer_id만 보고 Users에 있는 user_id를 사용해야 하는 것을 Orders에 있는 buyer_id로 착각하였다
- 위의 오류코드들을 실행할 때마다 user_id 3번이 결과에 나오지 않아 계속 헤맸는데 INNER JOIN이 아닌 LEFT JOIN을 해주어야 했다
- 그리고 2019년으로 조건을 설정하기 위해 JOIN 조건에 order_date가 2019년인 것만 추출하도록 설정해주었다
- JOIN을 할 때마다 꼭 하나의 조건만 넣어야 하는건 아니라는 것을 잊는다

<br>

    SELECT u.user_id AS buyer_id
        , u.join_date
        , COUNT(o.buyer_id) AS orders_in_2019 
    FROM Users u 
        LEFT JOIN Orders o ON u.user_id = o.buyer_id AND YEAR(o.order_date) ='2019'
    GROUP BY u.user_id