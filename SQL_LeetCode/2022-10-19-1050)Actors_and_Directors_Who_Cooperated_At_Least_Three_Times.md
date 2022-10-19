## **1050. Actors and Directors Who Cooperated At Least Three Times**

<br>

### **문제**

<br>

- actor와 direcetor가 적어도 3번 이상 협력한 actor_id, director_id 출력하기

<br>

#### **문제풀이**

<br>

- Window 함수를 이용해 actor_id, director_id가 몇 번 협력했는지 COUNT하고, FROM 절의 서브쿼리에 넣어 테이블로 사용
- actor_id, director_id로 그룹핑 후, 협력이 3번 이상인 것을 조건으로 넣어 id 출력

<br>

    SELECT actor_id, director_id
    FROM (
        SELECT actor_id
            , director_id
            , COUNT(actor_id) OVER (PARTITION BY actor_id, director_id) AS ad_count
        FROM ActorDirector
    ) count_ad
    GROUP BY actor_id, director_id
    HAVING COUNT(ad_count) >= 3