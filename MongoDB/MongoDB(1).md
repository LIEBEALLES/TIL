# 📚MongoDB(1)

#### 🍓NoSQL

> **고정되지 않은 테이블 스키마**
>
> - 필요할 때마다 필드를 추가/제거 가능->개발속도가 향상된다.
>
> **데이터 간의 관계를 정의하지 않는 데이터베이스**
>
> - db > collection > document
>
> **분산형 구조(대용량 데이터 저장용이)**
>
> - sharding 지원(클라스터 데이터 상호복제)

++몽고디비는 롤백을 지원하지 않는다.(롤백을 하는 방법이 한 가지 있긴 하다. **oplog**)

#### 🍓mongo shell

- interactive javascript interface
  - javascript interpreter 사용
  - js program, librarym function 활용가능

```mysql
#cmd창에서 mongo 치면 실행된다.

db.jstest.insert({name:'test',age: 100,class:'qclass'}) : 값입력
db.jstest.find() 하면
{ "_id" : ObjectId("5f45ab61b65e36fee78ce9db"), "name" : "test", "age" : 100, "class" : "qclass" } 값이 나오고 
변수에 저장해줄 수도 있다!

var cursor = db.jstest.find()
{ "_id" : ObjectId("5f45ab61b65e36fee78ce9db"), "name" : "test", "age" : 100, "class" : "qclass" }

var today = new Data()
today
ISODate("2020-08-26T00:30:08.509Z")
```

`jstest` : collection

#### 🍓Data Structure

##### 🍌Database

- 독립적인 하나의 권한을 가짐
- 각각의 db는 분리된 파일로 저장
- 예약된 db name
  - admin : rood db
  - local : 복제되지 않는 db(특정 서버에만 저장하는 collection에 사용)
  - config : shard 정보저장

##### 🍌Collection :star:

- document들의 group(rdbms의 table역할)
- schema(스키마)를 가지지 않는다.(document들의 field가 각각 다를 수 있다.)

##### 🍌Document

- data recode를 **:star:BSON(Binary Json)**으로 저장 (중요중요!!!!!)
  - http://bsonspec.org/
- field(key) 중복불가
- 대소문자구별

```mysql
{
	name : "yujeong"				 <-field : value
	age : 26 						<-field : value
	status : "A" 					<-field : value
	groups : ["news" , "sports"] 	<-field : value

}
```

🍌**명령어**

> show dbs : 전체 database목록 
>
> db : 현재 database확인
>
> use dbname : 해당 database로 변경
>
> show collections 현재 database의 collection목록



## 🥑CRUD

#### 🍓insert

- `insertOne`
- `insertMany`

- 필드에 싱글쿼터 , 더블쿼터 상관없다.
- value에 숫자는 상관없지만, 문자열숫자!랑은 다르다.

```mysql
mongodb에 값을 넣어주면 다음과 같이 뜬다.
#단일값 넣기
> db.qclass.insertOne({name : 'hong-gd', class : 'qclass', kor: 100,eng:50,math:70})
{
        "acknowledged" : true,
        //insertedId 엄청 중요함 엄청엄청엄청 중요!!!!!!!!
        "insertedId" : ObjectId("5f45b1aab65e36fee78ce9de") 
       
}
```

- 데이터를 insert하고 있는데 name , class, kor, eng, math 필드를 지정해서 넣어주고 있다 

  _id 라는 필드를 지정해주지 않으면 몽고디비가 알아서 objectId라고 _id를 만들어준다.

```mysql
 #다수값 넣기
 db.inventory.updateMany({status :'A'},{$push : {costs : 50000}})
{ "acknowledged" : true, "matchedCount" : 6, "modifiedCount" : 6 }
> db.inventory.find()
{ "_id" : ObjectId("5f45e3101717d936c1f3ba20"), "item" : "canvas", "qty" : 100, "tags" : [ "cotton" ], "size" : { "h" : 25, "w" : 35.5, "uom" : "cm" } }
{ "_id" : ObjectId("5f45e34a9d6d79b5de5aeff2"), "item" : "journal", "qty" : 25, "tags" : [ "blank", "red" ], "size" : { "h" : 14, "w" : 21, "uom" : "cm" } }
{ "_id" : ObjectId("5f45e34a9d6d79b5de5aeff3"), "item" : "mat", "qty" : 85, "tags" : [ "gray" ], "size" : { "h" : 27.9, "w" : 35.5, "uom" : "cm" } }
```

