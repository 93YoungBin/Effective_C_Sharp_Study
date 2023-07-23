# Effective c# 

# 아이템 31: 시퀀스에 사용할 수 있는 조합 가능한 API를 작성하라

- 알고리즘을 처리하는 함수를 작성할 때 foreach , for , while등의 반복문을 사용하여 컬렉션을 제어하는 함수를 많이 사용한다.
- 해당 알고리즘은 보통 매개변수로 컬렉션을 받아와서 컬렉션에 포함 된 요소들을 확인 , 수정 , 필터링하여 리턴하는 방식을 사용하게 된다.
- 해당 방식은 한개의 원본 컬렉션에서 여러 단계의 작업을 수행 한 후에 결과물이 나오는 경우가 대부분이기 때문에 효율성 측면에서 문제가 있다.
(각 단계를 거칠때마다 중간 결과를 저장하게 되면 메모리 관리에서 효율적이지 못하며 한 개의 루프 내에서 수행되도록 할 경우 범용성이 떨어지게 된다)

## 이터레이터 메서드

- 단일의 시퀀스를 입력으로 취하고 결과로 단일의 시퀀스를 반환하는 메서드
- 해당 메서드를 수행할 때 yield return을 사용하면 별도의 저장소를 마련할 필요가 없음.
(값이 필요한 시점에 시퀀스 요소를 가져오고, 출력 결과가 필요한 시점에 출력 시퀀스로 결과를 보내기 때문)

### 이터레이터의 장점?
- 다양한 방식으로 조합하여 사용할 수 있어 재사용성이 좋아진다.
- 전체 시퀀스를 한 번만 순회하면 되므로 런타임 효율이 개선된다.
- 각 메서드가 구현하고 있는 알고리즘 조합 가능성도 높아진다.

```
public static void Unique(IEnumerable<int> nums)
{
    var uniqueVals = new HashSet<int>();

    foreach (var num in nums)
    {
        if(!uniqueVals.Contains(num))
        {
            uniqueVals.Add(num);
            Debug.Log(item);
        }
    }
}

//하단과 같은 방식으로 변경한다

public static IEnumerable<int> Unique(IEnumerable<int> nums)
{
    var uniqueVals = new HashSet<int>();

    foreach (var num in nums)
    {
        if(!uniqueVals.Contains(num))
        {
            uniqueVals.Add(num);
             yield return num;
        }
    }
}
```

```
    void Start()
    {
        List<int> myList = new List<int>() { 0,3,4,5,7,3,2,7,8,0,3,1,};

        foreach (var item in Unique(myList))
            Debug.Log(item);
    }

    public static IEnumerable<int> Unique(IEnumerable<int> nums)
    {
        var uniqueVals = new HashSet<int>();

        Debug.Log("enter Method");

        foreach (var num in nums)
        {
            Debug.Log($"evaluating {num}");

            if (!uniqueVals.Contains(num))
            {
                Debug.Log($"Add {num}");
                uniqueVals.Add(num);
   
                yield return num;
                Debug.Log($"reenter after yield return");
            }
        }
        Debug.Log("Exit Method");
    }
```

![image](https://github.com/93YoungBin/Effective_C_Sharp_Study/assets/32691585/b14cb9aa-e0ac-4dd3-ab64-0c30887fa136)
![image](https://github.com/93YoungBin/Effective_C_Sharp_Study/assets/32691585/e1f5abc3-2ed4-4fb4-9904-550033a6025d)

- 순회를 도는 시점에서야 각 단계가 수행된다.
- 값을 반환한 후 내붖거으로 사용하는 이터레이터의 현재 위치와 상태 정보를 저장한다.
- 해당 방식처럼 시퀀수의 위치를 갱신하가며 반환하는 함수를 컨티뉴어블 메서드라고 부른다.

- 시퀀스 내의 개별 요소에 대한 지연 평가 및 수행이 가능하다
- 메서드가 foreach 문을 타더라도 조합 가능한 메서드로 생성할 수 있다.
- 제네릭 메서드로 변경하기 용이하다

```
 void Start()
    {
        List<int> myList = new List<int>() { 0,3,4,5,7,3,2,7,8,0,3,1,};

           foreach (var item in Square(Unique(myList)))
        Debug.Log(item);
    }

    public static IEnumerable<int> Unique(IEnumerable<int> nums)
    {
        var uniqueVals = new HashSet<int>();

        Debug.Log("enter Method");

        foreach (var num in nums)
        {
            if (!uniqueVals.Contains(num))
            {
                Debug.Log($"Add {num}");
                uniqueVals.Add(num);
   
                yield return num;
                Debug.Log($"reenter after yield return");
            }
        }
    }

    public static IEnumerable<int> Square(IEnumerable<int> nums)
{
    foreach (var num in nums)
        yield return num * num;
}
```
- 여러개의 이터레이터 메서드를 조합하더라도 전체 시퀀스에 대한 순회는 단 한 번만 이루어진다.


```
public static IEnumerable<string> Zip(IEnumerable<string> first, IEnumerable<string> second)
{
    using(var firstSequence = first.GetEnumerator())
    {
        using(var secondSequence = second.GetEnumerator())
        {
            while(firstSequence.MoveNext() && secondSequence.MoveNext())
                yield return $"{firstSequence.Current} {secondSequence.Current}";
        }
    }
}
```

- 일반적으로 한 개의 입력 시퀀스를 입력하고 출력하지면 복수도 가능하다

이터레이터 메서드는 여러가지 효율적인 조합이 가능하도록 구상된 것이니 잘 활용하여 짜 보자
