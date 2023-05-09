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
