## **180. Consecutive Numbers**

<br>
<br>

### **세번 연속으로 동일한 숫자가 나오는 값 출력하기**

<br>

#### **오답 문제풀이**

<br>

- 연속하는 숫자를 옆으로 붙게 하기 위해 셀프조인 사용
- WHERE절에서 셀프조인으로 붙인 세 값들이 모두 같은지 확인

<br>

    SELECT l.num AS ConsecutiveNums
    FROM Logs AS l
        INNER JOIN Logs AS l_2 ON l.id+1 = l_2.id
        INNER JOIN Logs AS l_3 ON l.id+2 = l_3.id
    WHERE l.num = l_2.num
    AND l_2.num = l_3.num
    AND l.num = l_3.num

<br>

#### **정답 문제풀이**

<br>

- 위 코드로 리트코드의 Run Code 시 옳은 정답이라고 나왔으나 실제 Submit에서 연속되는 값이 2번 출력되는 형태로 제출되었다
- 예를 들어 3이 연속으로 3번 나왔다면 결과값이 "3"이어야 하는데 "3,3"이 나온 것
- SELECT절에 l.num 하나만 작성했는데 왜 값이 두 번 나왔는지 모르겠으나 정답처리를 위해 DISTINCT 추가

<br>

    SELECT DISTINCT l.num AS ConsecutiveNums
    FROM Logs AS l
        INNER JOIN Logs AS l_2 ON l.id+1 = l_2.id
        INNER JOIN Logs AS l_3 ON l.id+2 = l_3.id
    WHERE l.num = l_2.num
    AND l_2.num = l_3.num
    AND l.num = l_3.num