# Effective c# 

# 아이템 39: function과 action 내에서는 예외가 발생하지 않도록 하라

- 일련의 값을 순차적으로 처리하는 코드가 중간에서 예외를 일으키면 상태를 복구할 수 없다.

```
var allEmployees = FinmdAllEmployees();
allEmployees.ForEach(e => e.MonthlySalary *= 1.05M);
```

해당 코드가 중간에서 멈추었을 경우, 어디서 멈추었고 어디까지 적용되었는지 파악할 수 없어 곤란해지는 상황이 오게 된다.

해당 문제는 코드가 시퀀스 내에 포함된 값을 직접 수정하기 때문에 발생한다.

해당 문제를 해결하려면 총 두가지로, 
- 절대 예외를 발생시키지 않거나
- 완전히 완료하기 전까지는 프로그램의 상태가 변경되지 않도록 하도록 해야 한다.
  
```
var updates = (from e in allEmployees
               select new Employee
               {
                   EmployeeID = e.EmployeeID,
                   Classification = e.Classification,
                   YearsOfService = e.YearsOfService,
                   MonthlySalary = e.MonthlySalary *= 1.05M
               }).ToList();
```
위와 같이 기존 값을 건드리지 않고, 코드의 루틴이 다 돌아갈 경우 목록을 교체해주는 방식을 사용하면 중간에 멈추더라도 문제가 없을 것으로 보이지만
이 방식이 무조건 적으로 좋은 것은 아니다.

해당 방식의 단점
- 코드가 길어져 관리해야할 코드 양이 증가
- 쿼리 외부에서 예외가 발생할 수 있어 추가적인 확인이 필요
- 성능 저하의 가능성이 있음
- 해당 함수를 다른 함수와 조합하여 사용하기 어려워짐 (해당 방식의 경우 여러 작업 후에 마지막에 데이터를 교체하는 형식으로 보완할 수 있음)

결론
- 람다를 사용하는 경우 내부에서 예외가 발생할 경우 원인을 규명하기 어렵다
- 해당 방식 및 멀티스레드 환경에서 해당 사항을 보완하기 위해 직접적인 값을 바꾸기보다 복사본에 작업을 수행하여 교체하는 방식을 사용하자.
