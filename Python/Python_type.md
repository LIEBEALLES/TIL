# 📚Python_type

#### **Install**

- 파이썬 3.8.5 설치 / path 체크박스 무조건 체크할 것

- appData라는 숨김폴더에 저장이 될 것

#### 특징

- interactive Shell 제공 

- REPL : Read Eval Print Loop(대화형 프로그래밍 환경)

- body {} 대신 **들여쓰기**(2space or 4space or 1tab)
- 플랫폼에 독립적 
- 스크립트언어 : 컴파일하지않고 인터프리터가 코드를 직접 한 줄 씩 실행
- 변수 함수 모두가 객체 

#### 파이썬 주석  '#'

---

#### type of Python

> |  데이터 타입  |                       |
> | :-----------: | :-------------------: |
> |  str(string)  |        문자열         |
> | bool(boolean) | 참 거짓(true / false) |
> |     float     |         실수          |
> |      int      |         정수          |
> |  dictionary   |       딕셔너리        |
> |     tuple     |         집합          |
> |     list      |     tuple과 비슷      |
>
> 

#### 데이터 타입을 확인할 수 있는 방법 

> ```python
> type()
> 
> #파이썬내에 미리 만들어져있는 builtin-function이 있는데 그 중 하나가 type()
> ```

> ```python
> print(type(123)) 
> print(type('123'))
> print(type('yujeong'))
> print(type(12.34))
> print(type([5,6,7,8]))
> print(type({9,10,11,12}))
> print(type((13,14,15)))
> 
> 
> #실행결과
> <class 'int'>
> <class 'str'>
> <class 'str'>
> <class 'float'>
> <class 'list'>
> <class 'set'>
> <class 'tuple'>
> ```

#### python 에서는 모든 것이 '객체'

- 단순하게 'a'라는 문자를 표현하는 것으로 끝내는 것이 아니고, 'a'라는 객체를 만들고 다양한 속성과 행동을 넣어둠  -> 객체지향프로그래밍

#### dictionary , set / list , tuple

- dictionary , set = Container타입
- list , tuple = Sequence타입 

*컨테이너 , 시퀀스타입에 대해서 좀 더 공부가 필요

### 🧡dictionary(dict)

- key에 value의 값들이 각각 매핑되어있는 객체 
- 순서없고 중복있음 **(key는 중복이 안되고 , value 는 중복이 가능)** 구분은 콜론(:) 데이터의 구분(,)
- java 의 map 이랑 비슷하다
- iterable 객체 : 즉, 반복가능한 객체

#### dict 값 추가하기

> ```python
> dict01 = dict()
> dict01[1] = 'one'
> dict01['two'] = 2
> 
> #실행결과
> {1: 'one', 'two': 2}
> ```



- 이미 dict01객체 내에 있는 key 값을 다시 추가하면 ? -> 기존의 값은 사라지고 새로운 값 추가

  => **key 는 중복이 안된다.**

> ```python
> dict01 = dict()
> dict01[1] = 'one'
> dict01['two'] = 2
> dict01['two'] = 3
> 
> #실행결과
> {1: 'one', 'two': 3}
> ```
>

- value는 중복가능

> ``` python
> dict01 = dict()
> dict01[1] = 'one'
> dict01[2] = 'one'
> 
> #실행결과
> {1: 'one', 2: 'one'}
> ```
>

#### dict 값 제거하기

- `del`

> ```python
> dict02 = dict()
> dict02['name'] = 'yujeong'
> dict02['age'] = 26
> dict02['addr'] = 'seoul'
> 
> #실행결과
> {'name': 'yujeong', 'age': 26, 'addr': 'seoul'}
> 
> del dict02['addr'] #삭제
> 
> #실행결과
> {'name': 'yujeong', 'age': 26}
> 
> ```
>

#### dict는 literable 한 객체 -> 반복가능한 객체 

- for 문을 통해서 key 값 / value 값 가져오기

  dict 타입은 순서정보가 없기 때문에 , 다른 순서로 key값이 출력될 수 있다.

