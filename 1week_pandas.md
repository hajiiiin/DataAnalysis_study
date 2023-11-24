## Pandas

- numpy를 기반으로 만들어짐
- 외부 데이터 읽고 쓰기
- 정리된 데이터 새로운 파일에 저장
- 데이터 시각
- numpy에 비해 표 형식의 데이터를 다루는 것에 능하다.

→ 대부분의 데이터는 2차원이다 (표 형식이다)

### DataFrame

: 표 형식의 자료형을 담는 자료형

다양한 자료형을 담을 수 있

(세로)열 : column (데이터의 특징)

(가로)행 : row (레코드)

- 데이터 프레임 만들기

```python
import pandas as pd

two_dimensional_list = [['a', 1, 2], ['b', 3, 4], ['c', 5, 6]]
my_df = pd.DataFrame(two_dimensional_list)
```

|  | 0 | 1 | 2 |
| --- | --- | --- | --- |
| 0 | a | 1 | 2 |
| 1 | b | 3 | 4 |
| 2 | c | 5 | 6 |

type ⇒ pandas의 DataFrame

- column, row명 지정하기

```python
my_df = pd.DataFrame(two_dimensional_list, columns=['1','2','3','4'], index=['A','B','C','D'])
```

columns와 index로 지정가능 

⇒ my_df.columns , my_df.index로 출력 가

- dictionary로 dataFrame 만들기

```python
import pandas as pd

names = ['dongwook', 'sineui', 'ikjoong', 'yoonsoo']
english_scores = [50, 89, 68, 88]
math_scores = [86, 31, 91, 75]

dict1 = {
    'name': names, 
    'english_score': english_scores, 
    'math_score': math_scores
}
```

### Pandas의 dtype (데이터 타입)

- int64 : 정수
- float64 : 소수
- object : 텍스트
- detetime65 : 날짜와 시간
- bool : 불린(참, 거짓)
- category : 카테고리

### CSV 파일 읽기

```python
pd.read_csv('파일')
```

보통 첫줄을 header로 읽음, 만약 헤더가 없는 파일일 때sms

```python
pd.read_csv('파일', headr=None)

pd.read_csv('파일', index_col=0) # 각 행의 첫 요소를 row index로 지
```

- 원하는 행 정보 가져오기

```python
file_name.loc['row명', 'col명'] # 해당 행의 열 정보를 가져옴
file_name.loc['row명', :] # 해당 행을 가져옴
file_name.loc['row명']  # 해당 행을 가져옴
```

⇒ 이렇게 가져온 행의 type은 Series : 판다스의 1차원 배열

- 원하는 열 정보 가져오기

```python
file_name.loc[: , 'col명'] 
file_name.loc['col명'] 

# 여러 열 가져오기
file_name.loc[:, ['col1','col2']] # ⭐행자리를 :로 채워주어야한다.
```

*배열로 묶어주면 여러 정보를 가져올 수 있다.*

- 데이터프레임 여러개 합치기

```python
import pandas as pd

samsong_df = pd.read_csv('data/samsong.csv')
hyundee_df = pd.read_csv('data/hyundee.csv')

combined_df = pd.DataFrame({
    'day': samsong_df['요일'], 
    'samsong': samsong_df['문화생활비'], 
    'hyundee': hyundee_df['문화생활비']
})
combined_df
```

각 데이터 프레임에서 원하는 열 정보를 뽑아낸 뒤, 파이썬 dictionary를 사용하여 DataFrame을 만들어 주었다.

