---
title: "파이썬 프로그래밍_10_Pandas_DataFrame"
header:
 overlay_image: /assets/images/overlayimage.jpg
categories:
  - Numpy & Pandas
tags:
  - Numpy & Pandas
use_math: true
toc: true
toc_label: "My Table of Contents"
toc_sticky: true
---
DataFrame

## DataFrame
### 1. create
```python
# 컬럼 설정
pd.DataFrame(columns=["Email","Name"])
# 데이터 삽입
df["Name"] = ["fcamp","dss"] #Name컬럼에 fcamp, dss 추가하가
df["Email"] = ["yea@gmail.com", "jjj@daum.net"]
---
# 하나의 컬럼의 type은 Series 이다.
type(df["Email"])
=> pandas.core.series.Series
---
# 딕셔너리 DataFrame만들기
dic = {
    "Name": ["fcamp", "dss"],
    "email": ["pdj@gmail.com", "dss@daum.net"],
    "id": [1, 2],
}
pd.DataFrame(dic)
#리스트로 DataFrame 만들기
ls = [
    {"Name": "fcamp", "email": "pdj@gmail.com", "id": 1},
    {"Name": "dss", "email": "dss@daum.net", "id": 2},
]
pd.DataFrame(ls)
---
# DataFrame만들때 index 값도 같이 만들어 주기
idx = ["one", "two"]
pd.DataFrame(ls, index=idx)

#데이터 갖고 오는 방법들
df.index
=> Index(['one', 'two'], dtype='object')
df.columns
=> Index(['Name', 'email', 'id'], dtype='object')
df.values
=> array([['fcamp', 'pdj@gmail.com', 1],
          ['dss', 'dss@daum.net', 2]], dtype=object)

# 컴럼별 데이터 타입 확인하기
df.dtypes
=> Name     object
   email    object
   id        int64
   dtype: object
```
### 2. insert
- row 추가
- colunm 추가  

#### row 데이터 추가하기
```python
df.loc[2] = {"email":"ee@gmail.com", "Name":"dad", "id":3}

  Name	email	         id
0	fcamp	pdj@gmail.com	  1
1	dss	  dss@daum.net  	2
2	data	data@gmail.com	3

#빠르게 추가하기
len(df)
df.loc[len(df)] = {"email":"dae@gamil.com", "Name":"e32", "id":4}

  Name	email	           id
0	fcamp	pdj@gmail.com	    1
1	dss	  dss@daum.net	    2
2	data	data@gmail.com	  3
3	data2	data2@gmail.com	  4
```
#### row 데이터 보기 & offset index
```python
df.loc[1]
=> Name              dss
  email    dss@daum.net
  id                  2
  Name: 1, dtype: objec

df.loc[1:2] #index 1에서

df.loc[::-1] #반다로 출력

df.loc[1:2, ["email","id"]] #index 1에서 2까지 그리고 column "email","id" 함께 출력
=>
  email	          id
1	dss@daum.net	   2
2	data@gmail.com	 3  

df.loc[[1,3], ["email","id"]] #index 1과 3 그리고 column "email","id" 함께 출력
=>
  email	          id
1	dss@daum.net	   2
3	data2@gmail.com	 4
```

#### 컬럼 데이터 추가하기
```python
df["Address"] = ["Seoul", "Busan", "Jeju", "Deagu"]
=>
  Name	email	         id	Address
0	fcamp	pdj@gmail.com	  1	Seoul
1	dss	  dss@daum.net	  2	Busan
2	data	data@gmail.com	3	Jeju
3	data2	data2@gmail.com	4	Deagu

df[["Address", "email"]]
=>
  Address	email
0	Seoul	  pdj@gmail.com
1	Busan	  dss@daum.net
2	Jeju  	data@gmail.com
3	Deagu	  data2@gmail.com
```