> ``` python
> dict03 = {
>     'name' : 'yujeong',
>     'age' : 26,
>     'addr' : 'seoul'
>     }
> 
> for key in dict03:
>     print(key)
>     
> #실행결과
> name
> age
> addr
> #-----------------------------
> 
> for key in dict03:
>     print(dict03[key])
> #실행결과
> yujeong
> 26
> seoul
> ```
>

#### dict 타입의 속성

- `setdefalut` : dict안에 만약 이미 해당 key 값으로 지정되어있다면 , 값을 덮어쓰지않고 기존에 있는 값을 리턴 / 만약 해당 key값이 없었다면 , 지정한 값을 리턴해주고 dict안에 값을 세팅

> ```python
> a = {'이름' : '최유정', '나이' : 26}
> 
> #'이름'이라는 키 값에 '최유정'이 있어서 덮어쓰지않고 기존에 값을 리턴
> print(a.setdefault('이름', '김지은'))
> #실행결과
> 최유정
> 
> #'별명' 이라는 해당 키 값이 없었기 때문에 지정한 값을 리턴해주고 새로추가해줌
> print(a.setdefault('별명', '쩡이'))
> #실행결과
> 쩡이
> 
> print(a)
> #실행결과
> {'이름': '최유정', '나이': 26, '별명': '쩡이'}
> ```
>

- `get` : dict안에 매칭되는 값이 없다면 None을 리턴 / 키워드에 매칭되는 값이 있다면 해당 값을 리턴 / 값이 없더라도 에러가 발생하지 않음

> ```python
> b = {'one' : 1 , 'two' : 2}
> print(b.get('one'))
> print(b.get('three'))
> 
> #실행결과
> 1
> None
> 
> #주의
> #원래 있는 값은 나
> print(b['one'])
> #실행결과 
> 1
> 
> #하지만 , 매칭되는 값이 없는데 get메소드를 안쓴다? 에러남
> print(b['three'])
> #KeyError: 'three' 
> 
> ```
>

- `update` : 두개의 dict를 합쳐준다.

> ```python
> a = {'이름' : '최유정', '나이' : 26}
> b = {'one' : 1 , 'two' : 2}
> a.update(b)
> print(a)
> 
> #실행결과
> {'이름': '최유정', '나이': 26, 'one': 1, 'two': 2}
> ```
>

- `keys` : 키들로만 새로운 객체를 생성해준다.

> ```python
> b = {'one' : 1 , 'two' : 2}
> print(b.keys())
> 
> #실행결과
> dict_keys(['one', 'two'])
> ```
>

- `values` : values값들로만 새로운 객체를 생성해준다.

> ```python
> b = {'one' : 1 , 'two' : 2, 'three' : 3}
> print(b.values())
> 
> #실행결과
> dict_values([1, 2, 3])
> 
> #for문 사용 - > value를 반복시킬 수 있음
> for value in b.values():
>     print(value)
> #실행결과 
> 1
> 2
> 3
> ```
>

- `items` :  key 와 value를 쌍으로 묶은 새로운 객체를 만들어준다.

> ```python
> b = {'one' : 1 , 'two' : 2, 'three' : 3}
> print(b.items())
> 
> #실행결과
> dict_items([('one', 1), ('two', 2), ('three', 3)])
> 
> #for문 사용
> for some in b.items():
>     print(some)
> 
> #실행결과
> ('one', 1)
> ('two', 2)
> ('three', 3)
> 
> ```
>



### 💛set

- dictionary와 마찬가지로 중괄호 key-value 형태가 아니라 하나의 값을 , 로 구분해서 담는다.
- 컴파일 할 때마다 자기맘대로 바뀐다.
- 생성자에 literable 한 객체(sequence)를 넣으면 set의 값으로 각각 분리되어 들어간다.
- **중복없고 순서없음**

> ```python
> string = '유정아안녕?'
> print(set(string))
> 
> #실행결과
> {'녕', '안', '?', '유', '아', '정'}
> 
> #순서는 계속 바뀐다
> ```
>