![결과](https://prod-files-secure.s3.us-west-2.amazonaws.com/177efddb-3aa9-4012-b699-da68d5d3319c/10351b23-4ad5-4a98-8e27-b7a445517760/Untitled.png)

결과

*다른 방법*

```python
import pandas as pd

samsong_df = pd.read_csv('data/samsong.csv')
hyundee_df = pd.read_csv('data/hyundee.csv')

# 여기에 코드를 작성하세요
merge_df = pd.merge(samsong_df, hyundee_df, on='요일')

# 병합된 데이터프레임에서 중복되는 '문화생활비' 열 이름을 변경
merge_df.rename(columns={'요일':'day','문화생활비_x': 'samsong','문화생활비_y':'hyundee'}, inplace=True)

df = merge_df[['day','samsong','hyundee']]
```

위는 merge하고 rename해주는 방식을 사용하였다.

### 인덱싱

1. 행이나 열의 이름으로 인덱싱 : loc
    
    ```python
    df.loc['행':'행', '열':'열']
    
    df.loc[[True, False, True, False]]
    # True인 인덱스 행만 출력 , 만약 행이 7개라면 뒤의 3개는 False로 처리
    
    df.loc[:,[True, False, True]] # 열도 동일 앞에 :로 행 처리만 해주면 
    ```
    
    - 조건문으로 인덱싱도 가능
        
        ```python
        test_df.loc[test_df['열이름'] > 5] # 조건을 만족하는 행을 출력
        test_df['열이름'] > 5  # 조건을 만족하는지 true, false로 출력
        ```
        

1. 행이나 열의 위치로 인덱싱 (0,1,2 숫자로 인덱싱) : iloc
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/177efddb-3aa9-4012-b699-da68d5d3319c/caa66ea2-1c10-42be-84b6-e0778c85d439/Untitled.png)
    
    ```python
    iphone_df.iloc[[1,3],[1,4]]
    #*행은 1과 3 행을 선택, 열은 1과 4 열을 선택한다는 의미이다.*
    
    iphone_df.iloc[3:, 1:4]
    # 연속된 인덱싱 3행~ 끝까지, 1열~ 3열까지 인덱싱한다는 의미이다.
    ```
    

### 값 수

- 새로운 row 추가하기

```python
df_file['row명'] = ['col값']
# 모든 col의 값을 입력해주어야한다.
```

- 새로운 col 추가

```python
df_file['col명'] = '값'
df_file[:, 'col명'] = '값'
```

- row 삭제하기

```python
df_file.drop('row명', axis='index', inplace=False)
```

⇒  inplace=False 이렇게 하면 기존 df_file이 변경되는 것이 아닌 해당 row를 삭제한 값을 보여주기만 함.

inplace=True를 하게되면 df_file에도 반영이 되어 해당 row가 삭제된다.

- column 삭제

```python
df_file.drop('col명', axis='column', inplace=False)
```

- column명 변경

```python
df_file.rename(columns={'원래이름' : '새로운 이름'})
# rename은 기본적으로 inplace=False기 때문에 
# df_file 자체를 변경하고싶다면 inplace=True를 해주어야
```

- index 변경

```python
# index 대표 이름 바꾸기
df_file.index.name = '새로운 이름'

# index 요소의 이름 바꾸기 -> 즉 index로 설정할 col 바꾸기
df_file.set_index('col 명')
# 단, 이렇게 하면 원래의 index열은 사라짐 - 따로 저장한 후 열로 추가해주기
# set_name도 inplace=False가 기본

# 결론
df_file['index열'] = df_file.index # index를 새로운 열로 추가함
df_file.set_name('새로운 col', inplace=True) # 기존 index가 새로운 col로 변경됨
```

index는 겹치지 않는 값으로 설정하는 것이 좋다. 인덱스는 요소 간의 구별이 가능한 이름을 부여해주는 것임

### 데이터 프레임 정보 확인

- 데이터 프레임 몇 개만 출력

```kotlin
df_file.head(개수) //상위 몇개

df_file.tail(개수) //하위 몇
```

- 정렬

```kotlin
df_file.sort_value(by='기준', ascending=False)
// inplace = False 가 기본 df자체를 바꾸려면 True로 설정하
```

- 정보 확인

```kotlin
df_file.shape // size
df_file.columns // columns 종류
df_file.info() // columns의 정보 (값의 개수, 타입 등)
df_file.describe() // 통계 정보
```

### 큰 series 살펴보기

```kotlin
df_file['col'].unique() //중복 제거한 값
df_file['col'].value_counts() //값이 몇번 나오는지
df_file['col'].describe() //series에 대한 통계 정보
```
