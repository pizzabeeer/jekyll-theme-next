---
title: 양방향 관계 엔티티 사이클 방지 @JsonIdentityInfo
categories:
- JAVA
---

회사 프로젝트 중 N:M관계로 @OneToMany, @ManyToOne 양방향 관계 설정 후, 데이터를 불러오는 과정에서 무한재귀로 데이터를 불러와서 StackOverFlow가 되는 현상이 발생하였다.

```java

//User.class
@OneToMany(cascade = {CascadeType.PERSIST}, mappedBy = "user")
public Collection<UserGroup> getUserShareGroupLists() {
  return userShareGroupLists;
}


//UserGroup.class
@Id
@ManyToOne
@JoinColumn(name = "user_id")
public User getUser() {
  return shareGroup;
}
```

위 코드로 이제 User의 getUserShareGroupLists()를 호출할 경우, UserGroup클래스를 추상화하면서 User를 다시 추상화되고, 그  User의 userShareGroupLists를 다시 생성하면서 무한재귀가 발생한다.
<br/><br/>
그래서 검색해보니 생각보다 많은 사람들이 겪는 현상이였다. 해결책중에 하나를 발견했는데, 바로 @JsonIdentityInfo이였다. 자세한 레퍼런스는 추후 확인을 해봐야하겠지만 기본적으로 JSON 타입으로 엔티티를 변환할때 @Id를 바탕으로 중복된 아이디는 JSON으로 변환시키지 않는 기능을 한다.

```java

//User.class
@OneToMany(cascade = {CascadeType.PERSIST}, mappedBy = "user")
public Collection<UserGroup> getUserShareGroupLists() {
  return userShareGroupLists;
}


//UserGroup.class
@Id
@ManyToOne
@JoinColumn(name = "user_id")
@JsonIdentityInfo(generator = ObjectIdGenerators.IntSequenceGenerator.class)
public User getUser() {
  return shareGroup;
}

//property를 지정해주지 않을 경우 기본으로 @id 어노테이션으로 기능을 실행한다.
```

일단 중복된 현상은 해결했지만, 처음보는 어노테이션이고 현 프로젝트에 적합한지 검증을 하지 않았기때문에 기록해두고 좀 더 알아보도록 해야겠다.