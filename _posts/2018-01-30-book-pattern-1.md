---
title: 전략 패턴[개발자가 정복해야 할 객체지향]
categories:
- BOOK/JAVA
---
# 전략(Strategy) 패턴<br/>
- 한 과일 매장은 상황에 따라 다른 가격할인 정책을 적용한다.<br/>
- '첫 손님 할인'정책과 '덜 신선한 과일 할인' 정책이 있다.

```java
//가격 계산 모듈에 할인 정책 구현이 포함된 코드
public class Calclator {
	public int calculate(boolean firstGuest, List<Item> items) {
    	int sum = 0;
        for(Item item : items) {
        	if(firstGuest)
            	sum += (int) (item.getPrice() * 0.9); //첫 손님 10%할인
			else if(!item.isFresh())
            	sum += (int) (item.getPrice() * 0.8); //덜 신선한 것 20%할인
            else
            	sum += item.getPrice();
        }
        return sum;
	}
}
```

- 위 코드드는 다음의 문제를 포함하고 있다.<br/>
	- 서로 다른 계산 정책들이 한 코드에 섞여 있어, 정책이 추가될수록 코드 분석을 어렵게 만든다.<br/>
	- 가격정책이 추가될 때마다 calculate 메서드를 수정하는 것이 점점 어려워진다.<br/>

- 이런 문제를 해결하기 위한 방법중 하나는 가격할인 정책을 별도 객체로 분리하는 것이다.


```java
public interface DiscountStrategy {
	int getDiscountPrice(Item item);
}
```

```java
public class Calculator {
	//상품의 할인 금액 계산을 추상화
	private DiscountStrategy discountStrategy;
    
    //생성자를 통해서 사용할 전략 객체를 전달.
    public Calculator(DiscountStrategy discountStrategy) {
    	this.discountStrategy = discountStrategy;
    }
    
    //각 Item의 가격을 계산할 때 전략 객체를 사용.
    public int calculate(List<Item> items) {
    	int sum = 0;
        for(Item item : items) {
        	sum += discountStrategy.getDiscountPrice(item);
        }
        return sum;
    }
}
```

- 각 아이템 별로 할인 정책이 있고 전체 금액에 대한 할인 정책이 별도로 필요할 때

```java
public interface DiscountStrategy {
	int getDiscountPrice(Item item);
    int getDiscountPrice(int totalPrice); //전체 금액 할인정책 메서드 추가
}
```

- 전략 패턴에서 콘텍스트는 사용할 전략을 직접 선택하지 않는다.<br/>
- 콘텍스트의 클라이언트가 콘텍스트에 사용할 전략 DI를 이용하여 전달해 준다.

```java
//첫 번째 손님에 대해 할인해주는 구현 클래스
public class FirstGuestDiscountStrategy implements DiscountStrategy {

	@Override
    public int getDiscountPrice(Item item) {
    	return (int) (item.getPrice() * 0.9);
    }
}
````

```java
private DiscountStrategy strategy;

//첫 손님 할인 버튼 누를때 생성됨.
public void onFirstGuestButtonClick() {
	strategy = new FirstGuestDiscountStrategy();
}

//계산 버튼 누를 때 실행됨
public void onCalculationButtonClick() {
	Calculator cal = new Calculator(strategy);
    int price = cal.calculate(item);
    //...
}
```

- 위 코드에서 클라이언트가 첫 손님 할인 버튼을 눌러야 객체를 생성하는 것을 알 수 있다.<br/>
- 전략 패턴을 적용함으로써 Calculator 클래스는 할인 정책 확정에는 열려 있고 변경에는 닫혀 있게 된다.<br/>
- 일반적으로 if-else로 구성된 코드 블록이 비삿한 기능을 수행하는 경우 전략패턴을 적용함으로써 코드를 확장 가능하도록 변경할 수 있다.