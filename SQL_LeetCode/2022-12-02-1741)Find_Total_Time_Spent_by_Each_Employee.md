## **1741. Find Total Time Spent by Each Employee**

<br>

### **문제**

<br>

- Employees 테이블은 직원들의 출퇴근 시간을 나타냄
- 각 직원들이 사무실에서 보낸 시간 계산하기
- 직원은 하루에 두 번 이상 사무실을 출입할 수 있음

<br>

#### **문제풀이**

<br>

- event_day, emp_id를 그룹핑하여 날짜별로 직원이 사무실에 들어간 시간, 나간 시간을 합하여 WITH문으로 테이블 생성
- 만들어놓은 time Table로 날짜, 직원 id, total_time을 출력

<br>

    WITH time AS (
        SELECT event_day
            , emp_id
            , SUM(in_time) AS in_time
            , SUM(out_time) AS out_time
        FROM Employees
        GROUP BY event_day, emp_id
    )
    SELECT event_day AS day
        , emp_id
        , out_time - in_time AS total_time
    FROM time