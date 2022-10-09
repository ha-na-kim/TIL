## **262. Trips and Users**

<br>
<br>

### **문제**

<br>

- 2013-10-01 and 2013-10-03 사이에 취소된 사용자가 있는 요청의 취소율을 찾기
- 취소율은 소수점 둘째 자리까지 반올림

<br>

#### **오답 문제풀이 1**

<br>

- Trips 테이블에서 client_id, driver_id가 banned 되지 않은 id만 선택되도록 조건 설정
- 날짜는 2013-10-01 - 2013-10-03 사이로 조건 설정
- 취소된 요청 수 / banned하지 않은 사용자로 취소율 구하기

<br>

    SELECT request_at AS Day
        , ROUND(COUNT(status = 'cancelled_by_driver') / COUNT(*), 2) AS "Cancellation Rate"
    FROM Trips
    WHERE client_id IN (SELECT users_id FROM Users WHERE banned = 'No')
    AND driver_id IN (SELECT users_id FROM Users WHERE banned = 'No')
    AND request_at BETWEEN '2013-10-01' AND '2013-10-03'

<br>

#### **오답 문제풀이 2**

<br>

- 취소율을 구하는 과정에서 분자의 COUNT 안에 IF문을 넣어 취소된 경우, 1을 부여하고 COUNT할 수 있도록 변경
- 날짜로 GROUP BY하여 일별로 집계되도록 변경

<br>

    SELECT request_at AS Day
        , ROUND(COUNT(IF(status="cancelled_by_driver", 1, 0)) / COUNT(*), 2) AS "Cancellation Rate"
    FROM Trips
    WHERE client_id IN (SELECT users_id FROM Users WHERE banned = 'No')
    AND driver_id IN (SELECT users_id FROM Users WHERE banned = 'No')
    AND request_at BETWEEN '2013-10-01' AND '2013-10-03'
    GROUP BY request_at

<br>

#### **정답 문제풀이**

<br>

- 취소율 구하는 분자의 IF문 안을 COUNT가 아닌 SUM으로 변경
- 코드 실행 시, 취소율이 항상 1이 나왔는데 COUNT를 하면 1, 0을 모두 COUNT해서 1이 나왔던 것 같음
- IF문 안에 "cancelled_by_driver" 뿐만 아니라 "cancelled_by_client" 조건 추가

<br>

    SELECT request_at AS Day
        , ROUND(SUM(IF(status = 'cancelled_by_driver' OR status = 'cancelled_by_client', 1,0)) / COUNT(*),2) AS "Cancellation Rate"
    FROM Trips
    WHERE client_id IN (SELECT users_id FROM Users WHERE banned = 'No')
    AND driver_id IN (SELECT users_id FROM Users WHERE banned = 'No')
    AND request_at BETWEEN '2013-10-01' AND '2013-10-03'
    GROUP BY request_at