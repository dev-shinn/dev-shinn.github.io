---
title: 테스트
date: 2024-08-17 23:18:00 +/-TTTT
categories: [TEST, 테스트]
tags: [TAG]     # TAG names should always be lowercase
published: true
---

싱글턴 패턴22
 - 클래스 인스턴스를 하나만 만들고, 그 인스턴스로의 전역 접근을 제공 22223333
 

핵심정리
 - 어떤 클래스에 싱글턴 패턴을 적용하면 그 클래스의 인스턴스가 1개만 있도록 할 수 있다.
 - 싱글턴 패턴을 사용하면 하나뿐인 인스턴스를 어디서든지 접근 할 수 있도록 할 수 있다.
 - 자바에서 싱글턴 패턴을 구현할 때는 private 생성자와 정적 메소드, 정적 변수를 사용합니다.
 - 멀티 스레드를 사용하는 애플리케이션에서는 속도와 자원 문제를 파악해 보고 적절한 구현법을 사용할것.
 - 클래스 로더가 여러 개 있으면 싱글턴이 제대로 작동하지 않고, 여러 개의 인스턴스가 생길 수 있다.
 - 자바의 enum을 쓰면 간단하게 싱글턴을 구현할 수 있다.

``` mermaid
---
title: Animal example
---
classDiagram
    class Singleton{
        static uniqueInstance
        // 기타 데이터
        static getInstance()
        // 기타 메소드()
    }
```



```java
public class Singleton {

    private static Singleton uniqueInstance; // Singleton 클래스의 하나뿐인 인스턴스를 저장하는 정적 변수

    private Singleton(){ // 생성자를 private으로 선언했으므로 Singleton에서만 클래스 인스턴스 만들 수 있음

    }

    public static Singleton getInstance(){ // 클래스의 인스턴스를 만들어서 리턴
        if (null == uniqueInstance) {
            uniqueInstance = new Singleton();
    /**
            * 인스턴스가 만들어지지 않았으면 private으로 선언된 생성자를 사용해서 
            * Singleton 객체 생성 후 uniqueInstance 에 대입
            * 인스턴스가 필요한 상황이 발생하기 전까지 인스턴스 생성하지 않음
            * 게이른 인스턴스 생성(lazyinstantiation) 이라 부름
            */
        }
        return uniqueInstance;
    }
}
```

일반 생성 
 - 인스턴스가 2개가 생성될 경우 문제가 발생할수 있음
```java
package Chapter5.chocolate;
public class ChocolateBoiler {
    private boolean empty;
    private boolean boiled;

    private ChocolateBoiler(){
        empty = true;
        boiled = false;
    }

    public void fill(){
        if(isEmpty()){
            empty = false;
            boiled = false;
            // 보일러에 우유와 초콜릿을 혼합한 재료를 넣음
        }
    }

    public void drain(){
        if(!isEmpty() && isBoiled()){
            // 끓인 재료를 다음 단계로 넘김
            empty = true;
        }
    }

    public void boil(){
        if(!isEmpty() && !isBoiled()){
            // 재료를 끓임
            boiled = true;
        }
    }

    public boolean isEmpty(){
        return empty;
    }

    public boolean isBoiled(){
        return boiled;
    }
}
```

싱글턴 방식으로 생성 
 - 멀티스레딩에서 인스턴스 중복 생성 가능
 - getInstance()를 동기화하여 멀티스레딩과 관련된 문제를 해결
 - synchronized 키워드 추가시 한 스레드가 메소드 사용을 끝내기 전까지 다른 스레드는 대기 하고 있어야함

동기화 문제
 - 동기화 시 속도 문제가 발생 할 수 있음
 - 동기화가 꼭 필요한 시점은 메소드가 시작될때만
(uniqueInstance 변수에 Singleton 인스턴스가 대입되면 동기화가 의미가 없어짐 -> 불필요한 오버헤드 증가)

해결 방안
 - 큰 문제가 없다면 그냥 둔다
 - 인스턴스가 필요할 때는 생성하지 말고 처음부터 생성

 ```java
 public class Singleton {

    // 클래스가 로딩될 때 JVM 에서 Singleton의 하나뿐인 인스턴스를 생성함
    private static Singleton uniqueInstance = new Singleton();

    private Singleton(){}

    public static Singleton getInstance(){
        return uniqueInstance;
    }

}
```

 - DCL(Double-Checked Locking)을 써서 getInstance() 에서 동기화되는 부분을 줄임

```java
public class Singleton {

    private volatile static Singleton uniqueInstance = new Singleton();

    private Singleton(){}

    public static Singleton getInstance(){
        if(uniqueInstance == null){
            synchronized(Singleton.class){
                if(uniqueInstance == null){
                    uniqueInstance = new Singleton();
                }
            }
        }
        return uniqueInstance;
    }

}
```

```java
package Chapter5.chocolate;

public class ChocolateBoiler {
    
    private boolean empty;
    private boolean boiled;

    private static ChocolateBoiler uniqueChocolateBoiler;

    private ChocolateBoiler(){
        empty = true;
        boiled = false;
    }

    public static ChocolateBoiler getInstance(){
        if(null==uniqueChocolateBoiler){
            uniqueChocolateBoiler = new ChocolateBoiler();
        }
        return uniqueChocolateBoiler;
    }

    public void fill(){
        if(isEmpty()){
            empty = false;
            boiled = false;
            // 보일러에 우유와 초콜릿을 혼합한 재료를 넣음
        }
    }

    public void drain(){
        if(!isEmpty() && isBoiled()){
            // 끓인 재료를 다음 단계로 넘김
            empty = true;
        }
    }

    public void boil(){
        if(!isEmpty() && !isBoiled()){
            // 재료를 끓임
            boiled = true;
        }
    }

    public boolean isEmpty(){
        return empty;
    }

    public boolean isBoiled(){
        return boiled;
    }
}
```

```java
    public static synchronized ChocolateBoiler getInstance(){
        if(null==uniqueChocolateBoiler){
            uniqueChocolateBoiler = new ChocolateBoiler();
        }
        return uniqueChocolateBoiler;
    }
```

-- 공부할것
volatile
클래스 로더