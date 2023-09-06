# Effective c# 

# 아이템 39: 쿼리 결과의 의미를 명확히 강제하고, Single()과 First()를 사용하라

- 보통은 Linq를 이용하여 여러 데이터 세트를 제어하거나 반환하는데 사용하곤 한다.
- 하지만 Linq에도 단일값을 반환하는 메서드를 지원한다.

- 한 개를 반환하는 요소로는 Single과 First가 있다.

### Single
- 단 하나만 반환해야 한다는 메서드
- 이러한 동작이 틀릴 경우 즉각적으로 확인하여 데이터 손상이 발생하는것을 방지할 수 있다.

```
var somePeople = new List<Person> {
  new Person { FirstName = "Bill", LastName = "Gates"},
  new Person { FirstName = "Bill", LastName = "Wagner"},
  new Person { FirstName = "Bill", LastName = "Johnson"}
};

// 시퀀스 내에 여러 개의 요소가 포함되므로 예외 발생
var answer = (from p in somePeople
              where p.FirstName == "Bill"
              select p).Single();

// null 이 반환되더라도 예외 발생
var answer = (from p in somePeople
              where p.FirstName == "Larry"
              select p).Single();
```

만약 하나이거나 해당되지 않아 null을 반환 받고 싶으면 SingleOrDefault() 를 사용할 수 있다.

### First
- 
