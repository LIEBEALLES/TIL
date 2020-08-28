# 📚python_크롤링

> ##### 파이썬 관련 패키지들이 모여있는 저장소 : https://pypi.org/
>
> ##### PYPI : Python Package Index 
>
> ##### https://www.crummy.com/software/BeautifulSoup/bs4/doc/ 
>
> ##### 설치: cmd창에 pip install beautifulsoup4 

## ⚽크롤링을 위한 사전준비(설치)

- `Requests`  : 사이트에 있는 html 코드를 가져온다.

- `BeautifulSoup` : html문서로 parsing 해줌

## ⚽네이버영화 크롤링

- `import urllib.request` : python 내부에 기본적으로 가지고 있는 패키지 / **원하는 페이지 연결**

- `urlopen()` : urlopen함수의 인자는 데이터를 얻고 싶은 웹 페이지의 주소를 주면 되고, 

  웹에서 얻은 데이터에 대한 객체를 반환해준다.

### 🏀find_all() & find() 함수

- :star:`find_all()` : 해당 조건에 맞는 모든 태그들을 가져온다.
- :star:`find()`: 해당 조건에 맞는 하나의 태그를 가져온다. 중복이면 가장 첫 번째 태그를 가지고옴

```python
from bs4 import BeautifulSoup
import urllib.request 
#파이썬 내부에 기본적으로 가지고 있는 패키지 / 이걸 통해서 내가 원하는 페이지를 연결할 수 있다.

#크롤링 할 페이지를 resp에 담아준다.
resp = urllib.request.urlopen('https://movie.naver.com/movie/running/current.nhn')
#print(resp) 
#실행결과
#<http.client.HTTPResponse object at 0x0000029875831FD0> 데이터에 대한 객체를 반환


soup= BeautifulSoup(resp,'html.parser') #위에서 얻은 객체를 parsing해줌


movies = soup.find_all('dl',class_='lst_dsc') #크롤링할 것들 가지고옴

#제목이랑 별점 출력하기
star = soup.find_all('span', class_='num')
title = soup.find_all('p', class_='rank_tx')

#for문을 통해서 하나하나씩 꺼내기 
for movie in movies:
    title = movie.find('a').text #a태그 제일 위애 있는 것 가지고 올 것이다.
    star = movie.find('span', class_='num').text
    print('[제목: {} , 별점 : {}]'.format(title,star))
    

#실행결과
[제목: 테넷 , 별점 : 8.54]
[제목: 다만 악에서 구하소서 , 별점 : 7.79]
[제목: 오케이 마담 , 별점 : 6.60]
[제목: 극장판 짱구는 못말려: 신혼여행 허리케인~ 사라진 아빠! , 별점 : 9.19]
[제목: 캐리비안 해적과 마법 다이아몬드 , 별점 : 5.50]
[제목: 남매의 여름밤 , 별점 : 9.12]
[제목: 강철비2: 정상회담 , 별점 : 5.04]
...
```



## ⚽네이버웹툰 크롤링

### 🏀태그와 속성을 이용해서 가져오기

> **태그와 속성을 이용할 때 함수의 인자로 원하는 태그를 첫 번째 인자로 , 두 번째 인자에는 {속성 : 값} 형태로(dictionary) 만들어서 넣어준다**.
>
> ex) `webtoons= soup.find('ul', {'class': 'img_list'})`

- :star:`find_all('태그명', {'속성명' : '값' ... })`
- :star:`find('태그명', {'속성명' : '값'})`

### 🏀find() 와 select() 의 차이점

> **select() 와 find()는 기본적으로 같은 역할을 한다. **
>
> **select() : 만족하는 여러 인스턴스를 찾는다.**
>
> **find() : 첫 번째 인스턴스를 반환한다.**

- :star:`find('태그명', class_='클래스명')`
- :star:`select('태그명.클래스명')`
- :star:`select('태그명')`
- :star:`select('.클래스명')`
- :star:`select('상위태그명 > 하위태그명 > 하위태그명')`
- :star:`select('상위태그명.클래스명 > 하위태그명.클래스명')` :바로 아래의(자식)태그를 선택시에는 > 기호를 사용한다.
- :star:`select('상위태그명.클래스명 하~~위태그명')`: 아래의(자손)태그를 선택시에는 띄어쓰기 사용한다.
- :star:`select('상위태그명 > 바로아래태그명 하~~위태그명')`
- :star:`select('.클래스명')`
- :star:`select('#아이디명')`
- :star:`select('#아이디명 > 태그명.클래스명')`
- :star:`select('태그명[속성1=값1]')`

