---
title: 템플릿 메서드 패턴[개발자가 정복해야 할 객체지향]
categories:
- BOOK/JAVA
---

# 템플릿 메서드 패턴<br/>
- 프로그램을 구현하다 보면, 완전히 동일한 절차를 가진 코드를 작성하게 될 때가 있다.<br/>
- 실행과정이 동일한 두 코드


```java
public class DbAuthenticator {

	/* [사용자 정보 인증 절차(실행과정이 같음) ] */
	public Auth authenticate(String id, String pw) {
    	//사용자 정보로 인증확인
        User use = userDao.selectById(id);
        boolean auth = user.qualPassword(pw);
        //인증 실패시 익셉션
        if( !auth )
        	throw createException();
		//인증 성공시 인증정보 제공
        return new Auth(id, user.getName());
    }
}
```

```java
public class LdapAuthenticator {

	/* [사용자 정보 인증 절차(실행과정이 같음) ] */
	public Auth authenticate(String id, String pw) {
    	//사용자 정보로 인증확인
        boolean lauth = ldapClient.authenticate(id, pw);
        //인증 실패시 익셉션
        if( !auth )
        	throw createException();
		//인증 성공시 인증정보 제공
        LdapContext ctx = ldapClient.find(ud);
        return new Auth(id, ctx.getAttribute('name'));
    }
}
```

- 실행 과정/단계는 동일한데 각 단계 중 일부의 구현이 다른 경우에 사용할 수 있는 패턴<br/>
- 템플릿 메서드 패턴의 두가지 구성<br/>
	- 실행과정을 구현한 상위 클래스<br/>
	- 실행 과정의 일부 단계를 구현한 하위 클래스<br/>

- 상위 클래스는 실행과정을 구현한 메서드를 제공한다.<br/>
- 이 메서드는 기능을 구현하는데 필요한 각 단계를 정의하며 이 중 일부 단계는 추상 메서드를 호출하는 방식으로 구현된다.<br/>

```java
//위 코드 사용자 인증과정을 템플릿 메서드를 적용하여 상위클래스 작성한 것
public abstract Authenticator {
	//템플릿 메서드
    public Auth authenticate(String id, String pw) {
    	if( !doAuthenticate(id, pw) )
        	throw createException();
		return createAuth(id);
    }
    
    protected abstract boolean doAuthenticate(String id, String pw);
    
    private RuntimeException createException() {
    	throw new AuthException();
    }
    
    protected abstract Auth createAuth(String id);
}
```

- authenticate() 메서드는 DbAuthenticator와 LdapAuthenticator에서 동일했던 실행 과정을 구현하고 있고, 두 클래스에서 차이가 나는 부분은 별도의 추상 메서드로 분리하였다.<br/>
- Authenticator클래스를 상속받은 클래스에서 알맞게 재정의한다.<br/>

```java
//알맞게 재정의한 클래스
public class LdapAuthenticator extends Authenticator {
	@Override
    protected boolean doAuthenticate(String id, String pw) {
    	return ldapClient.authenticate(id, pw);
    }
    
    @Override
    protected Auth createAuth(String id) {
    	LdapContext ctx = ldapClient.find(id);
        return new Auth(ud, ctx.getAttribute('name'));
    }
}
```

- 동일한 실행 과정의 구현을 제공하면서 동시에 하위타입에서 일부 단계를 구현하도록 할 수 있다.<br/>
- 코드 중복 문제를 제거하면서 동시에 코드를 재사용할 수 있게 된다.<br/>

## 상위 클래스가 흐름 제어 주체<br/>
- 템플릿 메서드 패턴은 상위 클래스에서 흐름을 제어한다.<br/>
- 일반적인 경우 하위타입이 상위 타입의 기능을 재사용할지 여부를 결정하기 때문에, 흐름 제어를 하위 타입이 하게 된다.<br/>

```java
//turnOn()메서드는 상위 클래스의 turnOn()메서드를 재사용할지 여부를 자신이 결정한다.
public class SuperCar extends ZetEngine {
	@Override
    public void turnOn(0 {
    	//하위 클래스에서 흐름 제어
        if(notReady)
        	beep();
		else
        	super.turnOn();
    })
}
```

