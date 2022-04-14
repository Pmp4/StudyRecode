# ⛏ Java Thread ⛏

## 프로세스
OS로부터 각각의 할당된 메모리 공간을 기반으로 <b>실행 중인 프로그램</b>을 프로세스라고 한다.
프로세스는 프로그램 수행에 필요한 데이터와 메모리로 자원 + 쓰레드로 구성된다.

## Thread
프로세스 공간 내 실제로 작업을 처리하는 작업단위를 뜻한다.

Thread는 SingleThread와 MultiThread로 두개로 나눠진다.  
- SingleThread : 실행 흐름이 하나로, 한 번에 하나의 작업처리
- MultiThread : 두 개 이상의 작업을 동시에 처리
  - 장점 : CPU 사용률 향상, 자원을 효율적 사용, 응답성 향상, 코드 간결
  - 단점 : 병목

### 구현 방법
1. Thread 클래스 상속
2. Runnable 인터페이스 구현

#### Thread 클래스 상속
```java
class MyThread extends Thread {
  public void run(){} //run() Overriding
}

MyThread mt = new MyThread();
mt.start();
```
Thread 클래스를 상속받고 run() 메서드를 오버라이딩한다. 해당 쓰레드를 사용하려면 객체 생성 후 start() 메서드를 사용하면된다.

#### Runnable 인터페이스 구현
```java
class MyRunable implements Runnable {
  public void run(){} //run() Overriding
}

MyRunable mr = new MyRunable();
Thread th = new Thread(mr); //Thread(Runnable target)
th.start();
```
Runnable 인터페이스를 구현한 쓰레드를 사용하기 위해선 객체 생성 후, 해당 객체를 Thread 생성자에 담아줘야한다.

#### 대표메서드
```java
currentThread()  //현재 실행중인 Thread의 참조를 리턴
getName()  //Thread의 이름 리턴
start()  //Thread의 객체를 생성 후 start()로 호출해야만 작업을 시작한다. 재사용 불가하다.
run() //메서드의 호출일 뿐 Thread의 생성으로 이어지지는 않는다. run() 안에 해당 Thread의 작업을 구현한다.
```
|메소드|설명|
|------|---|
|static void sleep(long msec)|msec 밀리초 대기|
|테스트1|테스트2|
|테스트1|테스트2|
|테스트1|테스트2|
|테스트1|테스트2|
|테스트1|테스트2|

### Thread 사용 경우

### Thread 사용 예시

### Thread의 메모리 구성

### Thread의 우선순위
JVM은 쓰레드의 실행을 컨트롤한다.
- 우선순위가 높은 쓰레드의 실행을 우선한다.
- 동일한 우선순위일 경우 CPU의 할당시간을 분배하여 실행한다.

우선순위의 값에 따라 쓰레드가 얻는 실행시간이 달라진다. 중요도에 따라 우선순위를 다르게 지정하여 특정 쓰레드가 더 많은 작업시간을 가지도록 할 수 있다.

> 가장 높은 우선순위 : 10<br>
> 가장 낮은 우선순위 : 1
```java
int getPriority() //우선순위를 리턴
void setPriority(int new Priority)  //우선순위를 지정

public static final int MAX_PRIORITY = 10
public static final int MIN_PRIORITY = 1
public static final int NORM_PRIORITY = 5
```

### Daemon Thread (데몬 쓰레드)
일반 쓰레드의 작업을 돕는 보조적 역할을 수행하는 쓰레드. 일반 쓰레드 종료 시 데몬 쓰레드도 강제적으로 종료된다.
```java
setDaemon(boolean on)
```
쓰레드에 해당 메서드를 사용하여 데몬 쓰레드의 여부를 설정한다. setDaemon()은 반드시 해당 쓰레드의 실행 전(start())에 호출해야한다. 

## Life Cycle
|상태|설명|
|:------|:---|
|NEW|쓰레드가 생성되고, 아직 start()가 호출되지 않은 상태|
|RUNNABLE|실행 중 또는 실행 가능 상태|
|BLOCKED|동기화블럭에 의해 일시정지|
|WAITING, TIMED_WAITING|테스트2|
|TERMINATED|쓰레드 작업 종료|

### 
