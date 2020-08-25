# 📚Python_함수(1)

- python은 인터프리터 언어이기 때문에 main메서드가 밑으로 와야한다.
- java 1.8 의 특징이 lambda이다.
- python에서는 스네이크 기법을 주로쓴다.

#### 🍪python 메인함수

```python
if __name_-=='__main__':
    #실행할 코드
```

#### 🍪input 함수 : 해당 문자열을 보여주고 문자열을 입력받는다.

```python
#예시 : 구구단

n = input('몇 단이 궁금하십니까?' : )

#콘솔창
몇 단이 궁금하십니까? : #입력
  #조건에 맞는 단 출력 
```

```python
def func01():
    print("함수 1 입니다")
    
def func02():
    return("함수 2 입니다")

def func03():
    return{1:'a', 2:'b'}
    
if __name__ == '__main__':
#프로그램의 주 진입점 
    print('프로그램 시작 시 가장 먼저 호출된다!')
    func01() #void
    print(func02()) #return 값이 있음
    print(func03()[1]) #키를 넣어주면 해당 값의 value가 나옴
    
 #실행결과
프로그램 시작 시 가장 먼저 호출된다!
함수 1 입니다
함수 2 입니다
a
```

- **pass**키워드 : 함수에 내용이 없을 때 에러발생을 막아준다.

```python
def gugu():
    pass
```

#### 🍩함수를 이용한 구구단 출력하기

> 1.for문을 사용하여 구구단을 전체 출력하는 함수 gugu()를 작성하고, main 함수에서 호출하자.
> 2.while문을 사용하여 구구단 중 파라미터로 전달된 단만 출력하는 함수 gugudan(x)를 작성하고, main함수에서 콘솔을 통해 x를 입력받아 호출하자.

```python
def gugu():
    for i in range(2,10):
        print('<<%d단>>' % i)
        for j in range (1,10):
            print('{}*{}={}'.format(i, j,i*j))
            

def gugudan(x):
    print('<<',x,'단>>')
    i = 1
    while i < 10:
        #print('{}*{}={}'.format(x,i,x*i)) 
        #위와 같이하면 값이 이상하게 나옴 이유 : x로 들어오는 게 str이기때문
        print('{}*{}={}'.format(x,i,int(x)*i)) 
        i += 1

if __name__=='__main__':
    gugu()
    x = input("몇 단? : " )
    gugudan(x)
    
#실행결과
<<2단>>
2*1=2
2*2=4
2*3=6
2*4=8
2*5=10
2*6=12
2*7=14
2*8=16
2*9=18
<<3단>>
3*1=3
3*2=6
3*3=9
3*4=12
3*5=15
3*6=18
3*7=21
3*8=24
3*9=27
<<4단>>
4*1=4
4*2=8
4*3=12
4*4=16
4*5=20
4*6=24
4*7=28
4*8=32
4*9=36
<<5단>>
5*1=5
5*2=10
5*3=15
5*4=20
5*5=25
5*6=30
5*7=35
5*8=40
5*9=45
<<6단>>
6*1=6
6*2=12
6*3=18
6*4=24
6*5=30
6*6=36
6*7=42
6*8=48
6*9=54
<<7단>>
7*1=7
7*2=14
7*3=21
7*4=28
7*5=35
7*6=42
7*7=49
7*8=56
7*9=63
<<8단>>
8*1=8
8*2=16
8*3=24
8*4=32
8*5=40
8*6=48
8*7=56
8*8=64
8*9=72
<<9단>>
9*1=9
9*2=18
9*3=27
9*4=36
9*5=45
9*6=54
9*7=63
9*8=72
9*9=81
몇 단? : 2
<< 2 단>>
2*1=2
2*2=4
2*3=6
2*4=8
2*5=10
2*6=12
2*7=14
2*8=16
2*9=18
```

#### 🍪lambda함수

> **익명함수 : 함수를 미리 만들지 않고(선언만 해두고), 필요할 떄 바디를 만들어서 사용한다.**
>
> - lambda 사용할 변수 : 식(파라미터)
> - 람다식 안에 람다식 가능

```python
hap01 = lambda a,b: a+b
print(hap01(10,20))

#실행결과
30

hap02 = lambda a,b,c : a + b + c
print(hap02(5,6,7))

#실행결과 
18
---
gob = lambda a,b: a*b
print(gob(30,40))

#실행결과
120

---
a= [(1,'one',9),(2,'two',8),(3,'three',7),(4,'four',6)]
print(a)
#람다 a에 튜플 하나하나가 들어간다. 넘어오는 것들의 1번지를 기준으로 정렬
a.sort(key=lambda a:a[1])
print(a)

#실행결과
[(1, 'one', 9), (2, 'two', 8), (3, 'three', 7), (4, 'four', 6)]
[(4, 'four', 6), (1, 'one', 9), (3, 'three', 7), (2, 'two', 8)]

---
#x,y를 입력하여 x가 y보다 크면 x 아니면 y
min = (lambda x,y : x if x>y else y)(11,22)
print(min)

#실행결과
22

#x랑 y랑 입력하여 x가 y보다 크면 x 아니면 y
max01 = (lambda x, y : x if x>y else y)
print(max01(10,100))

#실행결과
100
---
b = lambda x : (lambda y : x+y)
c = b(100) #lambda y : 100 + y
d = c(90) # 100 + 90
print(d)

#실행결과
190
```

#### 🍩람다식을 이용한 1~100 사이의 숫자 중 5의 배수를 출력하기