- 템플릿 메서드 패턴에서는 상위 타압의 템플릿 메서드가 모든 실행 흐름을 제어하고, 하위 타입의 메서드는 템플릿 메서드에서 호출되는 구조를 갖게 된다.<br/>
- 템플릿 메서드에서 호출하는 메서드를 추상 메서드로 정의했는데, 기본 구현을 제공하고 하위 클래스에서 알맞게 재정의하도록 구현할 수도 있다.<br/>

```java
//안드로이드에서 비동기 처리를 위한 기능을 제공하는 AsyncTack 클래스
//doInBackground() 추상 메서드와 빈 구현을 갖는 onPreExecute()메서드를 제공
public abstract class AsyncYask<Params, Progress, Result> {
	public AsyncTask() {
    	mWorker = new WorkerRunnable<Params, Result> () {
        	public Result call() throws Exception {
            	mTaskInvoked.set(true);
                Process.setThreadPriority(Process.THREAD_PRIORITY_BACKGROUD);
                return postResult(doInBackground(mParams));
            }
        };
        //...
    }
    
    public final AsyncTask<Params, Progress, Result> executeOnExecutor(
    	Excutor exec, Params... params) {
        //...
        mStatus = Status.RUNNING;
        onPreExecute();
        mWorker.mParams = params;
        exec.execute(mFuture);
        return this;
    }
    
    protected abstract Result doInBackground(Params... params);
    
    protected void onPreExecute() { //하위 클래스의 확정 지점
    	//.... 기타 다른 코드
    }
}
```

- AsyncTask를 상속받아 구현하는 클래스는 doInbackground()메서드는 반드시 구현해 주어야 하지만, onPreExecute() 메서드의 경우는 필요한 경우에만 구현해 주면 된다.<br/>
- 상위 클래스에서 실행 시점이 제어되고, 기본 구현을 제공하면서, 하위 클래스에서 알맞게 확장할 수 있는 메서드를 훅(hook) 메서드라고 부른다.<br/>

## 템플릿 메서드와 전략 패턴의 조합<br/>
- 템플릿 메서드와 전략 패턴을 함께 사용하면 상속이 아닌 조립의 방식으로 템플릿 메서드 패턴을 활용할 수 있다.<br/>

```java
//예시로는 스프링 프레임워크의 Template으로 끝나는 클래스들이 있다.
//작성된 코드는 트렌잭션기능을 제공하는 TransactionTemplate클래스
public <T> T execute(TransactionCallback<T> action) throws TransactionExcption {
	//일부 코드 생략
    TransactionStatus status = this.transactionManager.getTransaction(this);
    T result;
    
    try {
    	result = action.doInTransaction(status);
    }
    catch (RuntimeExceiption ex) {
    	rollbackOnExcption(status, ex);
        throw ex;
    }
    //...다른 익셉션 처리
    this.transactionManager.commit(status);
    return result;
}
```

- execute()메서드는 트랜젝션의 시작/커밋/롤백 등의 실행 흐름을 제공하는 템플릿 메서드.<br/>
- 앞서 살펴본 템플릿 메서드와 다른점<br/>
	- 앞서 템플릿 메서드가 하위 타입에서 재정의할 메서드를 호출하고 있다면
	- TransactionTemplate의 execute()메서드는 파라미터로 전달받은 action의 메서드를 호출하고 있다.<br/>

```java
//execute() 메서드를 사용하는 코드는 메서드를 호출할 때 원하는 기능을 구현한 객체를 전달.
transactionTemplate.execute(new TransactionCallback<String>() {
	public String doInTransaction(TransactionStatus status) {
    	//트렌젝션 범위 안에서 실행될 코드
    }
});
```

- 템플릿 메서드 패턴과 전략 패턴을 조합하게 되면, 상속에 기반을 둔 템프릿 메서드 구현과 비교해서 유연함을 갖는다.
