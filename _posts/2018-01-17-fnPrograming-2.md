---
title: [함수형 프로그래밍]함수형으로 전환하기
categories:
- JS
---
# 함수형으로 전환하기<br/>
## map과 filter<br/>
```javascript
var users = [
	{ id : 1, name : 'ID', age : 36 },
    { id : 2, name : 'BJ', age : 32 },
    { id : 3, name : 'JM', age : 32 },
    { id : 4, name : 'PJ', age : 27 },
    { id : 5, name : 'HA', age : 25 },
    { id : 6, name : 'JE', age : 26 },
    { id : 7, name : 'JI', age : 31 },
    { id : 8, name : 'MP', age : 23 }
];

// 1. 명령형 코드
//	1-1. 30세 이상인 users를 거른다.
var temp_users = [];
for(var i =0; i<users.length; i++) {
	if(user[i].age >= 30) {
    	temp_user.push(user[i]);
    }
}

//	1-2. 30세 이상인 users의 names를 수집한다.
var names = [];
for(var i =0; i<users.length; i++) {
	names.push(users[i].name);
}

//	1-3. 30세 미만인 users를 거른다.
var temp_users = [];
for(var i =0; i<users.length; i++) {
	if(user[i].age < 30) {
    	temp_user.push(user[i]);
    }
}

//	1-4. 30세 미만인 users의 names를 수집한다.
var names = [];
for(var i =0; i<users.length; i++) {
	names.push(users[i].name);
}


```

- 일급함수 : 함수를 값으로 다룰 수 있다.

```js
/* 일급함수 */

//변수에 함수를 담을 수 있다.
var f1 = function(a) { return a * b };

//함수를 인자로 받아 return 한다.
function f3(f) {
	return f();
}

console.log( f3(function() { return 10;}) ); //10 : f3에 함수를 넘겼다.
console.log( f3(function() { return 20;}) ); //20 : f3에 함수를 넘겼다.

/* add_maker */

//함수를 리턴하는 함수
function add_maker(a) {
	return function(b) {
		return a + b;
	}
}

var add10 = add_maker(10);
console.log( add10(20)); // 30

//함수를 인자로 받아 사용자가 평가시점에 사용할 수 있다.
function f4(f1, f2, f3) {
	return f3(f1() + f2());
}

console.log(
	f4(
		function() { return 2; },
    	function() { return 1; }
    	function(a) { return a * a }
	)
);	// 9
```

- 함수형 프로그래밍은 애플리케이션, 함수의 구성요소, 더 나아가서 언어 자체를 함수처럼 여기도록 만들고, 이러한 함수 개념을 가장 우선순위에 놓는다.<br/>
<blockquote>"함수형 사고방식은 문제의 해결 방법을 동사(함수)들로 구성(조합) 하는 것."
<br/>
마이클 포커스[클로저 프로그래밍의 즐거움]
</blockquote>
