---
layout: post
title:  "Inside Javascript"
date:   2016-02-02 10:00:00
categories: front-end
tags: front-end, javascript
---

Javascript.<br>
멋모르고 사용 중인 언어인데 몇일전 쇼크받은 일이 있어서 다시 공부하기 시작한다.<br>
내 포스트야 항상 그렇지만 잊어먹음 대처용 포스트다.

## Javscript Data Types

Javascript Types

1. 기본 타입
	1. Number
	1. String
	1. Boolean
	1. undefined
	1. null
1. 참조 타입
	1. Object
		1. Array
		1. Function
		1. Regular Expression

## 문자열

문자배열로 접근 가능

{% highlight Javascript %}
var str = 'test';
console.log(str[0]);	// (output) t
{% endhighlight %}

문자배열로 수정은 안됨

```Javascript
str[0] = 'T';
console.log(str[0]);	// (output) t
```

## null과 undefined

`null`의 type은 `object`

```Javascript
var strUnd;
console.log(strUnd);		// (output) undefined
console.log(typeof strUnd);	// (output) undefined

var strNull = null;
console.log(strNull);		// (output) null
console.log(typeof strNull);// (output) object

console.log(typeof strNull === null);	// (output)	false
console.log(strNull === null);			// (output)	true
```

## 객체 생성

객체를 생성하는 세가지 방법

```
1. Object() 생성자 함수 이용
2. 객체 리터럴을 이용하는 방법
3. 생성자 함수를 이용하는 방법
```

> #### 1. Object() 생성자 함수 이용

>> ```Javascript
>> var foo = Object();
>> 
>> foo.name = 'foo';
>> ```

> #### 2. 객체 리터럴을 이용하는 방법

>> 객체를 생성하는 표기법

>> ```Javascript
>> var foo = {
>> 	name : 'foo'
>> };
>> ```

> #### 3. 생성자 함수를 이용하는 방법

## Property Control

#### Read Property

```Javascript
console.log(foo.name);
console.log(foo['name']);
```

#### Update Property

``` Javascript
foo['name'] = 'newfoo';
console.log(foo['name']);	// (output) newfoo
```

#### 대괄호 표기법만 사용해야 하는 경우

```Javascript
// 표현식이거나 예약어일 경우
foo['full-name'] = 'foo bar';	// '-' 연산자가 포함됨
```

#### Delete property

```Javascript
console.log(foo.name);	// (output) newfoo
delete foo.name;		// delete name property
console.log(foo.name);	// (output) undefined

foo.name = 'newfoo';
delete foo;				// delete foo object
console.log(foo.name);	// (output) newfoo
```

