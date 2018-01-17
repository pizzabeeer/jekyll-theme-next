---
title: 함수형으로 전환하기[함수형 프로그래밍]
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
