## **1581. Customer Who Visited but Did Not Make Any Transactions**

<br>

### **문제**

<br>

- 쇼핑몰에 방문 후 거래가 없는 고객의 ID와 방문 횟수를 출력
- Visits 테이블은 쇼핑몰에 방문한 고객의 정보를 나타냄
- Transactions 테이블은 쇼핑몰에 방문한 동안 거래된 정보를 나타냄

<br>

#### **정답 문제풀이**

<br>

- Visits 테이블과 Transactions 테이블을 LEFT JOIN하여 쇼핑몰에 방문하여 거래가 있는 고객, 없는 고객 확인
- WHERE절에 Transactions 테이블의 visit_id에 IS NULL로 조건을 설정해 거래가 없는 고객만 선택
- customer_id로 그룹핑하여 고객 id와 방문 횟수 추출

<br>

    SELECT customer_id, COUNT(customer_id) AS count_no_trans
    FROM Visits v
        LEFT JOIN Transactions t ON v.visit_id = t.visit_id
    WHERE t.visit_id IS NULL
    GROUP BY customer_id