```python
from bs4 import BeautifulSoup
import requests
import json
from ctypes.test.test_as_parameter import dll

#크롤링 할 페이지 requests로 가져온다. (네이버 월요웹툰)
resp = requests.get('https://comic.naver.com/webtoon/weekdayList.nhn?week=mon')
#print(resp)
#실행결과
#<Response [200]> get은 단순히 응답코드만 가지고온다.

#html내용을 가지고 오기위해 text를 가지고와서 / parsing해준다.
#html = resp.text
#soup=BeautifulSoup(html,'html.parser')
#밑에 한 줄 코드와 같은 뜻 
soup = BeautifulSoup(resp.text,'html.parser')

#월요웹툰 제목 , 별점출력하기
webtoons= soup.find('ul', {'class': 'img_list'}) #속성으로 가지고옴
#webtoons2=soup.find_all('ul',class_='img_list') #[여기 안에 나옴]
dl = webtoons.select('dl') #select태그 : 속성으로 가지고 오지 않은 것은 안된다

lst = list() #배열 생성자
for chd in dl:
    title = chd.find('a')['title']
    star = chd.find('strong').text
    #print('{}[{}]'.format(title, star)) 
    
    tmp={} #딕셔너리 생성자
    tmp['title'] =title
    tmp['star']=star
    #{'title' : '제목' ,'star' : 0.00}
    lst.append(tmp)
#print(lst) 리스트에 딕셔너리가 들어가있다. 딕셔너리를 리스트로 관리한다는 말!
#실행결과
#[{'title': '인생존망', 'star': '9.82'}, {'title': '뷰티풀 군바리', 'star': '9.82'}, ...]

res = {} #딕셔너리 생성자
res['webtoons'] = lst
#실행결과 : json형태의 딕셔너리
#{'webtoons': [{'title': '인생존망', 'star': '9.82'}, {'title': '뷰티풀 군바리', 'star': '9.82'},...]
print(res)

#제이슨 객체로 저장하기
res_json = json.dumps(res,ensure_ascii=False) #dumps : 바꾸는 것
print(res_json)
#실행결과
#{'webtoons': [{'title': '인생존망', 'star': '9.82'}, {'title': '뷰티풀 군바리', 'star': '9.82'}, ...]

#패키지 새로고침하면 json파일이 생성되어있다.
with open('webtoons2.json', 'w', encoding='utf-8') as f:
    f.write(res_json)

```



## ⚽스타벅스 매장검색 크롤링

