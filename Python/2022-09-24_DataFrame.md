# **Udemy Pandas 강의**

<br>
<br>
<br>

## **Section 3. 판다스 기초**

<br>
<br>

### **판다스 화면 표시 옵션**

<br>

- 데이터프레임이 60개보다 적은 수의 행을 가지고 있다면 Pandas는 기본적으로 모든 행을 보여줌
- 60개 이상의 행을 가진다면 min_rows 설정이 우선하여 10개 행이 표시됨

- 처음 10개 행과 마지막 10개 행으로 총 20개의 행이 보고싶다면 min_rows 설정을 20으로 변경

<br>

    pd.options.display.max_rows
    pd.options.display.min_rows

<br>

- 옵션 설정을 통해서 보여지는 행을 변경할 수 있음
- 아래 코드를 통해 보여지는 행을 891개로 변경 가능

<br>

    pd.options.display.max_rows = 891

<br>
<br>

### **데이터 점검**

<br>

- info()를 통해 데이터프레임의 많은 정보를 얻을 수 있음
- 또한 결측치 등 잠재적인 문제를 식별 가능함

<br>

    titanic.info()

<br>

- 숫자열에 대한 요약통계 확인

<br>

    titanic.describe()

<br>

- include = "O"하면 자료형이 객체인 열을 선택 가능
- 결과값의 unique는 실제 고유값의 개수이고, top은 최빈값을 의미함

<br>

    titanic.describe(include = "O")
![](2022-09-24-22-54-24.png)

<br>
<br>

### **Pandas 내장함수, 속성 및 메소드**

<br>

- 데이터프레임 크기 확인

<br>

    titanic.shape

<br>

- 행 데이터 * 열 데이터

<br>

    titanic.size

<br>

- RangeIndex 확인하기
- 0부터 891까지 1칸 간격으로 인덱스가 있음을 확인

<br>

    titanic.index
![](2022-09-24-22-58-11.png)

<br>

- 데이터프레임의 모든 칼럼 보기

<br>

    titanic.columns

<br>

- method chaining
- 메소드들을 차례로 연결시켜서 결과를 볼 수 있음

<br>

    titanic.mean().sort_values().head(2)

<br>

### **열 선택하기**

<br>

#### **대괄호 [ ]를 이용한 열 선택**

<br>

- series 형태로 출력
- series는 한 개의 열 또는 한 개의 행만 보여줌
- 1차원 배열임

<br>

    titanic["age"]

<br>

- 두 개 이상의 열을 선택 시 아래와 같은 코드로 진행하면 오류가 발생함
- 오류 메시지를 확인하면 튜플로 된 열의 레이블이 없다는 뜻

<br>

    titanic["age", "sex"]
![](2022-09-24-23-03-31.png)

<br>

- 두 개 이상의 열을 선택할 때는 열 레이블을 리스트로 입력해줘야 함

<br>

    titanic[["age", "sex"]]

<br>

- 한 개의 열만 선택할 때도 리스트 처리 후 출력 가능함
- 그렇다면 한 개의 열을 출력할 때 Series가 좋은가? list가 좋은가?
- 답은 정해진 것이 없음. 대부분의 메소드를 서로 공유하기 때문임

<br>

#### **점 표기법을 이용한 한 개의 열 선택**

<br>

    titanic.age

<br>

- 한 개의 열을 선택할 때는 점 표기나 [ ] 표기나 결과가 동일함
- 아래의 equals()를 통해 확인해 볼 수 있고 결과가 True라면 동일하다는 의미

<br>

    titanic.age.equals(titanic["age"])