> ```python
> b=set('hello')
> print(b)
> 
> #실행결과 
> {'l', 'o', 'e', 'h'}
> {'o', 'l', 'e', 'h'}
> #순서가 바뀜 , 중복이 없음 
> ```
>

#### set 값 추가하기 

- `add`

> ```python
> a = {1,2,3}
> a.add(4)
> print(a)
> 
> #실행결과
> {1, 2, 3, 4}
> ```
>

#### set 값 제거하기

- `pop` : 임의의 값을 제거한다.
- `remove ` : 특정 값 제거한다.

> ```python
> b={'a','b','c','d'}
> b.remove('c')
> print(b)
> 
> #실행결과
> {'a', 'd', 'b'}
> ```
>

> ```python
> b={'a','b','c','d'}
> b.pop()
> print(b)
> 
> #실행결과
> {'c', 'a', 'd'}
> {'a', 'd', 'c'}
> {'a', 'd', 'b'}
> #등 순서없이 임의의 값을 제거한다
> #set객체가 비어있으면 에러를 발생시킨다
> 
> #신기한 거 발견!!!
> c={5,3,6,7,9,2}
> c.pop()
> print(c)
> #실행결과 : {3, 5, 6, 7, 9} 제일 작은 숫자인 2가 사라짐
> c.pop()
> print(c)
> #실행결과 ; {5, 6, 7, 9} 그 다음 작은 숫자 3이 사라짐 
> 
> #숫자끼리 있을 때면, 제일 작은 숫자부터 사라지나보다!
> ```
>

#### set의 속성값

- `update` : 다른 set을 받아서 업데이트합니다.

> ```python
> c={1,2}
> d={'2','3','4'}
> c.update(d)
> print(c)
> 
> #실행결과
> {1, 2, '3', '4', '2'}
> {'2', 1, 2, '4', '3'}
> # 등 순서없이 나온다
> ```
>

-  `union`: 다른 set 객체를 받아 합집합을 계산해서 리턴해줌(**합집합**)

> ``` python
> a={1,2,3}
> b={3,4,5}
> print(a.union(b))
> 
> #실행결과
> {1, 2, 3, 4, 5}
> ```
>

- `intersection`: **교집합**

> ```  python
> a={1,2,3}
> b={3,4,5}
> print(a.intersection(b))
> 
> #실행결과
> {3}
> ```
>

- `-` ,`difference` : **차집합**

> ```python
> a={1,2,3}
> b={3,4,5}
> print(a-b)
> 
> #실행결과
> {1, 2}
> ```
>

- 상위집합확인 : 해당 집합객체가 다른 집합객체의 상위집합인지 확인한다.

> ```python
> c={1,2,3}
> d={1,2,3,4}
> print(d.issuperset(c))
> print(c.issuperset(d))
> 
> #실행결과
> True
> False
> ```
>

- set객체는 set타입과 동시에 iterable타입 -> 반복가능

> ```python
> d={1,2,3,4}
> for d in {1,2,3,4}:
>     print(d)
>     
> #실행결과
> 1
> 2
> 3
> 4
> ```
>

#### set의 여러가지 함수들

`clear` : 집합 안에 있는 모든 값을 삭제

`copy` : 집합을 복제하여 리턴



### 💚list

- **순서가 있고 중복이 있음 **
- 수정이 가능함 (mutable)
- 리스트는 [ ] 대괄호로 생성 
- 리스트 안에 리스트 가능
- list 는 sequence타입이기 때문에 인덱스를 통해서 값을 찾을 수 있다.

> ```python
> a = [1, 'a', 3, (1,2)]
> print(a[0])
> print(a[1])
> 
> #실행결과
> 1
> a
> ```
>

#### list 값 수정

> ```python
> a=[1,2,3]
> a[0] = 'a'
> a[1] = 'c' #a[-2]랑 똑같음
> print(a)
> 
> #실행결과
> ['a', 'c', 3]
> ```
>

#### list 요소추가

- `append`