```mysql
db.myfriends.insert(
	{
		name:'슈퍼맨',
		buddy:['배트맨','원더우먼','아쿠아맨','조커']
	}
)
db.myfriends.insert(
	{
		name:'아이언맨',
		buddy:['토르','헐크','호크아이']
	}
)
```

#### **🍓find 특정조건을 검색**

> https://docs.mongodb.com/manual/reference/operator/query/
>
> https://docs.mongodb.com/manual/tutorial/insert-documents/  : insert methods

```mysql
db.qclass.find({kor:100})
#국어점수가 100점인 document출력

{ "_id" : ObjectId("5f45b1aab65e36fee78ce9de"), "name" : "hong-gd", "class" : "qclass", "kor" : 100, "eng" : 50, "math" : 70 }
{ "_id" : ObjectId("5f45babcb65e36fee78ce9e0"), "name" : "한지용", "kor" : 100, "eng" : 100, "math" : 100 }

db.qclass.find({kor :{$gte:70}}, {name : 1})
{ "_id" : ObjectId("5f45b1aab65e36fee78ce9de"), "name" : "hong-gd" }
{ "_id" : ObjectId("5f45babcb65e36fee78ce9e0"), "name" : "한지용" }
{ "_id" : ObjectId("5f45babcb65e36fee78ce9e1"), "name" : "황인규" }
{ "_id" : ObjectId("5f45babcb65e36fee78ce9e2"), "name" : "위영석" }

db.qclass.find({kor :{$gte:70}}, {name : 1, _id:0})
#국어점수가 70점 이상인 document의 이름만 출력(_id는 출력하지마!)
{ "name" : "hong-gd" }
{ "name" : "한지용" }
{ "name" : "황인규" }
{ "name" : "위영석" }
```

> 0이면 false , 0이 아닌 숫자는 true 왜? 씨언어에서 이렇게 시작했다.

```mysql
db.qclass.find({kor:{$gte:70}}) 

= select * from qclass 
	where kor >= 70; 
```

```mysql
#name 에 l(엘)이 들어간 document조회

> db.qclass.find({name:/l/})
> { "_id" : ObjectId("5f45b96fb65e36fee78ce9df"), "name" : "lee-ss", "midterm" : { "kor" : "100", "eng" : "50", "math" : "70" } }
>
> = select * from qclass where name like%l%
```

##### 🍌cursor 

> var cursor = db.collection.find()
>
> - find()를 통해 리턴되는 cursor를 var변수(js)에 저장할 수 있다.
> - hasNext(), forEach(), toArrary() 등을 사용하여 cursor 내부의 document들을 사용할 수 있다. 

```mysql
var cursor = db.qclass.find()

> cursor.toArray()
> [
>    {
>            "_id" : ObjectId("5f45b1aab65e36fee78ce9de"),
>            "name" : "hong-gd",
>            "class" : "qclass",
>            "kor" : 100,
>            "eng" : 50,
>            "math" : 70
>    },
>    {
>            "_id" : ObjectId("5f45b96fb65e36fee78ce9df"),
>            "name" : "lee-ss",
>            "midterm" : {
>                    "kor" : "100",
>                    "eng" : "50",
>                    "math" : "70"
>            }
>    },
>    {
>            "_id" : ObjectId("5f45babcb65e36fee78ce9e0"),
>            "name" : "한지용",
>            "kor" : 100,
>            "eng" : 100,
>            "math" : 100
>    },
>    {
>            "_id" : ObjectId("5f45babcb65e36fee78ce9e1"),
>            "name" : "황인규",
>            "kor" : 90,
>            "eng" : 90,
>            "math" : 90
>    },
>    {
>            "_id" : ObjectId("5f45babcb65e36fee78ce9e2"),
>            "name" : "위영석",
>            "kor" : 80,
>            "eng" : 80,
>            "math" : 80
>    }
> ]
```

