# Algorism

### ✨factorial (팩토리얼)

```python
#n! = n * (n-1) * (n-2) * ... * 2 * 1
def factorial(n):
    res = 1
    for i in range(1,n+1):
        res *= i
    return res

#재귀함수
#재귀함수를 쓸 때 중요한점 ! 종료조건
def factorialRecursion(n):
    if n == 1:
        return 1
    
    return n * factorialRecursion(n-1) #나 자신을 계속 호출하고 있다.

if __name__=='__main__':
    n = int(input('숫자를 입력해주세요  : '))
    print('{} factorial = {}'.format(n, factorial(n)))
    
#실행결과
숫자를 입력해주세요  : 3
3 factorial = 6

숫자를 입력해주세요  : 10
10 factorial = 3628800
...

```

### ✨bubble(버블정렬)

```python
def bubble(sort_data):
    for i in range (len(sort_data) -1):
        for j in range(len(sort_data) -i -1):
            if sort_data[j] > sort_data[j+1]:
                sort_data[j],sort_data[j+1] = sort_data[j+1],sort_data[j]
            print(sort_data)
        print(sort_data)

if __name__=='__main__':
    sort_data = [5,2,1,3,4]
    bubble(sort_data)
```

