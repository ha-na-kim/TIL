## **Ollivander's Inventory**

<br>
<br>

### **문제**

<br>

- 파워와 나이가 높은, 악하지 않은 지팡이를 하나 사는데 필요한 최소한의 금 갤리온 수를 결정
- id, age, coins_needed, power 순으로 출력하기
- power를 내림차순으로 정렬
- 둘 이상의 지팡이에 동일한 power가 있는 경우, 나이를 내림차순으로 정렬
- 둘 이상의 지팡이에 동일한 power가 있고, 동일한 나이가 2개 이상일 경우, coins_needed가 작은 지팡이 선택
- is_evil = 0은 지팡이가 악하지 않다는 뜻으로 is_evil이 0인 지팡이만 선택

<br>

#### **오답 문제풀이**

<br>

- Wand 테이블과 Wand_Property 테이블을 INNER JOIN하여 정보를 합치고 is_evil=0인 경우만 선택
- 위의 조건에 맞게 power, age 내림차순으로 정렬하고 FROM절에 서브쿼리로 넣어줌
- Window 함수를 이용해 나이가 동일할 경우 coins_needed가 적은 것을 선택하도록 하여 출력

<br>

    SELECT MIN(coins_needed) OVER (PARTITION BY age, ORDER BY power)
    FROM (
        SELECT w.id
            , wp.age
            , w.coins_needed
            , w.power
        FROM Wands w
            INNER JOIN Wands_Property wp ON w.code = wp.code
        WHERE is_evil = 0
        ORDER BY w.power DESC, wp.age DESC
        ) wand

<br>

#### **정답 문제풀이**

<br>

- 위의 쿼리에서 Window 함수를 From절의 테이블 안으로 옮긺
- power, age로 파티션을 나누고 coins_needed로 오름차순 정렬하여 순위를 부여함
- WHERE절에 순위가 1인 (같은 age일 때 coins_needed가 작은) 데이터만 선택하여 출력

<br>

    SELECT id
        , age
        , coins_needed
        , power
    FROM (
        SELECT w.id
            , wp.age
            , w.coins_needed
            , w.power
            , ROW_NUMBER() OVER (PARTITION BY w.power, wp.age ORDER BY w.coins_needed) AS row_num
        FROM Wands w
            INNER JOIN Wands_Property wp ON w.code = wp.code
        WHERE wp.is_evil = 0
        ) wand
    WHERE row_num = 1
    ORDER BY power DESC, age DESC