```python
# -*- coding:utf-8 -*-

import requests
import json

#https://www.starbucks.co.kr/store/store_map.do -> STORE -> 매장찾기 -> 지역검색

def getSido():
    url='https://www.starbucks.co.kr/store/getSidoList.do'
    resp = requests.post(url)
    #print(resp.json())(['list'])
    sido_json = resp.json()['list']
    
    #sido_json에 있는 값들을 x에 넣어서 
    #그 x에서sido_cd에 대한 value를 가지고와 
    #그걸 리스트로바꾸자
    sido_code=list(map(lambda x: x['sido_cd'], sido_json))
    #print(sido_code) 
    #실행결과
    #['01', '08', '02', '03', '04', '05', '06', '07', '09', '10', '11', '12', '13', '14', '15', '16', '17']
    
    sido_name=list(map(lambda x:x['sido_nm'], sido_json))
    #print(sido_name)
    #실행결과
    #['서울', '경기', '광주', '대구', '대전', '부산', '울산', '인천', '강원', '경남', '경북', '전남', '전북', '충남', '충북', '제주', '세종']
    
    #zip함수! 확실히 알아둘 것
    sido_dict = dict(zip(sido_code,sido_name))
    #print(sido_dict)
    #실행결과
    #{'01': '서울', '08': '경기', '02': '광주', '03': '대구', '04': '대전', '05': '부산', '06': '울산', '07': '인천', '09': '강원', '10': '경남', '11': '경북', '12': '전남', '13': '전북', '14': '충남', '15': '충북', '16': '제주', '17': '세종'}
    
    return sido_dict
def getGuGun(sido_code):
    url='https://www.starbucks.co.kr/store/getGugunList.do'
    resp = requests.post(url,data={'sido_cd' : sido_code})
    #print(resp.json)
    #실행결과
    #<bound method Response.json of <Response [200]>>
    
    gugun_json= resp.json()['list']
    #print(gugun_json)
    #실행결과
    #[{'seq': 0, 'sido_cd': None, 'sido_nm': None, 'gugun_cd': '0101', 'gugun_nm': '강남구', ...]
    
    #gugun_json에 있는 값들 x에 넣고 그 x에 대한 gugun_cd를 가지고온다 ->리스트로 바꾼다 
    #gugun_json에 있는 값들 x에 넣고 그 x에 대한 gugun_nm를 가지고와서 리스트로 바꿔 
    #이 두개를 합쳐
    gugun_dict=dict(zip(list(map(lambda x:x['gugun_cd'], gugun_json)),list(map(lambda x:x['gugun_nm'], gugun_json))))
    print(gugun_dict)
    
def getStore(sido_code='', gugun_code=''):
    url='https://www.starbucks.co.kr/store/getStore.do'
    resp = requests.post(url, data={
                            'ins_lat' : '37.56682',
                            'ins_lng': '126.97865',
                            'p_sido_cd' : sido_code,
                            'p_gugun_cd' : gugun_code,
                            'in_biz_cd':'',
                            'set_date': ''
        })   
    print(resp.json())
    print(resp.json()['list'])
    
    
#s_name, tel, doro_address, lat, lot -> json으로 만들자
    store_json = resp.json()['list']
    store_list=list()
    count=0
    for store in store_json:
            store_dict=dict()
            store_dict['s_name'] = store['s_name']
            store_dict['tel'] = store['tel']
            store_dict['doro_address'] = store['doro_address']
            store_dict['latitude'] = store['lat']
            store_dict['longtitude'] = store['lot']
            print(store_dict)
            store_list.append(store_dict)
            count += 1
    print(count)
    store_result = dict()
    store_result['store_list'] = store_list
    store_result['count'] = count
    result = json.dumps(store_result, ensure_ascii=False)
    return result
        
        
        
        
        
if __name__=='__main__':
    print(getSido())
    sido=input('도시 코드를 입력하세요 ! : ')
    print(getGuGun(sido))
    gugun = input('구군 코드를 입력하세요 ! : ')
    print(getStore(gugun_code=gugun))
    


```

```python
#위 코드 실행결과
{'01': '서울', '08': '경기', '02': '광주', '03': '대구', '04': '대전', '05': '부산', '06': '울산', '07': '인천', '09': '강원', '10': '경남', '11': '경북', '12': '전남', '13': '전북', '14': '충남', '15': '충북', '16': '제주', '17': '세종'}
도시 코드를 입력하세요 ! : 09
{'0912': '강릉시', '0914': '고성군', '0901': '동해시 ', '0902': '삼척시', '0903': '속초시', '0915': '양구군', '0904': '양양군', '0905': '영월군', '0916': '원주시', '0906': '인제군', '0913': '정선군', '0907': '철원군', '0908': '춘천시', '0917': '태백시', '0909': '평창군', '0910': '홍천군', '0911': '화천군', '0918': '횡성군'}
None
구군 코드를 입력하세요 ! : 0916
{'매장이름': '원주터미널', '전화번호': '1522-3232', '도로명주소': '강원도 원주시 서원대로 178 (단계동)', '위도': '37.343887', '경도': '127.9311'}
... 등
5 #매장갯수
{"store_list": [{"매장이름": "원주터미널", "전화번호": "1522-3232", "도로명주소": "강원도 원주시 서원대로 178 (단계동)", "위도": "37.343887", "경도": "127.9311"}, {"매장이름": "원주혁신도시", "전화번호": "1522-3232", "도로명주소": "강원도 원주시 혁신로 61 (반곡동)", "위도": "37.323157", "경도": "127.980611"}, {"매장이름": "원주무실", "전화번호": "1522-3232", "도로명주소": "강원도 원주시 능라동길 73 (무실동)", "위도": "37.332972", "경도": "127.930448"}, {"매장이름": "원주명륜DT", "전화번호": "1522-3232", "도로명주소": "강원도 원주시 남원로 588 (명륜동)", "위도": "37.335971", "경도": "127.949754"}, {"매장이름": "원주반곡DT", "전화번호": "1522-3232", "도로명주소": "강원도 원주시 동부순환로 37 (반곡동)", "위도": "37.319904", "경도": "127.971570"}], "count": 5}


```



