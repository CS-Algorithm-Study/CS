# 📲 Reflection
## 목차
### 1. Reflection 이란?
### 2. Reflection 동작 원리 및 예제
### 3. Reflection 장단점 및 활용 예시
</br>

## 🤔 Reflection 이란?

</br>

- **구체적인 클래스 타입을 알지 못해도** 그 클래스의 정보(메서드, 타입, 필드 등)에 접근할 수 있게 해주는 자바 API

</br>

```java
public class Car {
    private final String name;
    private int position;

    public Car(String name, int position) {
        this.name = name;
        this.position = position;
    }

    public void move() {
        this.position++;
    }
}

public static void main(String[] args) {
    Object obj = new Car("foo", 0);
    obj.move();    // 컴파일 에러 발생 java: cannot find symbol
}

```

- **컴파일 타임에 타입이 결정**되고, 위 코드에서 obj 객체는 컴파일 타임에 Object로 타입이 결정되었기 때문에 **Object 클래스의 인스턴스 변수와 메서드만 사용**할 수 있다.

</br>


## 🤔 Reflection 동작 원리 및 예제

</br>

![image](https://github.com/CS-Algorithm-Study/CS/assets/81271328/2fdb7968-81a5-4acf-a12f-1b097de52d53)

- **Method Area**에는 **`클래스의 이름`**, **`부모 클래스의 이름`**, **`메서드와 필드의 정보`**, **`상수`** 등의 정보가 존재
</br>

```java
public class Main {
    public static void main(String[] args) throws Exception {
        //1. .class를 통해 클래스를 가져오는 방법
        Class<Car> carClass = Car.class;

        //2. Car 클래스의 인스턴스를 통해 클래스를 가져오는 방법
        Car car = new Car();
        Class<? extends Car> carClass2 = car.getClass();

        //3. Class.forName을 통해 (패키지명).클래스명으로 클래스를 가져오는 방법
        Class<?> carClass3 = Class.forName("example.Car");


        // Reflection API를 이용하여 carClass의 move 메서드 가져오기
        Method move = carClass.getMethod("move");

        // move 메서드 실행, invoke(메서드를 실행시킬 객체, 해당 메서드에 넘길 인자)
        move.invoke(car, null);

        // Car의 private 변수인 position 가져오기
        Field field = carClass.getDeclaredField("position");
        field.setAccessible(true);
        int position = (Integet) field.get(car);
    }
}
```

</br>

## 3. Reflection 장단점

**장점**

- 런타임 시점에 클래스의 인스턴스를 생성하고, 접근 제어자와 관계없이 필드와 메서드에 접근하여 작업을 수행할 수 있는 유연성을 가진다

**단점**

- 컴파일 타임이 아닌 런타임에 동적으로 타입을 분석하고 정보를 가져오기 때문에 JVM을 최적화할 수 없어 성능 오버헤드가 발생한다
- 직접 접근할 수 없는 private 인스턴스 변수와 메서드에 접근하기 때문에 추상화가 깨지고 캡슐화를 저해한다
- 런타임 시점에 인스턴스를 생성하므로 구체적인 동작 흐름을 파악하기 어렵다

</br>

**활용 예시**
- Spring Container BeanFactory(DI)
- Intellij의 자동완성
- jackson 라이브러리(직렬화, 역직렬화)
- JPA 구현체 Hibernate
