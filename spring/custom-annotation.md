# Custom Annotation 작성

* AOP를 학습하면서 Custom Annotation 작성하는 방법을 배울 필요가 있다고 느낌

## 1. @interface 사용

```java
// Annotation 정의
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface CustomAnnotation{}
```

## 2. AOP 설정

```java
// AOP 설정, 시간 측정 예시
@Aspect
@Configuration
public class MethodAOPLogic{
    @Around("@annotation(CustomAnnotation)")
    public Object recordMethodExecution(ProceedingJoinPoint joinPoint) throws Throwable{
        StopWatch stopWatch = new StopWatch();
        stopWatch.start();
        
        Object result = joinPoint.proceed();
        
        stopWatch.stop();
        System.out.println(joinPoint.getSignature() + " execution time: " + stopWatch.getTotalTimeMillis());
        
        return result;
    }
}
```

## 3. 적용

```java
// @EnableAspectJAutoProxy 설정
@EnableAspectJAutoProxy
@SpringBootApplication
public void SpringApplication{
    ...
}

// AOP 적용
@CustomAnnotation
public void anyMethod(){
    ...
}
```