## ⚽selenium (인스타그램 크롤링)

> **자바, 파이썬 , c#, 코틀린, 자바스크립트 , 루비 등등 다양한 언어로 할 수 있다.**
>
> **사무자동화 많이한다.**



## ⚽Python web

> **프레임 워크는 정말 다양한데, 대표적으로 장고와 플라스크가 있다.**

### 🏀flask

- **파이썬 웹 어플리케이션을 만드는 프레임워크**

- **마이크로프레임워크**

- **플라스크는 다양한 프레임워크들 중에서도 매우 심플하고 가벼운 느낌을 가지고있다.** 

- **크롤링한 데이터들을 보여주기 위함**
- **설치 : cmd창에서 pip install flask**

### 🏀django

- **python기반 웹 어플리케이션 프레임워크로 웹 개발을 하기위한 여러가지 서비를 갖추고 있고 가장 많이 사용되고 있다**
- **mvc기반 패턴대로 개발할 수 있도록 이미 구조화 되어있어서 프레임워크 가이드대로 손쉽게 개발이 가능**
- **프레임워크 자체적으로 설계한 개발 패턴에서 크게 벗어날 수 없는 구조이다.**

### 🏸SSR(server side rendering)

> **(java, python, django)**
>
> 서버에서 화면을 만드는 것(서버에서 렌더링)
>
> 완전하게 만들어진 html파일을 받아오고 rendering 하게된다.
>
> 웹 서버에 요청할 때마다 브라우저 새로고침이 일어나고 서버에 새로운 페이지에 대한 요청을 하는 방식
>
> - 장점  
>   1. 초기 로딩 속도가 빨라, 사용자가 컨텐츠를 빨리 볼 수 있다.
>   2. 모든 검색엔진에 대한 검색엔진최적화가 가능하다.
>
> - 단점
>   1. 매번 페이지를 요청할 때마다 새로고침 되기 때문에 사용자 ux가 다소 떨어진다.
>   2. 서버에 매번 요청을 하기 때문에 트래픽, 서버 부하가 커진다

### 🏸CSR(client side rendering)

> 클라이언트 단에서 화면을 만들고 서버에선 데이터만 전달한다.
>
> - 장점
>   1. 첫 로딩에 html과 static파일들만 다 받으면, 동적으로 빠르게 렌더링하기 때문에 사용자 ux가 뛰어나다.
>   2. 서버에 요청하는 횟수가 훨씬 적기 때문에 서버 부담이 덜하다
> - 단점
>   1. 모든 html과 static 파일이 로드될 때까지 기다려야한다.
>   2. 검색엔진최적화(seo) 문제가 발생할 수 있다.

### 🏸SPA(Sing Page Application)

> CSR의 한 종류로써 서버에서 넘겨준 데이터 즉 , 비동기로 응답받은 데이터를 가지고 화면이 깜빡이지않고 하나의 화면에서 모든 것을 처리해준다
>
> 단일 페이지로 구성된 웹 어플리케이션 
>
> 화면이동 시, 필요한 데이터를 서버사이드에서 html로 전달받지 않고(서버사이드렌더링x) 필요한 데이터만 서버로부터 json으로 전달받아 동적으로 렌더링

### 🏸jinja2(https://jinja.palletsprojects.com/en/2.11.x/)

> **진자는 파이썬 웹 프레임워크인 flask에 내장되어있는 template엔진으로 jsp문법이나 es6의 template string과 비슷한 문법을 가지고 있다.**

- {% %} : 파이썬 들어감 / <%%>  : 자바 들어감

