# Proxy Pattern


## 목차
1. 개념
2. 예제

---

## 1. 개념
- 정의

  ![프록시 패턴 예제](https://github.com/CS-Algorithm-Study/CS/assets/90232934/e15ce93e-9d99-4ae5-8f4e-f5e53377c10d)
    > 어떤 객체를 사용하고자 할때, 객체를 직접적으로 참조 하는것이 아니라,
    > 해당 객체를 대행(대리, proxy)하는 객체를 통해 대상 객체에 접근
    >
    > - 해당 객체가 메모리에 존재하지 않아도 **기본적인 정보를 참조**하거나 설정 가능
    > - 실제 객체의 기능이 반드시 필요한 시점까지 **객체의 생성을 미룰 수** 있음

- `가상 프록시`
  - 꼭 필요로 하는 시점까지 객체의 생성을 연기하고, 해당 객체가 생성된 것처럼 동작하는 방식
  - 주체 클래스는 리소스가 많이 요구되는 작업이 필요할 때만 사용, 프록시 클래스에서 이외의 자잘한 작업들을 처리
  - 예) 해상도가 아주 높은 이미지를 처리할 때 작업 분산
- `원격 프록시`
  - 원격 객체에 대한 접근은 제어 로컬 환경에 존재하며, 원격 객체에 대한 대변인 역할을 하는 객체와 **서로 다른 공간에 있는 객체가 마치 같은 주소 공간에 있는 것처럼** 동작하는 방식
  - 예) Google Docs : 브라우저는 브라우저대로 필요한 자원을 로컬에 가지고 있고, 또다른 자원은 Google 서버에 있는 형태
- `보호 프록시`
  - 주체 클래스에 대한 접근을 제어하기 위해, 객체에 대한 접근 권한을 제어하거나 객체마다 접근 권한을 달리하도록 동작하는 방식
  - 프록시 클래스에서 클라이언트가 주체 클래스에 대한 접근을 허용할지 말지 결정

- 장점
  1. 사이즈가 큰 객체(예: 이미지)가 로딩되기 전에도 프록시를 통해 참조 가능
  2. 실제 객체의 public, protected 메소드들을 숨기고 인터페이스를 통해 노출 
  3. 로컬에 있지 않고 떨어져 있는 객체 사용 가능
  4. 원래 객체의 접근에 대해서 사전처리 가능
- 단점
  1. 객체를 생성할때 한단계를 거치게 되므로, 빈번한 객체 생성이 필요한 경우 성능 저하
  2. 프록시 내부에서 객체 생성을 위해 스레드 생성, 동기화가 구현되어야 하는 경우 성능 저하
  3. 로직이 난해해져 가독성 저하

## 2. 예제 
  - Image
        
        ```
        public interface Image {
           void displayImage();
        }

        ```
  - Real_Image (주체 클래스)
        
        ```
        public class Real_Image implements Image {
        
            private String fileName;
            
            public Real_Image(String fileName) {
                this.fileName = fileName;
                loadFromDisk(fileName);
            }
            
            private void loadFromDisk(String fileName) {
                System.out.println("Loading " + fileName);
            }
            
            @Override
            public void displayImage() {
                System.out.println("Displaying " + fileName);
            }
        }
        ```
  - Proxy_Image (프록시 클래스)
        
        ```
        public class Proxy_Image implements Image {
            private Real_Image realImage;
            private String fileName;
            
            public Proxy_Image(String fileName) {
                this.fileName = fileName;
            }
            
            @Override
            public void displayImage() {
                if (realImage == null) {
                    realImage = new Real_Image(fileName);
                }
                realImage.displayImage();
            }
        }
        ```
  - Proxy_Main (테스트)
        
        ```
        public class Proxy_Main {
            public static void main(String[] args) {
                Image image1 = new Proxy_Image("test1.png");
                Image image2 = new Proxy_Image("test2.png");
                
                image1.displayImage();
                System.out.println();
                image2.displayImage();
            }
        }
        ```
  - 결과
    
    ![결과](https://github.com/CS-Algorithm-Study/CS/assets/90232934/f0da677a-bfe0-4ee9-bc1d-e9e048cae9f2)
