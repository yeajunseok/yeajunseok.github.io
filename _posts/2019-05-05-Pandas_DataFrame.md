---
title: "파이썬 프로그래밍_7_Pandas_DataFrame"
header:
 overlay_image: /assets/images/overlayimage.jpg
categories:
  - Numpy_Pandas
tags:
  - Numpy_Pandas
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

##### row 데이터 추가하기
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
##### row 데이터 보기 & offset index
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

##### 컬럼 데이터 추가하기
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

##### index 수정
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
### 4. append
### 5. concat
### 6. groupby, aggregate
### 7. select
### 8. merge
