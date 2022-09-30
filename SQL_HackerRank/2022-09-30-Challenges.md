## **Chhallenges**

<br>
<br>

### **문제**

<br>

- hacker_id, name, the total number of challenges 출력하기
- the total number of challenges 내림차순으로 정렬하기
- 한명 이상이 같은 넘버를 가진 챌린지를 만들었다면 hacker_id로 오름차순 정렬
- 한명 이상이 똑같은 챌린지를 만들고, 최대 챌린지 생성 개수보다 작으면 결과에서 제외

<br>

### **작성순서**

<br>

- hacker_id별로 challenges 개수 세기
- 내림차순 sort하기
- challenges 개수가 중복이고 maximum 개수보다 작으면 결과에서 빼기

<br>

#### **오답 문제풀이**

<br>

1. Challenges 테이블의 hacker_id로 group by 후 challenges 개수 세기
2. HAVING절에 CASE 조건문을 넣어서  "한명 이상이 똑같은 챌린지를 만들고, 최대 챌린지 생성 개수보다 작으면 결과에서 제외" 걸러주기
3. Hackers 테이블과 INNER JOIN하고 필요한 결과컬럼 입력하기
4. ORDER 조건 넣어주기

<br>

    SELECT h.hacker_id
        , h.name
        , c.challenges_count
    FROM Hackers AS h
    INNER JOIN (
        SELECT hacker_id, COUNT(*) AS challenges_count
        FROM Challenges
        GROUP BY hacker_id
        HAVING
            CASE
                WHEN COUNT(challenges_count) >=2 AND MAX(challenges_count) THEN challenges_count
                WHEN COUNT(challenges_count) >=2 AND challenges_count < MAX(challenges_count) THEN NULL
                ELSE challenges_count
            END
        ORDER BY challenges_count DESC
        ) challenge_num AS c ON h.hacker_id = c.hacker_id
    ORDER BY c.challenges_count DESC, h.hacker_id

<br>

#### **정답 문제풀이**

<br>

1. WITH문을 사용해 hacker_id별로 만든 challenges 개수를 구한 테이블 만들기
2. WHERE절에 서브쿼리 이용해 challenges_created가 MAX인 1번째 조건식 넣기
3. 마찬가지로 WHERE절에 서브쿼리 이용해 중복되는 challenges_create가 없는 2번째 조건식 넣기

<br>

    WITH counter AS (
    SELECT Hackers.hacker_id
        , Hackers.name
        , COUNT(*) AS challenges_created
    FROM Challenges
        INNER Hackers ON Challenges.hacker_id = Hackers.hacker_id
    GROUP BY Hackers.hacker_id, Hackers.name
    )

    SELECT counter.hacker_id
        , counter.name
        , counter.challenges_created
    FROM counter
    WHERE challenges_created = (SELECT MAX(challenges_created) FROM COUNTER)
    OR challenges_created IN (SELECT challenges_created
                            FROM counter
                            GROUP BY challenges_created
                            HAVING COUNT(*) = 1)
    ORDER BY counter.challenges_created DESC, counter.hacker_id