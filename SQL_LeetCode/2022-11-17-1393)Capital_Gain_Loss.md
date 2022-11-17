## **1393. Capital Gain/Loss**

<br>

### **문제**

<br>

- 주식에 대한 자본 손익 출력하기
- 주식을 한 번 또는 여러 번 사고 판 후의 최종 손익 구하기

<br>

#### **정답 문제풀이**

<br>

1. stock_name과 operation(sell, buy)별로 그룹핑하여 총 금액 구하기
2. 1번 쿼리를 FROM절에 서브쿼리로 넣기
3. Window함수 중 LEAD()를 이용해 buy일 때 금액을 열로 추가해주기
4. 3번 쿼리를 FROM절에 서브쿼리로 넣기
5. stock_name, sell price - buy price하여 최종 손익을 출력하기
6. WHERE절에 sell price - buy price가 NULL값인 것은 제외하기

<br>

    SELECT stock_name, price-buy_price AS capital_gain_loss
    FROM (
        SELECT *
            , LEAD(price, 1) OVER (PARTITION BY stock_name) AS buy_price
        FROM (
            SELECT stock_name, operation, SUM(price) AS price
            FROM Stocks
            GROUP BY stock_name, operation
            ORDER BY 1
        ) stock_sum_price
    ) stock_lead
    WHERE price-buy_price IS NOT NULL