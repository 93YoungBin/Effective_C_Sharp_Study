# Effective c# 

# 아이템 4: string.Format()을 보간 문자열로 대체하라

## 보간 문자열 이란?
- C# 6.0부터 지원되는 문자열 표현 방식
- 기존 문자열 포매팅 방식 및 복합 형식 지정에 비해서 가독성이 높음

![photo1](https://user-images.githubusercontent.com/32691585/235422719-121bd509-4cc0-4210-8d9d-828b9c98f631.png)

## 보간 문자열의 사용법
- 문자열 리터럴("") 앞에 $ 기호를 붙이며, 사이에 공백이 없어야 함
- 제어 흐름을 변경하는 코드 (if/while 등)는 사용할 수 없고 변수 및 함수 return등의 Data만 사용할 수 있다.
- 문자열로 변경 할 표현식은 { } 사이에 입력하며 형식에 맞춰서 사용할 수 있다.
- 표현식의 형식은 ```{<interpolationExpression>[,<alignment>][:<formatString>]}```를 기준으로 한다.

![image](https://user-images.githubusercontent.com/32691585/235424905-6fe53dc5-cf3e-4346-af70-22c809c5375d.png)

- 삼항연산자 및 조건 표현식을 바로 사용할 수 없음

![image](https://user-images.githubusercontent.com/32691585/235424623-dc220e84-cb27-46ea-bf92-147206663ab5.png)

- 괄호 ()표시로 해당 표현식을 감싸서 사용할 수 있음

![image](https://user-images.githubusercontent.com/32691585/235425037-b3cdb27d-2bef-4ac6-9928-eb065115bae0.png)


## 보간 문자열의 Boxing / UnBoxing 방지
- 문자열 보간 기능을 사용하더라도 컴파일러는 object 배열을 전달하여 포매팅 함수를 호출함
- 문자열 보간에 포함 되는 Data를 toString()으로 string으로 변경하면 박싱 / 언박싱을 줄일 수 있음.

```Console.WriteLine($"파이는 {Math.PI} 입니다.");```

↓

```Console.WriteLine($"파이는 {Math.PI.ToString()} 입니다.");```

- 보간 문자열의 표현식은 중첩해서 사용할 수 있다.

![image](https://user-images.githubusercontent.com/32691585/235426180-27bff52e-7911-4504-85db-27e37ef5c313.png)

- 가독성이 나빠질 수 있어 주의가 필요함

- Linq에서도 사용할 수 있음

![image](https://user-images.githubusercontent.com/32691585/235426911-9487b5ad-3edf-44d7-9640-24772b7b4ba9.png)

- C# 11 이후부터는 원시 문자열을 포함한 문자열 보간 코드를 사용할 수 있습니다

```
int X = 2;
int Y = 3;

var pointMessage = $$"""The point {{{X}}, {{Y}}} is {{Math.Sqrt(X * X + Y * Y)}} from the origin""";
Console.WriteLine(pointMessage);
// output:  The point {2, 3} is 3.605551275463989 from the origin.
```
