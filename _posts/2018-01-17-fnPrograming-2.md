---
title: 함수형으로 전환하기[함수형 프로그래밍]
categories:
- JS
---
# 함수형으로 전환하기<br/>

## map과 filter<br/>

```js
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

// 2. _filter, _map으로 리펙토링

// 추상화의 단위를 객체나 함수를 이용한다.
function _filter(list, predi) {
	var new_liet = [];
	for(var i =0; i<list.length; i++) {
    	// 변경이 많은 부분을 함수로 변경
		if(predi(list[i])) {
    		new_liet.push(list[i]);
    	}
	}
	ruetnr new_list;
}

_filter(users, function(user) { return user.age >= 30; } );


// 다형성이 높고 data가 어떤 형식인지 알지 못한다.
function _map(list, mapper) {
	var new_list = [];
    for(var i=0; i<list.length; i++) {
    	new_list.push(mapper[i]);
    }
    return new_list;
}

_map(users, function(user) { return user.naem } );

// 함수형 프로그래밍은 대입문을 잘 사용하지 않는다.
// 중간에 변경할 여지가 없기 때문에 안정성이 높다.
_map(
	_filter(users, function(user) { return user.age >= 30; } ),
	, function(user) { return user.naem }
);

// 3. each만들기
```