#### index 수정
```python
pd.DataFrame(data=df.values, columns=df.columns, index=list("ABCD"))
=>
  Name	email	         id	 Address
A	fcamp	pdj@gmail.com	  1	 Seoul
B	dss	  dss@daum.net	  2	 Busan
C	data	data@gmail.com	3	 Jeju
D	data2	data2@gmail.com	4 Deagu

df.index = list("QWER")
=>
  Name	email          	id	Address
Q	fcamp	pdj@gmail.com	  1 	Seoul
W	dss	  dss@daum.net	  2	  Busan
E	data	data@gmail.com	3	  Jeju
R	data2	data2@gmail.com	4	  Deagu

df.index = range(4)
=>
  Name	email	         id	 Address
0	fcamp	pdj@gmail.com	  1	 Seoul
1	dss	  dss@daum.net	  2	 Busan
2	data	data@gmail.com	3	 Jeju
3	data2	data2@gmail.com	4	 Deagu
```
### 3. apply
- 모든 컬럼 요소에 함수를 적용해서 결과를 저장하는 방법
- function의 map과 유사  
```python
df["Address"] = df["Address"].apply(
  lambda addr: "{}({})".format(addr, len(addr)) )

df["Domain"] = df["email"].apply(
  lambda data: re.findall("[\w]+@([0-9a-z]+)[.][0-9a-z]+", data)[0])

s = "pdj@gmail.com"
p = "[\w]+@([0-9a-z]+)[.][0-9a-z]+"
re.findall(p, s)[0]
=> 'gmail'
```
### 4. append
- 데이터 프레임을 합치고 싶을때
```python
df3 = df1.append(df2) #기본적으로 세로로 합쳐짐
df1.append(df2, ignore_index = True)
```
#### reset_index
```python
df3.reset_index()
=>
      index	Age	Name
0	    0	    26	Alan
1	    1	    33	Alex
2	    2	    33	Jin
3	    3	    40	Science
4	    4	    32	Jin
5	    0	    29	Data
6	    1	    25	Jin
7	    2	    28	Alvin
8	    3	    29	Alan
9	    4	    39 	Fast
df3.reset_index(drop=True, inplace=True)
=>
  Age	Name
0	26	Alan
1	33	Alex
2	33	Jin
3	40	Science
4	32	Jin
5	29	Data
6	25	Jin
7	28	Alvin
8	29	Alan
9	39	Fast
df3["Age"].reset_index()
df3["Age"]
#둘의 차이점은 .reset_index()하면 DataFrame으로 만들어 준다.
```
### 5. concat
- row, column으로 데이터 프레임을 합치는 함수  

```python
pd.concat([df1, df2]).reset_index(drop=True) #세로로 합치기
pd.concat([df3, df1], axis=1) #가로로 합치
pd.concat([df3, df1], axis=1, join='inner') # nan 부분 제외
```  

### 6. groupby, aggregate
- 특정 컬럼의 중복되는 row를 합쳐서 새로운 데이터 프레임을 만드는 방법
- size : 데이터의 갯수를 출력
- sort_values : 정렬
- agg : min, max, mean등 기능을 사용할수 있는 함수  

```python
df.groupby("Name").size() #Name 안의 값들의 갯수를 알려준다.
=>
Name
Adam      2
Alan      1
Alex      2
Andrew    1
Billy     1
Data      2
Jin       1
dtype: int64

df.groupby("Name").size().reset_index(name = "counts")
=>
  Name	counts
0	Adam	  2
1	Alan	  1
2	Alex	  2
3	Andrew	1
4	Billy	  1
5	Data	  2
6	Jin	    1

#정렬
df.sort_values(by=["counts", "Name"], ascending=False)
#agg 함수
df.groupby("Name").agg("sum").reset_index()
=>
  Name	  Age
0	Adam	  77
1	Alan	  28
2	Alex	  41
3	Andrew	22
4	Billy	  28
5	Data	  64
6	Jin	    36
df.groupby("Name").agg(["min","max","mean"]).reset_index()
=>
  Name	           Age
         min	max	mean
0	Adam	  38	39	38.5
1	Alan	  28	28	28.0
2	Alex	  20	21	20.5
3	Andrew	22	22	22.0
4	Billy	  28	28	28.0
5	Data	  30	34	32.0
6	Jin	    36	36	36.0
```  

### 7. select  

```python
  Name	         Age
       min	max	mean
0	Adam	38	39	38.5
1	Alan	28	28	28.0
2	Alex	20	21	20.5

df.loc[2]["Age"]["max"]
=> 21
```  

### 8. merge
- 두개 이상의 데이터 프레임을 합쳐서 하나의 데이터 프레임으로 만드는 방법

```python
money_df.merge(user_df, left_on="ID", right_on="UserID")
pd.merge(money_df, user_df)
```

```python
df.["ID"].unique()
df.rename(columns={"UserID":"ID"})
```

|![ads1](/assets/images/test.jpg)|
