# 📚Python_조건문 / 반복문

#### 🔹 반복문 : for / while

#### 🔹 조건문 : 조건에 맞는 상황이 주어졌을 때, 실행문이 실행되는 것

#### 	if / if else / if elif else 

---

### 🦄while

```python
while 조건식:
	반복할코드
	블록
    
#while 키워드 다음에 조건식이 오게된다. 조건식 다음에 무조건 콜론!
#조건식이 참인 동안, 들여쓰기 된 코드는 반복된다. 
```

- 1~10출력하기

```python
i=1
while i <= 10:
    print(i)
    i += 1 
    
#실행결과
1
2
3
4
5
6
7
8
9
10
    

```

***주의 : 파이썬에는 자바와 다르게 ++이 없다.**



- while 문을 사용하여 1~10까지의 총 합계를 출력하기

```python
sum=0
cnt=1
while cnt <=10:
    sum+= cnt
    cnt +=1
else:
    print('sum',sum,sep='=')
    print('cnt',cnt,sep='=')
    
#실행결과
sum=55
cnt=11
```



- 조건식이 거짓이면 else 실행한다.

``` python
while True:
    print("true")
    break
else:
    print("false")
#실행결과
true

while False:
    print("true")
    break
else:
    print("false")
#실행결과 
false
    
```



- while문은 계속 반복되기 때문에 필요없을 시, `break`를 통해서 강제로 종료시켜주어야한다.

```python
i = 0
while True:
    print(i)
    i += 2
    if i == 10:
        print("10이다 끝!!!")
        break
else:
    print("실행이 안될거야..")
    
#실행결과
0
2
4
6
8
10이다 끝!!!
```



- `continue`  : 반복문을 완전히 종료시키는 것이 아니라, 이번 반복만 생략시키는 방법

  반복문 내에서 continue 를 만나게되면 , 아직 반복문 내에 실행해야하는 코드가 남아있더라도 실행하지않고 다음 반복문으로 넘어간다

- 1부터 10까지 홀수만 출력하기

```python
j = 0
while j < 11:
    j += 1
    if j == 10:
        break
    if j % 2 == 0: #짝수면 아래코드를 실행하지않고 넘어감
        continue 
    print(j)  
    
 #실행결과
1
3
5
7
9
```



### 🦄for

- 파이썬에는 iterable 타입이라는 객체가 있다 (type()으로 확인할 때 나오는 객체타입이랑은 조금 다름) 

  어떤 객체가 반복가능하다면 iterable 객체라고 이야기한다. 

  즉 , **순서대로 값을 가져올 수 있는 객체**

  앞서 배웠던 **list, tuple, dict**가 iterable객체

```python
for 변수 in iterable객체:
    코드블록
    
#변수에는 iterable객체 값이 하나씩 순차적으로 담겨서 반복되어서 나온다.
```

```python
subject = ['java', 'javascript', 'python']

for i in subject:  # 서브젝트 안에 있는 값을 하나하나가지고와서 i에다가 담는다.
    print(i, end=' ')
else:  # 반복문에 else가 붙을 수있나요? 파이썬에서는 붙을 수 있음 
    print('재미써!!!!!!!!!!')
    
#실행결과
java javascript python 재미써!!!!!!!!!!

#for문이 정상적으로 다 실행된 후에 else문이 실행된다.
```



- for문도 `break` 사용가능하다. `break`를 만나서 종료되면, else문은 실행되지않는다.

```python
i = 0
for c in 'abcde':
    print("{i}번째 문자는 {c}입니다.".format(i=i , c=c))
    i += 1
else:
    print("종료")
    
#실행결과
0번째 문자는 a입니다.
1번째 문자는 b입니다.
2번째 문자는 c입니다.
3번째 문자는 d입니다.
4번째 문자는 e입니다.
종료

#break 를 사용했을 때 
i = 0
for c in 'abcde':
    print("{i}번째 문자는 {c}입니다.".format(i=i , c=c))
    i += 1
    if i == 3:
        break
else:
    print("종료")

#실행결과
0번째 문자는 a입니다.
1번째 문자는 b입니다.
2번째 문자는 c입니다.


#else문은 실행안된 것을 볼 수 있다.
```



- `range` : 숫자 범위 내에서 반복가능한 객체를 만들 수 있다 

  `renge(start, end, step)` step : 어떤 간격으로 다음숫자로 넘어갈 지 정한다. 기본값 1

- 1부터 20까지 출력하기

```python
for i in range(1,21):
    print(i, end= " ") #일렬로 안 나오게 처리하는 방법
    
 #실행결과
 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 
```

```python
for j in range(1,11,2):
    print(j, end= " ")
    
#실행결과
1 3 5 7 9
```

- 구구단 출력하기

```python
for i in range(2,10):
    print("<",str(i),"단>")
    for j in range(1,10):
        print("{}*{}={}".format(i,j,i*j)) 
        
#위와같은 방법(.format) 외에도 다양한 방법으로 출력할 수 있음
#print(str(i) + '*' + str(j) + ' = ' + str(i*j))
#print(str(i), '*', str(j), '=', str(i * j))
```

- 1~10 거꾸로 출력하기

```python
print("1~10 거꾸로 출력하기!")
for i in range(10,0,-1):  #뒤에 -1을 붙여주면 거꾸로 출력이된다.
    print(i, end = ' ')
#실행결과
10 9 8 7 6 5 4 3 2 1 
#그럼 -2를하면 ??
10 8 6 4 2  #이렇게 나오겠지!?
```



### 🦄if/ if~else / if elif else

- if문 : 조건이 true일 때는 무조건 실행이되고, false일때는 무조건 실행이 안된다.

```python
a = 1
if a == 1:
    print("정답")
else:
    print("틀렸어")
 
#실행결과
정답

a = 1
if a == 2:
    print("정답")
else:
    print("틀렸어")
    
#실행결과
틀렸어
```

- if ~ else문 : if의 조건이 들어맞지 않을 때 실행되는 또 다른 실행문(else)

```python
score1 = 50
if score1 > 80:
    print("합격입니다.")
else:
    print("불합격입니다.")

#실행결과
불합격입니다.
```

- if ~ elif ~ else(다중조건문)

```python
score = 73
if True:
    if score >= 80:
        print("a대학 지원가능") 
    elif score >= 70:
        print("b대학 지원가능")
    else:
        print("대학못감")
 
#실행결과
b대학 지원가능
```



#### 🦄연산자

- **관계연산자**

  a > b 

  a >=b

  a!=b

  a==b

- **boolean**(조건문에서 유용하게 사용될 수 있다. / 참,거짓)

  내용이 없을 때와 숫자 0은 False 

  내용이 있거나 숫자 1은 True

```python
a=()
print(bool(a))
#실행결과 : 내용이 없으므로 False

b={'a','b'}
print(bool(b))
#실행결과 : 내용이 있으므로 True
```

- **논리연산자**(and or not)

  a and b : a와 b가 **모두** 참 true

  a or b : a와 b 둘 중에 **하나만** 참이라도 True

  a not b : 논리식을 반전 not 1 = 1 거짓이다. 즉 참,거짓을 반대로 반전시킨다.



