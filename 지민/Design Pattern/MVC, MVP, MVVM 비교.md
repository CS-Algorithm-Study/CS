# MVC, MVP, MVVM 비교

## 1. MVC

- MVC 패턴은 Model + View + Controller를 합친 용어

### 1) 구조

<img src="https://github.com/CS-Algorithm-Study/CS/assets/77067383/fb5b814e-ecf8-4e35-a4d1-116aa5fdf57f" width="70%">

- Model: 애플리케이션에서 사용되는 데이터와 비즈니스 로직을 처리하는 부분
- View: 사용자에게 보여지는 UI 부분
- Controller: 사용자의 입력(Action)을 받고 처리하는 부분

### 2) 동작

1. 사용자의 Action들은 Controller에 들어온다.
2. Controller는 사용자의 Action을 확인하고, Model을 업데이트 한다.
3. Controller는 Model을 나타내줄 View를 선택한다.
4. View는 Model을 이용하여 화면을 나타낸다.

> 참고 - MVC에서 View가 업데이트 되는 방법
> - View가 Model을 이용해 직접 업데이트 하는 방법
> - Model에서 View에게 Notify 하여 업데이트 하는 방법
> - View가 Polling으로 Model의 변경을 주기적으로 감지해서 업데이트 하는 방법

### 3) 특징

- Controller는 여러개의 View를 선택할 수 있는 1:N 구조임
- Controller는 View를 선택할 뿐 직접 업데이트 하지 않음 (View는 Controller를 알지 못함)

### 4) 장점

- MVC 패턴은 단순함
    - 단순하다 보니 보편적으로 많이 사용되는 디자인 패턴

### 5) 단점

- View와 Model 사이 의존성이 높음
    - 이 높은 의존성은 애플리케이션 규모가 커질수록 복잡해지고 유지보수를 어렵게 만들 수 있음

---

## 2. MVP

- MVP는 Model + View + Presenter를 합친 용어
- Model, View는 MVC 패턴과 동일하고, Controller 대신 Presenter가 존재함

### 1) 구조

<img src="https://github.com/CS-Algorithm-Study/CS/assets/77067383/c4af1eac-7a0e-4370-bd34-6991be9187b1" width="70%">

- Model: 애플리케이션에서 사용되는 데이터와 비즈니스 로직을 처리하는 부분
- View: 사용자에게 보여지는 UI 부분
- Presenter: View에서 요청한 정보로 Model을 가공하여 View에 전달해주는 부분

### 2) 동작

1. 사용자의 Action들은 View를 통해 들어오게 된다.
2. View는 데이터를 Presenter에게 요청한다.
3. Presenter는 Model에게 데이터를 요청한다.
4. Model은 Presenter에게 요청받은 데이터를 응답한다.
5. Presenter는 View에게 데이터를 응답한다.
6. View는 Presenter가 응답한 데이터를 이용하여 화면을 나타낸다.

### 3) 특징

- Presenter는 View와 Model의 인스턴스를 가지고 있어 둘을 연결하는 접착제 역할을 함
    - 인스턴스: 객체 지향 프로그래밍에서 특정 클래스의 실체화된 예입니다. 클래스는 객체를 생성하기 위한 일종의 템플릿, 이 템플릿을 기반으로 실제로 메모리에 할당된 것임
- Presenter와 View는 1:1 관계임

### 4) 장점

- View와 Model의 의존성이 없음
    - Presenter을 통해서만 데이터를 전달받기 때문

### 5) 단점

- View와 Model 사이의 의존성은 해결됐지만, View와 Presenter 사이의 의존성이 높아짐
    - 애플리케이션이 복잡해질수록 View와 Presenter 사이의 의존성이 강해지는 단점이 있음

---

## 3. MVVM

- MVVM패턴은 Model + View + View Model을 합친 용어
- Model과 View는 다른 패턴과 동일함

### 1) 구조

<img src="https://github.com/CS-Algorithm-Study/CS/assets/77067383/b00ebed2-d091-478a-ad6a-ead048419d32" width="70%">

- Model: 애플리케이션에서 사용되는 데이터와 비즈니스 로직을 처리하는 부분
- View: 사용자에게 보여지는 UI 부분
- View Model: View를 표현하기 위해 만든 View를 위한 Model
    - View를 나타내기 위한 데이터를 처리하는 부분

### 2) 동작

1. 사용자의 Action들은 View를 통해서 들어오게 됩니다.
2. View에서 Action이 들어오면, Command 패턴으로 View Model에 Action을 전달한다.
    1. **Command 패턴**은 특정 작업을 나타내는 객체를 생성하여 해당 작업을 수행하도록 요청하는 것
3. View Model은 Model에게 데이터를 요청한다.
4. Model은 View Model에게 요청받은 데이터를 응답한다.
5. View Model은 응답을 받은 데이터를 가공하여 저장한다.
6. View는 View Model과 Data Binding하여 화면을 나타낸다. 
    1. **Data Binding**은 UI 컴포넌트와 데이터 소스(일반적으로 ViewModel) 간의 자동 동기화를 가능하게 하는 기술이나 패턴을 말함

### 3) 특징

- Command 패턴과 Data Binding 두 가지 패턴을 사용해서 구현됨
- 이 두 가지를 이용해 View와 View Model 사이의 의존성을 없앴음
    - Command 패턴
        - Command 패턴은 사용자의 액션을 특정 객체(커맨드 객체)로 캡슐화한다.
        - View에서 발생한 사용자 액션은 Command 객체를 생성하고, 이를 ViewModel로 전달한다.
        - 이로써 View는 ViewModel의 구체적인 구현에 대한 지식 없이도 액션을 수행할 수 있다.
        - 즉, View는 ViewModel에 직접적으로 의존하지 않고, Command 객체를 통해 간접적으로 상호 작용한다.
    - Data Binding
        - Data Binding은 View와 ViewModel 간의 속성을 연결하여 자동으로 동기화될 수 있게 한다.
        - View는 ViewModel의 데이터를 직접적으로 참조하거나 호출하지 않고, 데이터 바인딩을 통해 간접적으로 데이터에 접근한다.
        - 이로써 View는 ViewModel의 내부 구현에 대한 세부 사항을 알 필요 없이 UI를 업데이트할 수 있다.
        - ViewModel은 데이터의 변경을 알리는데 집중하고, View는 이 변경 사항에 반응하는 역할을 수행한다.
- View Model과 View는 1:N 관계임

### 4) 장점

- View와 Model 사이 의존성이 없음
- 또한 Command 패턴과 Data Binding을 사용하여 View와 View Model 사이의 의존성 또한 없앤 디자인 패턴
- 각 부분이 독립적이기 때문에 모듈화하여 개발할 수 있음

### 5) 단점

- View Model의 설계가 어려움

[참고](https://beomy.tistory.com/43#recentComments)
