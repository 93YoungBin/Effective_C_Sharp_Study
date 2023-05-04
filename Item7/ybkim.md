# Effective c# 

# 아이템 7: 델리게이트를 이용하여 콜백을 표현하라

## 콜백 (CallBack)?
- 콜백은 Method를 인자로 캐싱하여 해당 함수를 나중에 필요한 타이밍에 호출하는것을 의미
- 비동기 처리 및 이벤트 기반 코드작성에 사용


## 델리게이트 (Delegate)
- 델리게이트는 함수 참조 타입으로 함수를 변수처럼 다룰 수 있도록 제공하는 기능
- 런타임 시점에서 동적으로 콜백에 사용할 함수를 결정하고 호출할 수 있음
- 해당 델리게이트를 다른 클래스나 어셈블리에서 사용할 수 있어 이식성이 좋아짐

## C# 델리게이트의 종류

### Predicate<T>
 - 'T'형식의 객체를 인자로 받아 bool Type으로 반환해 주는 Delegate
  
```
static bool IsEven(int x)
{
    return x % 2 == 0;
}

static void Main(string[] args)
{
    int[] numbers = { 1, 2, 3, 4, 5 };
    var evenNumbers = Array.FindAll(numbers, IsEven);
    foreach (var n in evenNumbers)
    {
        Console.WriteLine(n);
    }
}
```
### Action<T>
 -'T'형식의 객체를 인자로 받아, 반환값이 없이 실행되는 Method를 참조하는 Delegate
```
 static void PrintNumber(int x)
{
    Console.WriteLine(x);
}

static void Main(string[] args)
{
    int[] numbers = { 1, 2, 3, 4, 5 };
    Array.ForEach(numbers, PrintNumber);
}
```
### Function<T,TResult>
 -'T'형식의 객체를 인자로 받고 TResult 형식의 값을 반환하는 Method를 참조하는 Delegate
 - bool 형식을 Return하는 Method의 경우 Predicate<T>와 동일한 구조
```
  static bool IsPositive(int x)
{
    return x > 0;
}

static void Main(string[] args)
{
    Func<int, bool> isPositive = IsPositive;
    bool result = isPositive(10); // true
    Console.WriteLine(result);
    result = isPositive(-10); // false
    Console.WriteLine(result);
}
  ```
  
## Linq Delegate
 - LINQ는 콜백을 사용하여 특정 데이터를 필터링 하여 호출하거나, 정렬 / 목록화 하는 기능을 사용한다.
```
  using System;
using System.Collections.Generic;
using System.Linq;

class Program
{
    static void Main(string[] args)
    {
        List<int> numbers = new List<int> { 5, 2, 8, 3, 7, 1, 4, 6 };

        // Where 메서드로 조건에 맞는 요소만 필터링 - Predicate<T> 인수로 사용
        var filteredNumbers = numbers.Where(n => n > 3);

        // OrderBy 메서드로 오름차순 정렬 - Comparison<T> 인수로 사용
        var sortedNumbers = squaredNumbers.OrderBy(n => n);

        // OrderByDescending 메서드로 내림차순 정렬 - Comparison<T> 인수로 사용
        var reversedNumbers = sortedNumbers.OrderByDescending(n => n);

        // GroupBy 메서드로 요소를 그룹화 - Func<T, TKey> 인수로 사용
        var groups = numbers.GroupBy(n => n % 2 == 0 ? "Even" : "Odd");

        // Find 메서드로 조건에 맞는 첫 번째 요소 찾기 - Predicate<T> 인수로 사용
        var firstEvenNumber = numbers.Find(n => n % 2 == 0);

        // TrueForAll 메서드로 모든 요소가 조건을 만족하는지 검사 - 개별 항목에 대해 Predicate<T> 처럼 True값 
        var allNumbersGreaterThanOne = numbers.TrueForAll(n => n > 1);

        // ForEach 메서드로 모든 요소에 대해 작업 수행 -  Action<T> 인수로 
        squaredNumbers.ForEach(n => Console.WriteLine(n));
    }
}
  ```

 ## Delegate Multicast
  - 하나의 이벤트에 대해 여러 개의 메서드를 등록할 수 있는 기능
  - 코드의 유연성이 높아지나, 예외처리 및 메모리 누수등의 이슈가 발생할 수 있음
 
  - 델리게이트의 반환값은 마지막 호출 함수의 반환값이 된다
 ```
 delegate int Calculator(int x, int y);

class Program
{
    static int Add(int x, int y)
    {
        Console.WriteLine($"Adding {x} and {y}");
        return x + y;
    }

    static int Multiply(int x, int y)
    {
        Console.WriteLine($"Multiplying {x} and {y}");
        return x * y;
    }

    static void Main(string[] args)
    {
        Calculator calc = Add;
        calc += Multiply;

        int result = calc(3, 4);
        
        Console.WriteLine($"Result = {result}");
        //result = 12
    }
}
 ```
 
 ```
 public void LegthyOperation(Func<bool> pred)
 {
    foreach (ComplicatedClass cl in container)
    {
       cl.DoLegthyOperation();
       if(pred == false)
       return;
    }
  }
 ```
 멀티캐스트로 사용 시 에러 발생
 
 ```
 Func<bool> cp = () => CheckWithUser();
 cp += () => CheckWithSystem();
 c.LegthyOperation(cp);
 // 해당 경우 CheckWithUser가 false더라도 CheckWithSystem이 True면 정상 작동
 ```
 
 ```
 public void LegthyOperation(Func<bool> pred)
 {
    bool 
    foreach (ComplicatedClass cl in container)
    {
       cl.DoLegthyOperation();
       if(pred == false)
       return;
    }
  }
