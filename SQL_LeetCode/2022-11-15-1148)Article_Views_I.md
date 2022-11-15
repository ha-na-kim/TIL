## **1148. Article Views I**

<br>

### **문제**

<br>

- 적어도 한 번 이상은 본인 아티클을 본 작가 출력
- id로 오름차순 정렬

<br>

#### **정답 문제풀이**

- author_id와 viewer_id가 같을 때 작가가 본인 아티클을 읽었다고 보기 때문에 WHERE 절에 해당 조건 추가
- SELECT절에 DISTINCT를 추가하여 중복 id 제거

<br>

    SELECT DISTINCT author_id AS id
    FROM Views
    WHERE author_id = viewer_id
    ORDER BY author_id