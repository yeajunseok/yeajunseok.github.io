---
title: "파이썬 프로그래밍_4_class"
header:
 overlay_image: /assets/images/overlayimage.jpg
categories:
  - python
tags:
  - python
use_math: true
toc: true
toc_label: "My Table of Contents"
toc_sticky: true
---
class, __init__, 상속, super, getter, setter, nonpublic, is a, has a, magic

# class

## 1. 클래스의 구조
- self : 객체 자신을 의미
- 클래스 내부의 함수를 선언할때 첫번째 파라미터는 self를 반드시 써줘야 합니다.  

```python
# 계산기 클래스
class Calculator:

    def set_data(self, num1, num2):
        self.num1 = num1
        self.num2 = num2

    def add(self):
        return self.num1 + self.num2

    def sub(self):
        return self.num1 - self.num2
```
```python
c1 = Calculator()
c1.set_data(5, 7)
c1.add()

c2 = Calculator()
c2.set_data(10, 12)
c2.add()

%whos
```
```python
# class에서 self는 객체를 의미합니다.
c3 = Calculator()
```


## 2. 생성자 함수
- 클래스가 객체로 만들어질때 초기값을 설정하는 기능으로 많이 사용됩니다.
- `__init__` 함수이름으로 함수를 선언하면 됩니다.
- 꼭 필요한 변수가 있어야 객체가 만들어 질수 있도록 해주는 기능이 있습니다.  

```python
# 계산기 클래스 : 생성자 이용
class Calculator:
    # 생성자 함수는 클래스가 객체가 될때 자동으로 실행 됩니다.
    def __init__(self, func, *args, **kwargs):
        self.total = sum(args)
        self.plus = func
        self.num1 = kwargs["num1"]
        self.num2 = kwargs["num2"]

    def setData(self, num1, num2):
        self.num1 = num1
        self.num2 = num2

    def add(self):
        return self.plus(self.num1 + self.num2)

    def sub(self):
        return self.num1 - self.num2
```
```python
c4 = Calculator(lambda *args: sum(args), 1, 2, 3, 4, 5, 6, 7, 8, num1=10, num2=100)
c4
```
```python
c4.total
---
36

c4.add()
---
110
```
### Quiz : 야구 타율
- 데이터를 클래스와 객체로 나타내세요.
- 타율을 계산하는 함수를 추가
- 객체를 만들어서 각 선수의 타율(안타/타석)을 출력해주세요.
- 타율은 소수 3째자리까지 출력 (round)  

```
python - 476타석, 176안타
data - 382타석, 120안타
fast - 422타석, 150안타
```  
```python
class Player:

    def __init__(self, bb, hit):
        self.bb = bb
        self.hit= hit

    def avg(self):
        return round(self.hit / self.bb, 3)
```  

```python
python = Player(476, 176)
data = Player(382, 120)
fast = Player(422, 150)

python.avg(), data.avg(), fast.avg()
```  

### Quiz : 성적
- 학생의 국어, 영어, 수학 점수를 입력받아서 총점과 평균을 구하는 클래스를 만드세요.
- 학생 한명이 하나의 객체
- 두가지 방법
  - 생성자 함수에서 국어, 영어, 수학를 받도록 코드 작성
  - 생성자 함수에서 과목의 갯수 제한 없이 데이터 받도록 코드 작성  


```python
class Student:

    def __init__(self, korean, english, math):
        self.korean = korean
        self.english = english
        self.math = math

    def total(self):
        return self.korean + self.english + self.math

    def avg(self):
        return round(self.total() / 3, 2)
```
```python
s1 = Student(98, 87, 78)
s1.total(), s1.avg()
```
```python
class Student:

    def __init__(self, **kwargs):
        self.datas = kwargs

    def total(self):
        return sum(self.datas.values())

    def avg(self):
        return round(self.total() / len(self.datas), 2)
```
```python
s1 = Student(korean=99, english=88)
s2 = Student(korean=90, english=85, math=98)
s3 = Student(korean=90, english=85, math=98, science=76)
s1.total(), s1.avg(), s2.total(), s2.avg(), s3.total(), s3.avg()
```
## 3. 상속
- 기존의 클래스에 새로운 변수나 함수를 추가하거나 변경(수정)할수 있도록 해주는 기능
- 부모클래스, 자식클래스
- overiding : 부모클래스가 가지고 있던 함수를 다시 정의해서 사용하는 개념
- overloading : 함수의 이름은 같으나, 아규먼트의 갯수 차이로 다른 코드를 실행시켜주는 방법

```python
# 계산기 클래스
class Calculator:
    def __init__(self, num1, num2):
        self.num1 = num1
        self.num2 = num2

    def add(self):
        return self.num1 + self.num2

    def sub(self):
        return self.num1 - self.num2
```
```python
class ImprovedCalculator(Calculator):

    def pow_func(self):
        return self.num1 ** self.num2
```
```python
ic = ImprovedCalculator(2, 3)
ic.add(), ic.sub(), ic.pow_func()
```
### Quiz : 아이폰
- 아이폰이 버전 1 ~ 3까지 업그레이드 되었습니다.
- 상속을 이용해서 아이폰 버전 1 ~ 3까지의 클래스를 작성해주세요.
- 기능들은 print(기능이름) 으로 출력이 되도록 함수내용을 작성  

```
iPhone_1 : calling
iPhone_2 : calling, send_msg
iPhone_3 : calling, send_msg, internet
```

