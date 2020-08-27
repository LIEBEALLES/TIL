# MongoDB(2)

## 🔸복습🔸

- javascript interpreter 사용하기때문에 ->js program , library, function 사용가능 ex) jquery
- collection : rdbms의 table
- document : BSON (Binary Json)으로 저장
  - field(key) 중복불가
  - 대소문자구별
- insertOne / insertMany : 다수의 값을 입력할 때 배열형태 안에 넣어줄 수 있었다.
- find(query, projection)
  - cursor : 다른 애들이랑은 다르게 커서라는 변수 **일회용**
- updateOne / updateMany :필드를 수정
- replaceOne(filter, update, options)
- deleteOne, deleteMany
- aggregation
  - stage 순서 매우 중요

---

## 🧸map reduce

> **aggregation framework가 처리하지 못하는 복잡한 집계작업에 사용**
>
> **javascript function을 사용하여 복잡한 작업처리**
>
> **shard에 대응 - 분산처리가능**
>
> 

> ### ✨순서 query -> map -> reduce -> out
>
> **map : data mapping (grouping)**
>
> **reduce : 집계 연산 실행**
>
> **query : 입력될 docment**
>
> **out : collection or document 출력**

![image-20200827175953894](https://user-images.githubusercontent.com/67030978/91433726-b0fbf300-e89e-11ea-9589-1fa52fd8d0d8.png)


![image-20200827180245805](https://user-images.githubusercontent.com/67030978/91433749-b9542e00-e89e-11ea-8085-eafb5d761dd0.png)


- test가 final인 document 들의 이름과 국어와 영어를 출력하고, '국어와 영어의 합' 을 출력하자

  -> 출력못해 . why?  

  - aggregation은 컬럼 하나만 집계한다. 그래서 국어와 영어의 합을 출력하지 못한다.

    그래서 우리는 **map reduce를 쓴다!**

### ✨map 

**우리가 필요한 데이터들을 가지고 와준다.**

```
function myMap(){
   emit(this.score,{name:this.name, kor:this.kor, eng:this.eng, test:this.test})
}
```

### ✨reduce

##### 가지고 온 거 실제로 처리해준다.

```javascript
function myReduce(key,values){
	var result = {name: new Array(),kor:new Array(),eng:new Array(),total:new Array()};

//값을 넣어줄 공간을 미리 만들어놨다 / 리턴될 것임

   values.forEach(function(val){
   if(val.test=='final'){
   result.total+=val.kor+val.eng+" ";
   result.name+=val.name+" ";
   result.kor+=val.kor+" ";
   result.eng+=val.eng+" ";
   }

//데이터들을 reduce에서 하나하나 가지고와서 처리해주고 있다.

});
return result
}
```

```mysql
db.score.find()

#실행결과
{ "_id" : ObjectId("5f45f39944b6a5a34bbbc3aa"), "name" : "홍길동", "kor" : 90, "eng" : 80, "math" : 98, "test" : "midterm" }
{ "_id" : ObjectId("5f45f39944b6a5a34bbbc3ab"), "name" : "이순신", "kor" : 100, "eng" : 100, "math" : 76, "test" : "final" }
{ "_id" : ObjectId("5f45f39944b6a5a34bbbc3ac"), "name" : "김선달", "kor" : 80, "eng" : 55, "math" : 67, "test" : "midterm" }
{ "_id" : ObjectId("5f45f39944b6a5a34bbbc3ad"), "name" : "강호동", "kor" : 70, "eng" : 69, "math" : 89, "test" : "midterm" }
{ "_id" : ObjectId("5f45f39944b6a5a34bbbc3ae"), "name" : "유재석", "kor" : 60, "eng" : 80, "math" : 78, "test" : "final" }
{ "_id" : ObjectId("5f45f39944b6a5a34bbbc3af"), "name" : "신동엽", "kor" : 100, "eng" : 64, "math" : 89, "test" : "midterm" }
{ "_id" : ObjectId("5f45f39944b6a5a34bbbc3b0"), "name" : "조세호", "kor" : 75, "eng" : 100, "math" : 100, "test" : "final" }
```

```mysql
db.score.mapReduce(myMap,myReduce,{out:{replace:'MyRes'}})

db.myRes.find()
#실행결과
{ "_id" : null, "value" : { "name" : "조세호 유재석 이순신 ", "kor" : "75 60 100 ", "eng" : "100 80 100 ", "total" : "175 140 200 " } }
```



## 🧸python에서 mongoDB연결하기

> 참고 : https://pymongo.readthedocs.io/en/stable/tutorial.html 
>
> cmd창에서 pip install pymongo 설치
>
> - pymongo : python으로 mongodb를 커넥션하여 사용할 때 가장 많이 사용하는 라이브러리

```python
#설치완료 되었으면, import해준다.

from pymongo import MongoClient

#mongodb와 연결
#client = MongoClient('127.0.0.1', 27017) 
#client=MongoClient('mongodb://localhost:27017')
client = MongoClient('localhost', 27017)

#연결된 client에서 db와 연결
#db = client.test
db=client['test']

#연결된 db에서 collection 가져오기
#collection = db.score
collection = db['score']

for doc in collection.find()
	print(doc)
    
#실행결과
#score collection에 있는 값들을 불러와준다 . 
{'_id': ObjectId('5f472c19245c18f4fb85066a'), 'name': '최유정', 'kor': '100', 'eng': '85', 'math': '65'}
{'_id': ObjectId('5f472c19245c18f4fb85066b'), 'name': '한지용', 'kor': 100, 'eng': 100, 'math': 100}
{'_id': ObjectId('5f472c19245c18f4fb85066c'), 'name': '강여림', 'kor': 90, 'eng': 90, 'math': 90}
{'_id': ObjectId('5f472c19245c18f4fb85066d'), 'name': '김지후', 'kor': 80, 'eng': 80, 'math': 80}
{'_id': ObjectId('5f472c19245c18f4fb85066e'), 'name': '위영석', 'kor': 70, 'eng': 70, 'math': 70}
{'_id': ObjectId('5f472c19245c18f4fb85066f'), 'name': '최유정', 'kor': 80, 'eng': 70, 'math': 40}

```

### ✨mongo01_find

```python
from pymongo import MongoClient
import pprint #예쁘게 출력

client = MongoClient('localhost', 27017)
db = client.test
score = db.score

cursor = score.find()
print(cursor)
	#실행결과
    #<pymongo.cursor.Cursor object at 0x00000242F081DFA0>

for doc in cursor:
    pprint.pprint(doc)
    #실행결과
    #{'_id': ObjectId('5f472c19245c18f4fb85066a'),
 'eng': '85',
 'kor': '100',
 'math': '65',
 'name': '최유정'}
{'_id': ObjectId('5f472c19245c18f4fb85066b'),
 'eng': 100,
 'kor': 100,
 'math': 100,
 'name': '한지용'}
{'_id': ObjectId('5f472c19245c18f4fb85066c'),
 'eng': 90,
 'kor': 90,
 'math': 90,
 'name': '강여림'}
{'_id': ObjectId('5f472c19245c18f4fb85066d'),
 'eng': 80,
 'kor': 80,
 'math': 80,
 'name': '김지후'}
{'_id': ObjectId('5f472c19245c18f4fb85066e'),
 'eng': 70,
 'kor': 70,
 'math': 70,
 'name': '위영석'}
{'_id': ObjectId('5f472c19245c18f4fb85066f'),
 'eng': 70,
 'kor': 80,
 'math': 40,
 'name': '최유정'}
    
kang = score.find({'name': '강여림'}) #find로 값일 가져오면 기본적으로 cursor객체 형태로 가져온다
print(kang) #하나만 가져와도 cursor객체로 가져와지기 때문에 값이 제대로 나오지 않고 cursor값이 나온다
for doc in kang:
    pprint.pprint(doc)
    print(doc)

#실행결과
<pymongo.cursor.Cursor object at 0x0000020AFD38CB50>
{'_id': ObjectId('5f472c19245c18f4fb85066c'),
 'eng': 90,
 'kor': 90,
 'math': 90,
 'name': '강여림'}
{'_id': ObjectId('5f472c19245c18f4fb85066c'), 'name': '강여림', 'kor': 90, 'eng': 90, 'math': 90}

#알아두어야 할 점!: 만약 값이 없으면 none도 아니고 걍 안나온다 당황했음

hong = score.find_one({'name':'홍길동'})
pprint.pprint(hong) #find_one으로 값을 가져오면 cursor객체가아닌 값 그대로를 가져와서 바로 출력해 줄 수 있다

#실행결과 
None
#알아두어야 할 점!: find_one일 때 값이 없으면 none이라고 나오지만 find 일때는 값이 없으면 none이라고 안 알려준다.


#국어점수가 60점보다 더 큰 document들을 모두 출력하자
docs = score.find({'kor':{'$gt':60}})
for doc in docs:
    pprint.pprint(doc)
    
print('score collection안에 있는 document의 총 갯수 :' , score.count_documents({}))

```

### ✨mongo02_insert

```mysql
from pymongo import MongoClient
import pprint

client = MongoClient('localhost', 27017)

db = client.test
score = db.score

#{name : 자기이름 , kor : ? , eng : ? , math : ?} 등록하자


choi = score.insert_one({'name' : '최유정' , "kor" : "100", "eng" : "85", "math" : "65"})
print(choi.inserted_id)
#실행결과
5f4783a8877f5d2404120224

#여러개 호출
result = score.insert_many([
    {
        'name' : '한지용',
        'kor' : 100,
        'eng' : 100,
        'math' : 100
    },
    {
        'name' : '강여림',
        'kor':90,
        'eng':90,
        'math':90
        
    },
        {
        'name' : '김지후',
        'kor':80,
        'eng':80,
        'math':80
        
    },
    {
        'name' : '위영석',
        'kor':70,
        'eng':70,
        'math':70
        
    },
        {
        'name' : '최유정',
        'kor':80,
        'eng':70,
        'math':40
        
    }

])

pprint.pprint(result.inserted_ids)
#실행결과
[ObjectId('5f478406526bb599a06c92b3'),
 ObjectId('5f478406526bb599a06c92b4'),
 ObjectId('5f478406526bb599a06c92b5'),
 ObjectId('5f478406526bb599a06c92b6'),
 ObjectId('5f478406526bb599a06c92b7')]


```

### ✨mongo03_update

```mysql
from pymongo import MongoClient

client = MongoClient("localhost", 27017)
db= client['test']
score = db['score']

#name이 자기이름인 document를 찾아서 , 국어점수를 0점으로 만들자 
result01 = score.update_one({'name':'최유정'}, {'$set': {'kor' : 0}})
print(result01.matched_count) 	#찾은 갯수
print(result01.modified_count) 	#수정된 갯수

#실행결과
1
1

#영어점수가 90 이하인 document를 0점으로 바꿔라.
result02 = score.update_many({'eng': {'$lte' :90}}, {'$set' : {'eng' : 0}})
print(result02.matched_count)
print(result02.modified_count)

#실행결과
16
16

```

### ✨mongo04_delete

```mysql
from pymongo import MongoClient

client = MongoClient('localhost', 27017)

db = client.test
score = db.score

#name이 자기이름인 document를 하나 찾아서, 삭제하자
result01 = score.delete_one({'name' : '최유정'})
print(result01.deleted_count)
#실행결과 
1
result02 =score.delete_many({'math' : {'$lt' : 100}})
print(result02.deleted_count)  #deleted_count()하면 에러남
#실행결과
16

print(score.delete_many({}).deleted_count)
#실행결과
7
```

### ✨mongo05_aggregation

> https://pymongo.readthedocs.io/en/stable/api/pymongo/command_cursor.html

```mysql
from pymongo import MongoClient
import pprint

client = MongoClient('localhost', 27017)

db= client.test
score = db.score

aggr = score.aggregate([
        {'$match' : {'kor' : {'$gte' : 90}}},
        {'$group' : {'_id': 'mysum', 'sum' : {'$sum' : '$kor'}}}
    ])
print(aggr)
#실행결과
#<pymongo.command_cursor.CommandCursor object at 0x00000230620F3220>
#CommandCursorr객체로 넘어오는 것 확인 :  iterable한 객체

pprint.pprint(list(aggr))
#실행결과
#[{'_id': 'mysum', 'sum': 190}]

```



----

### ✨시각화(Visualization) - word cloud

> pip install wordcloud cmd창에서 설치
>
> 한글을 지원해주지 않는다.
>
> json객체의 데이터를 원하는 글씨체,모양으로 적용해 파일을 만든다.  (png파일로 만들어짐)

```python
from wordcloud import WordCloud
import json
#크롤링 한 것 json파일 패키지에 넣기 

cloud = WordCloud(font_path='Goyang.ttf', background_color='white', max_words=30, width=400, height=300)

with open('webtoons.json', encoding='UTF-8') as file:
    webtoon = json.load(file)

res = dict()
for webtoon in webtoon['webtoons']:
    res[webtoon['title']]=int(float(webtoon['star'])*100)
    
visual = cloud.fit_words(res)
visual.to_image()
visual.to_file('cloud.png')
```

#실행결과 (cloud.png라는 파일이 만들어졌다. 패키지를 새로고침하면 생김)


![image-20200827194624757](https://user-images.githubusercontent.com/67030978/91433787-c3762c80-e89e-11ea-8b68-2c2fe97fb396.png)






