- {{ }} : 파이썬 변수 html 출력  / <%= %>  :자바변수 html 출력

---

### 🏀플라스크 맛보기

```python
from flask import Flask #Flask라는 클래스를 불러온다.

app = Flask(__name__) #Flask라는 클래스의 객체를 생성하고(app) 인수로써 __name__을 입력

@app.route('/') #파이썬에선 데코레이션이라고 부른다 자바의 어노테이션이라고 똑같이 생각하면 된다. 
#앱에 대한 경로 , 루트경로로 요청이 들어오면 def hello가 실행된다. hello,world를 리턴해줘!!
def hello():
    return 'hello,world!'

if __name__=='__main__':
    app.run()
    
#실행결과
 * Serving Flask app "hello" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)

#해당주소로 들어가면 웹에서 hello,world! 가 출력되는 것을 볼 수 있다.
```

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_root():
    return '<h1><a href="/flask">hello, flask</a></h1>


@app.route('/flask')
def helloo_flask():
    return '<h1><a href="/">go root</a></h1>'

if __name__=='__main__':
    app.run()
    
 #실행결과
#hello,flask 로 들어가면 hello_flask() 가 실행 왔다리 갔다리 함 신기하다.
```

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_root():
    return'<h1><a href="/test/admin">parameter test</a><h1>'

@app.route('/test/<id>')
def hello_test(id):
    return '<h1>hello,' + id + '</h1><a href="/">go root</a>'
    
    
if __name__=='__main__':
    app.run()
    
 #실행결과
#parameter test링크를 클릭하면 아래함수가 실행되고 , 주소창에 id값을 바꿔넣으면
#return 값에 적용이된다! 
```

```python
from flask import Flask, request

app= Flask(__name__)


#겟방식과 포스트방식에 따라서 다른 거 볼 수 있음! 

@app.route('/', methods=['GET' , 'POST'])
def hello_root():
    html = '''
    <h1>Get or Post</h1>
    <a href="/test">get</a>
    <form action="/test" method="post">
        <input type="submit" value="post"/>
    </form>
    '''
    
    return html

@app.route('/test', methods=['GET','POST'])
def hello_test():
    if request.method=='GET':
        return '<h1 style="color :red;">Request Get</h1>'
    elif request.method == 'POST':
        return '<h1 style="color : green">Request Post</h1>'



if __name__=='__main__':
    app.run()
    
```

### 🏀플라스크_진자 맛보기

```python
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route('/')
def root():
    return '''
        <a href="/hello">hello</a><br/>
        <input type='button' value='hello' onclick='location.href="/hello/name"'/>
    '''
@app.route('/hello') #hello라는 url 
@app.route('/hello/<name>') #이름까지 있음
def hello(name=None):#헬로라는 url 누르면 네임엔 아무것도 안들어감 값이 들어오면 여기다가 저장을 해줄거
    return render_template("hello.html",name=name) #hello.html 이라는 파일을 템플릿을 사용해서 그려주세요. 네임을 네임이라는 이름으로 전달 request.setAttribute("name",name) 인 느낌 / 다르지만 이런 느낌이다

#포스트방식으로 온 애 코맨드로 받는 애를 다시 테스트에 담아가지고 보낸다
@app.route('/test', methods=['POST'])
def test():
    return render_template("test.html", test=request.form['command']) # html에서 온 command값을 appli에서 test 에 담아주고 그걸로 test.html로 보내줌


if __name__=='__main__':
    app.run(host='localhost', port='8585')

```

```html
<!--포스트 방식으로 들어온 애 처리 -->
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

<h1>hello, {{test}}</h1>
<a href="/">root_여기는 test.html</a>
</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<link rel="stylesheet" href="{{url_for('static', filename='test.css')}}"/>
</head>
<body>

{% if name %} <!-- name 에 값이 있으면 true 없으면 false -->
	<h1>hello, {{name}} 여기는 hello.html</h1>
{% else %}
	<h1>hello,world! 여기는 hello.html</h1>
{% endif %}

<!--value test 란 값을 command에 담아서 보낸다 !!!!  -->
<form action="/test" method="post">
	<input type="hidden" name="command" value="value test"/>
	<input type="submit" value="test"/>
</form>
</body>
</html>
```