```python
baesu = lambda x : bool(x % 5) #0이기떄문에 5의 배수일 때만 false가 나옴
five = [i for i in range(1,101) if not baesu(i)] 
#조건문 앞에 변수를 주고 , 조건문이 true 면 반영한다? 라고 이해함
print(five)

#실행결과
[5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85, 90, 95, 100]
```

#### 🍩None == null

```python
f = lambda x: x if(x % 5 != 0) else None
result = [i for i in range(1,101) if not f(i)]
# not null = true
print(result)


#실행결과
[5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85, 90, 95, 100]

---
result_five = [i for i in range(1,101) if not (lambda x:x if(x % 5 !=0) else None)(i)]
print(result_five)

#실행결과
[5, 10, 15, 20, 25, 30, 35, 40, 45, 50, 55, 60, 65, 70, 75, 80, 85, 90, 95, 100]
```

#### 🍩메인함수에서 람다식

```python
def mysum(x,y):
    return 2 * x + y
if __name__ == '__main__':
    print(mysum(2, 3))
    print((lambda x,y : 2 * x + y)(2,3))
    
#실행결과
7
7
```

#### 🍪모듈

> 함수를 배포하면 모듈이 된다.

#### 🍪ramdom 모듈

- 사용법 : `import ramdom`  -> python이 가지고 있는 랜덤이라는 모듈을 임포트한다.

  ##### ->python에서는 java와 다르게 필요한 곳에서 `import`할 수 있다. 

#### 🍩특정범위 내에서 랜덤으로 숫자뽑기

> **`random.random(n,m)` : n~m 중 하나의 숫자를 반환한다.**
>
> **`random.random()`: 0과1사이의 숫자를 반환한다.**

```python
r = random.randint(1,45)
print(r)

#실행결과
33
32
3
7
25
등 1~45사이의 숫자가 나온다.

```

#### 🍩로또함수

> 1부터 45사이의 숫자 6개를 중복없이 만들어서 리스트로 리턴하는 lotto() 함수를 만들자.
> main함수에서 호출하여 출력하자.

```python
import random #랜덤모듈임포트

#첫 번째 방법
def lotto():
    i = 1
    a=set() #set은 중복이 없으므로 선언해주었다.
    cnt=0 
    while a.__len__() < 6:
        i = random.randint(1,45)
        a.add(i) #set에는 append 속성 x
        cnt += 1 #숫자 6개가 들어가야하기 때문에
    return a

#두 번쨰 방법
lotto = set()
    while len(lotto) < 6:
        ran = random.randint(1,45)
        lotto.add(ran)
        
    result = sorted(lotto)
    return result

if __name__=='__main__':
    print(lotto())
```

#### 🍪math 모듈

- 사용법 : `math` : math라는 모듈을 가지고온다.
  - `from math import pi` :math에서 pi만 가지고온다.

```python
from math import pi

def circle(r): 
    return pi*r*r



if __name__ =='__main__':
    r = int(input('원의 반지름 : '))
    print('반지름이 {}인 원의 넓이는 {}입니다.'.format(r,circle(r)))
    
 #실행결과
원의 반지름 : 4
반지름이 4인 원의 넓이는 50.26548245743669입니다.
```

#### 🍪그래프

```python
#pip install numpy ->수치해석
#pip install matplotlib -> 그래프
#cmd 창에서 설치해주어야한다. 저대로 쓰면 된다!

import numpy as np
import matplotlib.pyplot as plt 
import random
def plt01():
    x = np.arange(0,11)
    y = x
    
    plt.plot(x,y)
    plt.xlabel('x')
    plt.ylabel('y')
    plt.legend(['y=x'])
    plt.show()
    
def plt02():
    y =[random.randint(0,10) for i in range(10)]
    x = range(10)
    plt.bar(x, y)
    plt.xticks(range(11))
    plt.yticks(range(11))
    plt.show()
    
def plt03():
    data = [random.randint(100,1000) for i in range(4)]
    plt.pie(data)
    category = ['first','second','third','fourth']
    plt.legend(category)
    plt.show()    
    
    
    
if __name__=='__main__':
    plt01()
    plt02()
    plt03()
```

![image-20200825230706052](https://user-images.githubusercontent.com/67030978/91185909-57be8300-e729-11ea-915b-cf41ea7656b4.png) **plt01()**



![image-20200825230853438](https://user-images.githubusercontent.com/67030978/91185925-5b520a00-e729-11ea-8ea4-691a28e182c3.png)**plt02()**

![image-20200825230945355](https://user-images.githubusercontent.com/67030978/91185948-6016be00-e729-11ea-9156-0502efca3ccd.png)**plt03()*

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(0,1,50)

y1 = np.cos(4* np.pi * x)
y2 = np.cos(4* np.pi * x) * np.exp(-2 * x)

fig, ax = plt.subplots(2,1)

ax[0].plot(x,y1, 'r-*',lw = 1)
ax[0].grid(True)
ax[0].set_ylabel(r'$sing(4 \pi x)$')
ax[0].axis([0,1,-1.5,1.5])

ax[1].plot(x,y2,'b--o',lw=1)
ax[1].grid(True)
ax[1].set_xlabel('x')
ax[1].set_ylabel(r'$e^{-2x}sin(4 \pi x)$')
ax[1].axis([0,1,-1.5,1.5])

plt.tight_layout()
plt.show()
```



![image-20200825231100090](https://user-images.githubusercontent.com/67030978/91185964-63aa4500-e729-11ea-9a09-cd630db5fdc6.png)**plt**
