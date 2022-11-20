## **1484. Group Sold Products By The Date**

<br>

### **문제**

<br>

- 날짜별 판매 개수와 제품명 출력하기
- 날짜별 판매된 제품 이름은 사전순으로 정렬

<br>

#### **문제풀이**

<br>

- 날짜별 판매 제품과 개수를 보기 위해 날짜별로 그룹핑
- 몇 개의 제품이 팔렸는지 확인하기 위해 중복제품을 제외하고 COUNT
- 날짜별 판매된 제품을 나열하기 위해 GROUP_CONCAT 사용

<br>

    SELECT sell_date
        , COUNT(DISTINCT product) AS num_sold
        , GROUP_CONCAT(DISTINCT product ORDER BY product SEPARATOR ',') AS products
    FROM Activities
    GROUP BY sell_date
    ORDER BY sell_date