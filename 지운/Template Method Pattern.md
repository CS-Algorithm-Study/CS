# Template Method Pattern


## 목차
1. 개념
2. 예제
3. 익명 내부 클래스

---

## 1. 개념
- GoF에서의 정의
    
    > 알고리즘의 구조를 메소드에 정의하고,
    하위 클래스에서 알고리즘 구조의 변경없이 알고리즘을 재정의 하는 패턴이다. 
    알고리즘이 단계별로 나누어 지거나, 
    같은 역할을 하는 메소드이지만 여러 곳에서 다른 형태로 사용이 필요한 경우 유용한 패턴이다.
    > 
    
- 스프링에서의 정의
    
    > 상속을 통해 슈퍼클래스의 기능을 확장할 때 사용하는 가장 대표적인 방법이다.
    > 
    > 
    > 변하지 않는 기능은 슈퍼클래스에 만들어두고
    > 
    > 자주 변경되며 확장할 기능은 서브클래스에서 만들도록 한다.
    > 
    

  ⇒ 슈퍼클래스에 공통적인 기능을 담아두고(불변), 서브클래스에 확장된 기능을 담아둔다(가변).

  ⇒ 상속관계와 오버라이딩을 통한 **다형성**으로 변하는 부분과 변하지 않는 부분을 분리하는 방법이다.

  ⇒ 슈퍼클래스와 서브클래스는 상속관계에 있는 부모-자식 관계를 생각하면 된다.

## 2. 예제
  - 라면을 끓이는 과정에 대해 다음과 같을 때를 가정한다.
        
    ![초기 상태](https://github.com/stockOneQ/back/assets/90232934/9ddc86e2-0850-4de7-b629-6253ddeece95)
        
    
  - 상속 관계를 사용하지 않고 class를 정의하면 다음과 같다.
        
        ```java
        public class ShinRamen {
        public void boilWater() {
                System.out.println("물을 끓인다.");
            }
        
        public void putNoodles() {
                System.out.println("면을 넣는다.");
            }
        
        public void putEgg() {
                System.out.println("계란을 넣는다.");
            }
        
        public void waitForMinutes() {
                System.out.println("4분 기다린다.");
            }
        }
        
        public class RaccoonRamen {
        public void boilWater() {
                System.out.println("물을 끓인다.");
            }
        
        public void putNoodles() {
                System.out.println("면을 넣는다.");
            }
        
        public void putBeanSprouts() {
                System.out.println("콩나물을 넣는다.");
            }
        
        public void waitForMinutes() {
                System.out.println("5분 기다린다.");
            }
        }
        ```
        
    
  - boilWater과 putNoodles는 이 class들이 모두 똑같이 수행하는 함수이므로 중복을 줄이기 위해 상위 class에 이 함수들을 정의하는 상속관계를 표현한다.
        
    ![적용 전 구조](https://github.com/stockOneQ/back/assets/90232934/193380e9-c2f5-4102-bba5-1a8013c6fa19)
        
    
  - 하지만 더 생각해보자. 하위 class에서 공통적으로 하는 행위가 있다. “~를 넣는다.” 와 “~분 기다린다.” 가 바로 그 행위들이다. 템플릿 메소드 패턴에서는 이 공통적으로 일어나는 행위 또한 추상화하여 상위 클래스에서 표현한다.
        
        ```java
        // 상위 클래스를 추상 클래스로 명시한다.
        public abstract class Ramen {
        
        // 템플릿 메소드
        public void makeRamen() {
                boilWater();
                putNoodles();
                putExtra();
                waitForMinutes();
            }
        
        public void boilWater() {
                System.out.println("물을 끓인다.");
            }
        
        public void putNoodles() {
                System.out.println("면을 넣는다.");
            }
        
        // 훅 메소드(추상 메소드)
        public abstract void putExtra();
        
        public abstract void waitForMinutes();
        }
        
        // 하위 클래스들
        public class ShinRamen extends Ramen {
        
            // 훅 메소드에 대해 하위 클래스마다 각각 재정의한다.
            @Override
            public void putExtra() {
                System.out.println("계란을 넣는다.");
            }
        
            @Override
            public void waitForMinutes() {
                System.out.println("4분 기다린다.");
            }
        }
        
        public class RaccoonRamen extends Ramen {
        
            @Override
            public void putExtra() {
                System.out.println("콩나물을 넣는다.");
            }
        
            @Override
            public void waitForMinutes() {
                System.out.println("5분 기다린다.");
            }
        }
        ```
        
    
  - **훅 메소드**
        
      > 추상 클래스에 들어있으며 아무 일도 하지 않거나 기본 행동을 정의하는 메소드로,
      > 
      > 
      > **서브 클래스에서 오버라이드** 할 수 있다.
      > 
    
  - 템플릿 메소드가 적용된 뒤 class의 구조
        
    ![적용 후 구조](https://github.com/stockOneQ/back/assets/90232934/62605cf9-1b50-45eb-b068-fcc30fbce849)
        
## 3. 익명 내부 클래스
  - 개념
    
    > 하위 클래스에 대한 정의를 하지 않고,
    > 
    > 
    > 객체를 생성할 때 생성할 클래스를 상속 받은 자식 클래스를 정의한다.
    > 
    > 직접 지정하는 이름이 없고 클래스 내부에 선언되는 클래스이다.
    > 
    > 일회용 클래스로, 테스트 시 사용이 용이하다.
    > 
    
  - 익명 내부 클래스 사용 방법
      
      ```
        객체 = new 인스턴스() {
              해당 인스턴스가 가진 특정 메소드를 재정의하여 사용
        };
      ```
        
    
  - 익명 내부 클래스로 RaccoonRamen 표현
      
      ```java
      @Test
      void testMethod {
          Ramen raccoon = new Ramen(){  
              @Override
              public void putExtra() {
                  System.out.println("콩나물을 넣는다.");
              }
          
              @Override
              public void waitForMinutes() {
                  System.out.println("5분 기다린다.");
              }	
          }
          Ramen.makeRamen();
      }
      ```