🍌**find()문제풀이**

```mysql
#qclass collection에서풀자

#1.where class='qclass' 출력

db.qclass.find({class:'qclass'})

#2.midterm exists출력

db.qclass.find({midterm:{$exists:true}})

#3.40 <= kor < 90 출력

db.qclass.find({kor:{$gte:40,$lt:90}})
db.qclass.find{$and:[{kor:{$gt:40}},{kor:{$lt:90}}]}
```

#### 🍓비교(comparison)연산자

> 연산자 사용법 : {<field>: {<opr>:<value>}}

| operator | 설명                                                  |
| -------- | ----------------------------------------------------- |
| $eq      | equals / 주어진 값과 일치하는 값                      |
| $gt      | greater than / 주어진 값보다 큰 값                    |
| $gte     | greater than or equals / 주어진 값보다 크거나 같은 값 |
| $lt      | less than / 주어진 값보다 작은 값                     |
| $lte     | less than or equals / 주어진 값보다 작거나 같은 값    |
| $ne      | not equal / 주어진 값과 일치하지 않는 값              |
| $in      | 주어진 배열 안에 속하는 값                            |
| $nin     | 주어진 배열 안에 속하지 않는 값                       |

🍌**sort() : 내림차순, 오름차순** (무조건 1 아니면 -1)

- 내림차순

```mysql
db.qclass.find().sort({kor:-1})
```

- 오름차순

```
db.qclass.find().sort({kor:1})
```

🍌**limit(n) : n개만 출력해준다.**

```mysql
db.qclass.find().sort({kor:-1}).limit(2).pretty()
#국어점수를 내림차순으로 출력하되 , 2개까지만 출력! 그리고 예쁘게 출력해주세요.
{
   "_id" : ObjectId("5f45babcb65e36fee78ce9e0"),
   "name" : "한지용",
   "kor" : 100,
   "eng" : 100,
   "math" : 100
}
{
   "_id" : ObjectId("5f45b1aab65e36fee78ce9de"),
   "name" : "hong-gd",
   "class" : "qclass",
   "kor" : 100,
   "eng" : 50,
   "math" : 70
}
```

##### 🍌pretty() : 예쁘게 출력해준다.

**🍌skip(n) : n개 건너띄고 출력해준다.**

```mysql
db.qclass.find().sort({kor:-1}).skip(1).pretty()
#국어점수 내림차순으로 출력하되, 하나 건너 뛰고 예쁘게 출력해주세요.
{
   "_id" : ObjectId("5f45babcb65e36fee78ce9e0"),
   "name" : "한지용",
   "kor" : 100,
   "eng" : 100,
   "math" : 100
}
{
   "_id" : ObjectId("5f45babcb65e36fee78ce9e1"),
   "name" : "황인규",
   "kor" : 90,
   "eng" : 90,
   "math" : 90
}
{
   "_id" : ObjectId("5f45babcb65e36fee78ce9e2"),
   "name" : "위영석",
   "kor" : 80,
   "eng" : 80,
   "math" : 80
}
{
   "_id" : ObjectId("5f45b96fb65e36fee78ce9df"),
   "name" : "lee-ss",
   "midterm" : {
           "kor" : "100",
           "eng" : "50",
           "math" : "70"
   }
}
```

#### 🍓update

- `updataOne`
- `updateMany`
- `replaceOne`

```
db.inventory.updateOne(
   { item: "paper" }, 아이템이 paper 인애 찾아와서 변경!
   {
     $set: { "size.uom": "cm", status: "P" },
     $currentDate: { lastModified: true }
   }
)
```

```mysql
#1.name hong-gd 찾아서 '홍길동'
db.qclass.updateOne(
	{name:"hong-gd"},
    {
    $set:{"name":"홍길동"}
    }
)
```

##### 🍌자바스크립트를 이용한 update문

```javascript
function updateKor(){var tmp = db.qclass.updateMany({kor:{$lte:90}}, {$set:{kor:0}});return tmp}


```

#### 🍓delete / remove

- `deleteOne`
- `deleteMany`
- `remove`

