## **1667. Fix Names in a Table**

<br>

### **문제**

<br>

- 이름의 첫글자는 대문자, 나머지는 소문자로 수정하기
- user_id를 기준으로 오름차순 정렬하기

<br>

#### **문제풀이**

<br>

- LEFT 함수를 이용해 첫글자만 가져와 UPPER 함수로 대문자 변경
- SUBSTR 함수를 이용해 나머지 글자를 가져와 LOWER 함수로 소문자 변경
- CONCAT 함수를 이용해 글자를 합치기

<br>

    SELECT user_id
        , CONCAT(UPPER(LEFT(name, 1)), LOWER(SUBSTR(name, 2))) AS name
    FROM Users
    ORDER BY 1