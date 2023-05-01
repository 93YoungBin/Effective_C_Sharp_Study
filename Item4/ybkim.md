# Effective c# 

# 아이템 4: string.Format()을 보간 문자열로 대체하라

## 보간 문자열 이란?
- C# 6.0부터 지원되는 문자열 표현 방식
- 기존 문자열 포매팅 방식 및 복합 형식 지정에 비해서 가독성이 높음

## 보간 문자열의 사용법
- 문자열 리터럴("") 앞에 $ 기호를 붙이며, 사이에 공백이 없어야 함
- 제어 흐름을 변경하는 코드 (if/while 등)는 사용할 수 없고 변수 및 함수 return등의 Data만 사용할 수 있다.
- 문자열로 변경 할 표현식은 { } 사이에 입력하며 형식에 맞춰서 사용할 수 있다.
- 표현식의 형식은 ```{<interpolationExpression>[,<alignment>][:<formatString>]}```를 기준으로 한다.

<img scr="./photo1.png">

## 보간 문자열의 Boxing / UnBoxing 방지
