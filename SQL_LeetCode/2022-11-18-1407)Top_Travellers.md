## **1407. Top Travellers**

<br>

### **문제**

<br>

- 유저별 이동한 distance를 구하기
- distance를 기준으로 내림차순 정렬하기
- 둘 이상의 유저가 같은 distance를 이동했다면 name을 기준으로 오름차순 정렬하기

<br>

#### **정답 문제풀이**

<br>

1. Users 테이블과 Rides 테이블을 LEFT JOIN
2. user_id별로 그룹핑하고 이동한 distance를 합하기(CASE문 사용)
3. 1, 2번 과정을 거친 테이블을 FROM절의 서브쿼리로 넣기
4. 정렬 기준에 맞춰 name, travelled_distance를 출력

<br>

    SELECT name, travelled_distance
    FROM (
        SELECT user_id
            , name
            , CASE 
                WHEN COUNT(user_id) >=2 THEN SUM(distance)
                WHEN COUNT(user_id) = 1 THEN distance
                ELSE 0
            END AS travelled_distance
        FROM Users u
            LEFT JOIN Rides r ON u.id = r.user_id
        GROUP BY user_id
    ) travel_distance
    ORDER BY 2 DESC, 1