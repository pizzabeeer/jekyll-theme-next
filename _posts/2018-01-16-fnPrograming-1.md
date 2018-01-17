---
title: [함수형 프로그래밍]순수함수와 일급함수
categories:
- JS
---
# 함수형 프로그래밍<br/>
## 정의<br/>
- 성공적인 프로그래밍을 위해 부수 효과를 미워하고 조합성을 강조하는 프로그래밍 패러다임.<br/>
- 부수 효과를 미워한다. -> 순수 함수를 만든다.<br/>
- 조합성을 강조한다. -> 모듈화 수준을 높인다.<br/>
- 순수 함수 -> 오류를 줄이고 잔정성을 높인다.<br/>
- 모듈화 수준이 높다 -> 생산성을 높인다.<br/>

```javascript
/* 순수함수 */

//항상 동일한 인자를 주면 동일한 값을 리턴한다.
function add(a,b) {
	return a + b;
}

//프로그래밍에 있어 c가 변경될 위험이 있으면 add2는 순수함수가 아니다.
//c가 상수일 경우는 순수함수
var c = 10;
function add2(a, b) {
	return a + b + c;
}

//부수효과가 없어야 한다.(외부의 인자가 함수 안에서 변경되면 안된다.)
var c = 20;
function add3() {
	c = b; //부수효과
    return a + b;
}

var obj1 = { val : 10 }
function add4(obj, b) {
	obj.val += b; //부수효과 : obj의 val값을 변경한다.
}

//add4를 순수함수로 만든 것
var obj1 = { val : 10 }
function add5(obj, b) {
	return { val : obj.val + b } //obj.val을 참조만 할 뿐 변경하지 않는다.
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