```python
class Iphone1:
    def calling(self):
        print("calling")    
```
```python
class Iphone2(Iphone1):
    def send_msg(self):
        print("send_msg")
```
```python
class Iphone3(Iphone2):
    def internet(self):
        print("internet")
```
```python
iphone1 = Iphone1()
iphone2 = Iphone2()
iphone3 = Iphone3()
```
```python
[var for var in dir(iphone1) if var[:2] != "__"]
=> ['calling']
[var for var in dir(iphone2) if var[:2] != "__"]
=> ['calling', 'send_msg']
[var for var in dir(iphone3) if var[:2] != "__"]
=> ['calling', 'internet', 'send_msg']
```

## 4. 다중상속
- 파이썬은 다중상속이 가능합니다.
- 부모 클래스를 여러개 사용하는 방법  

```
# A -> C, B -> C (X)
# A -> B -> C (O)
class C(B, A):
    pass
```
- 오버라이딩, 오버로딩  
```python
class Human:
    def walk(self):
        print("walking")
```
```python
class Korean:
    def eat(self):
        print("kimchi")
```
```python
class Indian:
    def eat(self):
        print("curry")
```
```python
class Jin(Korean, Human):
    def skill(self):
        print("coding")

    # 오버라이딩
    def eat(self):
        print("noodle")
```
```python
jin = Jin()
jin.eat() => noodle
jin.walk() => walking
jin.skill() => coding
show_func(jin) => ['eat', 'skill', 'walk']
```
```python
class Anchal(Indian, Human):
    def skill(self):
        print("speak english")

    # 오버라이딩
    def eat(self, place=None):
        # 오버로딩
        if place is None:
            print("curry")
        else:
            print("curry in {}".format(place))
```
```python
anchal = Anchal()
anchal.eat() => curry
anchal.eat("seoul") => curry in seoul
anchal.walk() => walking
anchal.skill() => speak english
show_func(anchal) => ['eat', 'skill', 'walk']
```
## 5. super
- 부모클래스의 변수나 함수를 받아올때 사용합니다.
- 주로 부모클래스의 생성자를 다시 선언해줄때 많이 사용됩니다.
- 다이아몬드 상속시 중복해서 생성자를 상속 받는것을 방지
    -  (A -> B), (A -> C), (B,C -> D)  

```python
class A:
    def __init__(self):
        print("init A")

class B(A):
    def __init__(self):
        print("init B")
        super(B, self).__init__()

class C(A):
    def __init__(self):
        print("init C")
        super(C, self).__init__()

class D(C, B):
    def __init__(self):
        print("init D")
        super(D, self).__init__()
```
```python
d = D()
---
init D
init C
init B
init A
```
### Quiz : 스타크래프트
```python
class Human:
    def __init__(self):
        self.health = 40

    def set_health(self, var):
        self.health += var
        if self.health > 40:
            self.health = 40
        elif self.health < 0:
            self.health = 0
```
```python
class Marin(Human):

    def __init__(self, attack_pow=5, kill=0):
        super(Marin, self).__init__()
        self.attack_pow = attack_pow
        self.kill = kill

    def attack(self, obj):

        if obj.health == 0:
            print("aleady die")
            return

        obj.set_health(-self.attack_pow)
        #super(Marin, obj).set_health(-self.attack_pow)

        if obj.health <= 0:
            self.kill += 1
            print("kill")
```
```python
class Medic(Human):

    def __init__(self, heal_pow=6):
        super(Medic, self).__init__()
        self.heal_pow = heal_pow

    def heal(self, obj):

        if obj.health == 0:
            print("aleady die")
            return

        obj.set_health(self.heal_pow)
```
```python
marin1 = Marin()
marin2 = Marin()
marin1.health, marin1.kill, marin2.health, marin2.kill
---
marin1.attack(marin2)
marin1.health, marin1.kill, marin2.health, marin2.kill
```
```python
medic = Medic()
medic.heal(marin1)
marin1.health
```
## 6. getter, setter
- 객체의 내부 변수에 접근할때 특정 로직을 거쳐서 접근할수 있도록 하는 방법
- python에서는 두가지 방법으로 구현 가능
  - property
  - decolator  

```python
#property
class Person1:
    def __init__(self, input_name):
        self.hidden_name = input_name.upper()

    def getter(self):
        print("getter")
        return self.hidden_name.split(" ")[0] + " " + self.hidden_name.split(" ")[1].lower()

    def setter(self, input_name):
        print("setter")
        self.hidden_name = input_name.upper()

    name = property(getter, setter)
```
```python
person = Person1("park python")
person.name                    => 'PARK python'
person.name ="lee data"        =>  setter
person.name                    => 'LEE data'
person.hidden_name             => 'LEE DATA'
person.hidden_name ="kim macbook"
person.hidden_name             => 'kim macbook'
```
```python
#decorlator
class Person2:
    def __init__(self, input_name):
        self.hidden_name = input_name.upper()

    @property
    def name(self):
        print("getter")
        return self.hidden_name.split(" ")[0] + " " + self.hidden_name.split(" ")[1].lower()

    @name.setter
    def name(self, input_name):
        print("setter")
        self.hidden_name = input_name.upper()
```
```python
person = Person2("park python")
person.name                    => 'PARK python'
person.name = "lee data"       =>  setter
person.name                    => 'LEE data'
person.hidden_name = "kim macbook"
person.hidden_name             => 'kim macbook'
```
## 7. non public
- 파이썬에서는 private 기능을 magling 이라는 방법을 사용하여 구현
- class의 내부 변수에 다이렉트로 접근 불가능하도록 해줍니다.
- 사용방법은 앞에 `__`를 붙여서 변수를 선언해줍니다.
- 완벽한 방법은 아니고 `(객체이름)._(클래스명)(변수명)`으로 접근이 가능합니다.  


## 8. 클래스의 메서드의 종류
## 9. is a / has a
## 10. magic method
