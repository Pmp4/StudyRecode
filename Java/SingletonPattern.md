# 🍥 SingletonPattern (싱글톤 패턴) 🍥
소프트웨어 설계를 잘 할 수 있도록 만들어 줄 수 있는 방법
- 소프트웨어 설계 방법론 : 일반화된 것
- 사례를 통한 공유 : 너무 구체적

너무 일반적이지도, 또한 너무 구체적이지도 않은 형태로 소프트웨어 설계를 위한 지식이나 노하우를 공유할 수 있는 방법

### 인스턴스를 하나만 생성하여 사용하는 패턴
```java
public class Singleton {
  private static Singleton instance;
  
  private Singleton() {}
  
  public static Singleton getInstance() {
    if(instance == null) {
       instance = new Singleton();
    }
    
    return instance;
  }
}
```

```java
public class Person {
  private static Person instance; 
  //static 변수 - 클래스 차원에서 하나만 생성되어 모든 객체가 공유한다.
  
  private Person() {} //private으로 무분별한 instance 생성을 막는다.
  
  public Person getInstance() {
    if(instance == null) {  //현재 생성된 인스턴스가 없을 경우
      instance = this();    //인스턴스를 생성하여, static 변수에 담아준다.
    }
    
    return instance;
  }
  
  public void showInfo() {
    System.out.println("Person 메서드");
  }
}
```
