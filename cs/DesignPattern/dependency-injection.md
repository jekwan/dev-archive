# 의존성 주입

객체 지향에서 클래스는 종종 다른 클래스를 필요로 합니다. 예를 들어 `Car`라는 클래스는 움직이기 위해 `Engine`이라는 클래스를 필요로 합니다. 이 때, `Engine`과 같은 클래스를 의존성이라고 표현합니다.

`Car`가 `Engine`을 갖게하는 가장 직관적인 방법은 `Car`클래스가 직접 자신만의 `Engine`인스턴스를 생성하여 갖는 것이며 아래와 같은 형태를 보입니다.

```java
class Car {
  private Engine engine = new Engine();

  ...
}
```

위 방식은 몇가지 문제점을 야기합니다.

- `코드 재사용성`이 떨어집니다. 만약 의존성 클래스인 `Engine`이 세분화되어 `Gas`방식과 `Electric` 방식으로 나뉜다면, `Car`클래스 또한 `Gas`방식 엔진을 사용하는 클래스와 `Electric`방식 엔진을 사용하는 클래스로 나뉘어야 합니다. 만약 `Car` 클래스가 `Wheel`, `Color`등 더 많은 의존성을 갖는다면 훨씬 상황은 악화됩니다.

- `테스트`를 어렵게 만듭니다. `Car`의 동작이 `Engine`의 동작에 의존되기 때문에 `Engine`의 테스트 실패가 `Car`의 테스트 실패로 이어집니다. 따라서 `Car` 클래스만을 독립적으로 테스트하기 힘들어집니다.

개선된 코드는 아래와 같습니다.

---

## 생성자를 통한 주입

```java
class Car {
  private final Engine engine;

  public Car(Engine engine) {
    this.engine = engine;
  }

  ...
}
```

이제 `Car`클래스가 직접 의존성 클래스를 생성하지 않고, 외부로부터 주입 받습니다. 이러한 디자인 패턴을 `Dependency Injection(DI)`이라고 합니다. `Inversion Of Control`의 한 형태이며, 객체간 결합도를 느슨하게 만들어주기 때문에 위에서 언급된 여러 문제들을 해결할 수 있습니다.

- `Engine` 인터페이스를 따르기만 한다면, 이제 엔진이 `Gas`방식이던 `Electric`방식이던 같은 `Car`클래스를 사용할 수 있습니다.

- `Engine` 클래스는 외부에서 주입되기 때문에, 테스트시 간단한 목킹 엔진을 주입하여 `Car`클래스의 기능만 테스트할 수 있습니다.

---

## Setter를 통한 주입

```java
class Car {
  private Engine engine;

  public void setEngine(Engine engine) {
    this.engine = engine;
  }

  ...
}
```

위 코드는 생성자를 통해 `Engine`을 받는 대신 setter를 통해 주입 받습니다. 생성자를 통한 주입과 setter를 통한 주입은 대표적인 `DI`방식입니다.

---

## 단점

`DI`를 위한 `Injector`를 추가적으로 개발해야 합니다. 또한 코드 흐름을 추적하기에 더 복잡해지는 경향이 있습니다.
