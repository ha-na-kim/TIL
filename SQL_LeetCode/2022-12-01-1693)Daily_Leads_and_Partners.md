## **1693. Daily Leads and Partners**

<br>

### **문제**

<br>

- DailySales 테이블에는 판매된 제품의 날짜, 이름, 판매된 lead_id, partner_id가 있음
- 각 날짜, 이름별로 고유한 lead_id, partner_id의 수를 구하기

<br>

#### **오답 문제풀이**

<br>

- date_id를 DISTINCT하여 날짜별로 lead_id, partner_id를 구할 수 있도록 하고
- WINDOW 함수 중 COUNT()에 DISTINCT를 사용하여 고유한 id값들을 출력하도록 쿼리 작성
- 그러나 WINDOW 함수에서는 DISTINCT를 사용할 수 없다는 오류 발생

<br>

    SELECT DISTINCT date_id
        , make_name
        , COUNT(DISTINCT lead_id) OVER (PARTITION BY date_id, make_name) AS unique_leads
        , COUNT(DISTINCT partner_id) OVER (PARTITION BY date_id, make_name) AS unique_partners
    FROM DailySales

<br>

#### **정답 문제풀이**

<br>

- WINDOW 함수를 사용하지 않고 date_id, make_name으로 그룹핑 후 COUNT에 DISTINCT를 사용하여 쿼리를 작성함

<br>

    SELECT DISTINCT date_id
        , make_name
        , COUNT(DISTINCT lead_id) AS unique_leads
        , COUNT(DISTINCT partner_id) AS unique_partners
    FROM DailySales
    GROUP BY date_id, make_name