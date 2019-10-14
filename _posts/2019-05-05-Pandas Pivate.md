---
title: "파이썬 프로그래밍_9_Pandas_Pivot"
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
Pivot

## Pivot
- index, value, column 데이터를 데이터 프레임의 컬럼 데이터에서 선택해서 만드는 방법  

### 1. groupby, pivot 사용
```python
titanic_df1 = titanic.groupby(["Sex", "Pclass"]).size().reset_index(name="counts")
=>
Sex	Pclass	Counts
0	female	1	94
1	female	2	76
2	female	3	144
3	male	1	122
4	male	2	108
5	male	3	347

titanic_df2 = titanic_df1.pivot("Sex", "Pclass", "counts")
=>
Pclass	1	2	3
Sex			
female	94	76	144
male	122	108	347
```

```python
titanic_df1 = titanic.groupby(["Sex","Survived"]).size().reset_index(name="counts")
=>
Sex	Survived	Counts
0	female	0	81
1	female	1	233
2	male	0	468
3	male	1	109

titanic_df2 = titanic_df1.pivot("Sex","Survived", "counts")
=>
Survived	0	1
Sex		
female	81	233
male	468	109
```

### 2. pivot_table 사용
```python
titanic["Counts"] = 1 #Count 컬럼 만들어서 전부 1 넣기
titanic.pivot_table("Counts", "Survived", "Sex", aggfunc=np.sum)
titanic.pivot_table("Counts", ["Sex", "Pclass"], "Survived", aggfunc=np.sum)
=>
Survived	0	1
   Sex	Pclass		
female	1	3	91
        2	6	70
        3	72	72
male	  1	77	45
        2	91	17
        3	300	47

df["total"] = df["female"] + df["male"]
=>
Sex	     female	male	total
Survived			
       0	81	468	549
       1	233	109	342

df.loc["total"] = df.loc[0] + df.loc[1]
=>
     Sex	female	male	total
Survived			
       0	81	468	549
       1	233	109	342
   total	314	577	891

# 로우 데이터 삭제
df.drop("total", inplace=True)
=>
     Sex	female	male	total
Survived			
       0	81	468	549
       1	233	109	342

# 컬럼 데이터 삭제
df.drop("total", inplace=True)
=>
     Sex	female	male
Survived		
       0	81	468
       1	233	109

titanic.pivot_table("Count", "Survived", ["parch", "Pclass"], aggfunc=np.sum)
titanic.pivot_table("Count", "Survived", ["parch", "Pclass"], aggfunc=np.sum, fill_value=0) #NaN데이터 0으로 채우기
titanic.pivot_table("Count", "Survived", ["parch", "Pclass"], aggfunc=np.sum, dropna=False, fill_value=0) #없는 값도 모두 표시하
```

### 데이터 처리
- 불필요한 feature 데이터 제거
- nan 데이터가 있는 레코드 제거
- 원핫 인코딩
- 연령대 컬럼 만들기
- 20대 이산은 성인이라는 컬럼을 만들어 0, 1을 추가  

```python
#필요한 컬럼데이터만 필터링
titanic[['Survived', 'Pclass', 'Sex', 'Age', 'SibSp', 'Parch', 'Fare', 'Embarked']]
#필요없는 컬럼 데이터 제거 : drop
titanic.drop(columns=["PassengerId", "Name", "Ticket", "Cabin", "Count"])
#nan 데이터가 있는 레코드 제거
df[df.notna().all(axis=1)].reset_index(drop=True)
=>
Survived	Pclass	Sex	Age	SibSp	Parch	Fare	Embarked
710	1	1	male	26.0	0	0	30.00	C
711	0	3	male	32.0	0	0	7.75	Q

result["Sex"].unique()
=> array(['male', 'female'], dtype=object)


```
