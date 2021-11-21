# 스프링 부트 앱이 구동될때 코드를 실행시키는 방법

## 1. CommandLineRunner

* run() 메소드 하나만 갖고 있는 함수형 인터페이스이다. (람다로 표현 가능하다)
* 모든 컴포넌트가 등록되고 활성화 된 이후에 run() 메소드가 자동으로 실행되는 것이 보장된다.

```java

@Component
public class RepositoryDatabaseLoader {

  @Bean
  CommandLineRunner initialize(Repository repository) {
    return args -> {
      repository.save(new Item("ProductA"));
      repository.save(new Item("ProductB"));
    };
  }
} 
```

## 2. ApplicationRunner

* CommandLineRunner 와 차이점은 받은 argument 가 더 다양하다는점이다.
* ApplicationRunner 가 더 최신버전에서 지원한다.

```java

@Component
public class RepositoryDatabaseLoader {

  @Bean
  ApplicationRunner initialize(Repository repository) {
    return args -> {
      repository.save(new Item("ProductA"));
      repository.save(new Item("ProductB"));
    };
  }
} 
```

## 3. EventListener
* 메소드에 `@EventListener` 어노테이션을 추가 후 받을 이벤트를 설정하면 된다.
* 스프링에서 기본으로 제공하는 이벤트는 아래와 같다.
  * ApplicationContextInitializedEvent
  * ApplicationEnvironmentPreparedEvent
  * ApplicationPreparedEvent
  * ApplicationReadyEvent
  * ApplicationStartedEvent
  * ApplicationFailedEvent
  * ApplicationStartingEvent
* 이외에 이벤트를 직접 생성해서 추가해도 된다.  
* 이벤트 처리 방식은 bean 간의 결합을 끊어버릴 수 있다.
```java

@Component
@RequiredArgsConstrucotr
public class RepositoryDatabaseLoader {

  private final Repository repository;

  @EventListener(ApplicationReadyEvent.class)
  void init() {
    repository.save(new Item("ProductA"));
    repository.save(new Item("ProductB"));
  }
} 
```

* EventListener 나 ApplicationRunner 를 사용하는게 좋겠다. 

## 참고
* [토비의 스프링 부트 1 - 스프링 부트 앱에 초기화 코드를 넣는 방법 3가지](https://www.youtube.com/watch?v=f017PD5BIEc) 