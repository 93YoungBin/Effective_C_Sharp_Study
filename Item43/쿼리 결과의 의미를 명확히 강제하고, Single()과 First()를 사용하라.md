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
- Single과 비슷하나, 가장 첫 번째 것을 반환한다
- 첫 번째것을 반환하기 때문에 Single과 달리 여러개의 값이 있어도 정상 동작한다.
```
var answer = (from p in Forwards
              where p.GoalsScored > 0
              orderby p.GoalsScored
              select p).FirstOrDefault();

var answer2 = (from p in Forwards
              where p.GoalsScored > 0
              orderby p.GoalsScored
              select p).First();
```

- 만약 n번째의 데이터를 가져오고 싶다면 Skip 을 사용하여 받아올 수 있다.

  ```
  var answer =  (from p in Forwards
              where p.GoalsScored > 0
              orderby p.GoalsScored
              select p).Skip(2).First();
  ```

- 해당 방식을 사용하지 않고 Sort를 사용하여 정렬 후 원하는 값을 뽑아내는 방법 도 있다
- Take 대신 First를 사용한 이유는 정확히 1개의 요소를 가져오려는 의도를 명확하게 하기 위함이다.
- 쿼리를 구성하기 위한 다른 속성이 있는지, List<T>를 지원하는지, 인덱스 작업을 지원하는지를 확인 후 코드를 구성하는편이 좋다.

정리
- Single 은 정확히 한 개의 값만 존재해야한다는 뜻
- SingleOrDefault는 요소가 없거나 한 개만 존재한다는 뜻
- First나 Last는 시퀀스 중 1개만 가져오려 할 때 필요
