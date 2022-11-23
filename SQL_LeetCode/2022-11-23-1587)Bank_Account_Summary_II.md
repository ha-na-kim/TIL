## **1587. Bank Account Summary II**

<br>

### **문제**

<br>

- 잔액이 10000보다 큰 사용자의 이름과 잔액 출력하기
- Users 테이블은 은행에 있는 사용자의 계정 정보를 나타내고, 동일한 이름을 가진 두 명의 사용자는 없음
- Transactions 테이블은 모든 계정에 대한 변경사항을 나타냄

<br>

#### **문제풀이**

<br>

- 1. Transactions 테이블에서 account로 그룹핑, 잔액이 10000 이상인 계정과 잔액 출력
- 2. 1번 테이블을 FROM절의 서브쿼리에 넣고 Users 테이블과 account 기준으로 INNER JOIN하여 이름과 잔액 출력

<br>

    SELECT name, balance
    FROM (
        SELECT account, SUM(amount) AS balance
        FROM Transactions
        GROUP BY account
        HAVING SUM(amount) > 10000
    ) t INNER JOIN Users u ON t.account = u.account