> ```python
> a=[]
> a.append(1)
> a.append('a')
> print(a)
> 
> #실행결과
> [1, 'a']
> ```
>

- `insert` : 특정위치에 값을 추가할 수 있는 메소드 `list.insert(객체를 추가할 위치, 추가할 객체)`

> ```python
> a=[1,2,3,4]
> 
> #insert(객체를 추가할 위치, 추가할 객체)
> a.insert(1, 'a')
> print(a)
> 
> #실행결과
> [1, 'a', 2, 3, 4]
> ```
>

#### list 값 제거

- `del` : 값 제거해준다.

> ```python
> a=[1,2,3,4]
> del a[0]
> print(a)
> 
> #실행결과
> [2, 3, 4]
> ```
>

#### list의 여러가지 함수 

`sort` : 순서대로정렬

`reverse` : 역순으로 정렬

`remove(a)`  : a를 찾아서 삭제(위치는 관계없이 찾아서 삭제)
`pop` 마지막 자료를 찾아서 삭제

> ```
> list_a=[1,2,3,4,5]
> list_a.pop() 
> print(list_a)
> 
> #실행결과
> [1, 2, 3, 4]
> ```

`extend` : 추가적인 내용을 연장해준다 . `apppend` 와 차이가 있음!

> ```python
> list_a=[1,2,3,4]
> list_a.extend([5,6,7])
> print(list_a)
> #실행결과 [1, 2, 3, 4, 5, 6, 7]
> 
> list_a.append([5,6,7])
> print(list_a)
> #실행결과 [1, 2, 3, 4,  [5, 6, 7]]
> 
> #위의 코드로 append 와 extend 차이점을 볼 수 있음
> ```



### 💙tuple

- 괄호를 이용해 생성 ( )

  주의 : 빈 튜플을 만들 때만 사용 / 비어있지 않은 튜플은 컴마를 통해 생성된다. 

- 한 번 생성하면 수정할 수 없음(immutable) **리스트와 반대되는 개념**
- container 타입 / iterable 타입
- 튜플에는 append라는 속성없음 
- 튜플 생성자에는 아규먼트 **1개만** 들어가야함!

- **순서가 있고 중복이 있음**

```python
a = 'yujeong'
print(a[0])
print(a[1])
print(a[2])

#실행결과
y
u
j

b=('a','b','c','d')
print(b[0])
print(b[1])
#실행결과
a
b

#시퀀스 타입의 특정이다! 
```

- iterable 타입이므로 반복가능한 객체

```python
c= 1,2,3,4, '5', '5','yu','jeong'
for value in c:
    print(value)
    #value에는 다른 변수가 들어가도 된다.
#실행결과
1
2
3
4
5
5
yu
jeong
```

```python
c= 1,2,3,4, '5', '5','yu','jeong'
print(1 in c)
print(5 in c)
print(len(c))

#실행결과
True
False
8
```

#### tuple 값 추가하기

- 튜플에는 append라는 속성이 없기때문에 리스트로 변환하고 append를 해주어야한다.

```python
a=tuple([1,2,3,4]) #tuple에는 단 하나의 아규먼트만 들어가야한다.
 #a=(1,2,3,4)랑 같음
 #실행결과 : (1,2,3,4)
list_a=list(a)
#실행결과 : [1,2,3,4]
list_a.append(5)
#실행결과 : [1,2,3,4,5]
```

`+` : 튜플끼리 더해줄 수 도 있다.

```python
b=tuple([1,2,3,4])
c=(1,2,'3')
d=b+c
print(d)
	실행결과 : (1,2,3,4,1,2,'3')
      #더하기 가능!!!
```

#### *착각하지 말 것 : 함수는 독립적 메소드는 종속적 !

#### 😫헷갈리는 부분 다시 정리 

- list : 순서 o 중복 o 수정 o 
- tuple : 순서 o 중복 o 수정 x
- dict : 키와 밸류형태 순서 x 중복 o(키는 중복x 밸류만 o) 수정 o
- set : 키값으로만 존재 순서 x , 중복 x 





