```mysql
#전체삭제

db.qclass.remove({})

#특정값 삭제

db.qclass.remove({name:'홍길동'})
```

```mysql
#값 하나 삭제

db.qclass.deleteOne({name:'한지용'})
```

```mysql
#class라는 field가 존재하는 모든 document를 삭제

db.qclass.deleteMany({class:{$exists:true}})
```

```mysql
#midterm 필드 안에 kor 필드가 100인 document찾아서 삭제

db.qclass.deleteOne({'midterm.kor':{$eq:"100"}}) #문자열 숫자 조심조심!
```

##### 🍌update / pop 예제

```mysql
#1.아이언맨의 친구에 [캡틴아메리카, 블랙위도우] 추가하자
db.myfriends.update({name: '아이언맨'}, {$push:{buddy:{$each:['캡틴아메리카','블랙위도우']}}})

#2. 슈퍼맨의 친구에서 가장 마지막 한명(조커)빼자.

db.myfriends.update({name:'슈퍼맨'},{$pop:{buddy:1}})

 == db.myfriends.update({name:'슈퍼맨'},{$pop:{buddy:-1}}) : #이건 맨앞

#중요!! 1,-1밖에 없음 !!!!
```

----

## 🥑aggregation 

> **collection이 각 stage를 거치면서 document 처리 및 집계**
>
> **일부처리는 shard 에 대응(각 shard에서 처리)**
>
> - sharding : 데이터를 여러 서버에 분산해서 저장하고 처리할 수 있도록 하는 기술
>
> **:star:pipeline : 이전 단계의 연산결과를 다음 단계에서 사용**
>
> #### **:star:stage 순서중요!!:star::eyes:**



|   SQL    |  NoSQL   |
| :------: | :------: |
|  WHERE   |  $match  |
| GROUP BY |  $group  |
|  HAVING  |  $match  |
|  SELECT  | $project |
| ORDER BY |  $sort   |
|  LIMIT   |  $limit  |
|   SUM    |   $sum   |
|  COUNT   |   $sum   |

```mysql
db.score.aggregate(
  #score collection 에서
  {$match:{"test":"midterm"}},
  #test가 midterm 인 document들을 뽑고,
  {$project:{"kor":1}},
  #kor만 출력시켜서, {_id:"..", kor:""}
  {$group:{"_id":"test","average":{"$avg":"$kor"}}
  #집계해서 {_id:"test", average:n}으로 출력한다.
)

```



<img src="C:\Users\dywjd\AppData\Roaming\Typora\typora-user-images\image-20200826142811204.png" alt="image-20200826142811204"  />



- stage1의 결과값으로 stage2d의 입력값으로 넣어준다.


 

```mysql
 #국어점수가 80점 이상인 애들을 출력해줄건데,  아이디는 test값으로 주고 average 라는 필드값에 평균을 내서 출력해주라 (stage1에서 뽑힌 애들의 kor평균값)
db.score.aggregate(

	{$match:{kor:{$gte:80}}},
	{$project:{kor:1}},
	{$group:{_id:'test',average: {$avg:'$kor'}}}

)

== select test, avg(kor) as average
from score
where kor > =80
#전체 다 그룹이라서 group by 없음


```

```mysql
#test가 midterm인 document들 중 
#국어점수랑 test만 가져와서 국어점수의 평균을 test라는 이름으로 출력하자 
#(_id의 이름은 test의 값들로 한다. -> midterm / final)

db.score.aggregate(

	{$match:{test:"midterm"}},
	{$project:{kor:1, test:1}},
	{$group:{_id:'$test', average:{$avg:'$kor'}}}

)
```



## 🥑map reduce

> **aggregation framework가 처리하지 못하는 복잡한 집계작업에 사용**
>
> **javascript function 을 사용하여 복잡한 작업처리**
>
> **shard에 대응 -> 분산처리가능**

##### 🍌순서 : query -> map -> reduce -> out

-map : data mapping(grouping)

-reduce : 집계연산실행

-query : 입력될 document

-out : collection or document 출력

![image-20200826234838919](C:\Users\dywjd\AppData\Roaming\Typora\typora-user-images\image-20200826234838919.png)





















