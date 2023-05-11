# Effective c# 

# 아이템 10: 베이스 클래스가 업그레이드된 경우에만 new 한정자를 사용하라

## new 한정자?
- 부모 클래스에 있는 메서드를 숨기고 새로운 메서드를 정의하고자 할 때 사용
- virtual로 선언되지 않은 멤버를 재 정의 할 때 사용

```
public class Animal
{
    public virtual void MakeSound()
    {
        Debug.LogError("Animal is making a sound.");
    }
}

public class Dog : Animal
{
    public override void MakeSound()
    {
        Debug.LogError("Dog is barking.");
    }
}

public class Cat : Animal
{
    public new void MakeSound()
    {
        Debug.LogError("Cat is meowing.");
    }
}

void Start()
    {
        {
            Animal dog = new Dog();
            // Dog is barking.
            dog.MakeSound();

            Animal cat = new Cat();
            // Animal is making a sound.
            cat.MakeSound();

            Cat cat2 = new Cat();
            // Cat is meowing.
            cat2.MakeSound();
        }
    }

```
- 객체를 어떻게 참조하느냐에 따라 다르게 동작한다

비가상 메서드는 정적으로 바인딩되므로 런타임에서 메서드의 새로운 정의나 변경사항을 조회하지 않음
가상 메서드는 동적으로 바인딩 되므로 런타임에 객체의 타입이 무엇이냐에 따라 그에 부합하는 메서드를 호출합니다.

- 다형성을 고려해서 작업해야하는 메서드나 속성에만 가상 메서드를 사용하고, 그 외에는 new를 사용하는것이 좋다

## new를 사용해야 하는 Case

- 타 Plugin 혹은 Base Class를 상속받아 사용하고 있었으나, 해당 클래스가 Update되면서 Method에 충돌 되었을 경우 사용
```
public class newPlugin : BasePlugin
{
    public void PluginAction()
    {
        
    }
}

public class BasePlugin
{
    public void PluginAction()
    {
        
    }
}
```

↓

- 해결법 1 - 내가 생성한 Class의 이름을 변경한다
 ```
public class newPlugin : BasePlugin
{
    public void PluginNewAction()
    {
        
    }
}
```

- 해결법 2 - new 생성자를 사용한다
 ```
public class newPlugin : BasePlugin
{
    public new void PluginAction()
    {
        
    }
}
```

## 결론?
new를 남발해서 사용할 경우 메서드를 호출할 때 모호한 상황이 발생할 수 있음
개인적으로는 메서드명이 겹치더라도 웬만해서 new를 사용하지 않는것이 바람직하다고 생각함
