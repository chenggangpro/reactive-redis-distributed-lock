# reactive-lock
Reactive Lock Demo

### Use Case

* 1 . Make Sure Use Spring Boot Starter Dependency 

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis-reactive</artifactId>
</dependency>
```

* 2 . Configure Properties


```yaml
lock:
  redis:
    reactive:
      expire-after: 10s
      expire-evict-idle: 1s
      reactive-lock-type:
        - DEFAULT  # default
        - REDIS   # redis
```

* 3 . Autowired `ReactiveLockRegistry defaultReactiveLockRegistry` / `ReactiveLockRegistry redisReactiveLockRegistry` By Spring

* 4 . Use `ReactiveLockRegistry` With Your Reactive Application

```java
public interface ReactiveLock {

    /**
     * Try to acquire the lock once
     * @param lockResultExecution apply lockResult return executable Mono
     * @param <T> Mono type
     * @return executable Mono
     */
    <T> Mono<T> tryLockThenExecute(@NotNull Function<Boolean, Mono<T>> lockResultExecution);

    /**
     * Try to acquire the lock once
     * @param lockResultExecution apply lockResult return executable Flux
     * @param <T> Flux type
     * @return executable Flux
     */
    <T> Flux<T> tryLockThenExecuteMany(@NotNull Function<Boolean, Flux<T>> lockResultExecution);

    /**
     * Try to acquire the lock for a given duration.
     * @param duration lock expire duration
     * @param lockResultExecution apply lockResult return executable Mono
     * @param <T> Mono type
     * @return executable Mono
     */
    <T> Mono<T> lockThenExecute(@NotNull Duration duration, @NotNull Function<Boolean, Mono<T>> lockResultExecution);

    /**
     * Try to acquire the lock for a given duration.
     * @param duration lock expire duration
     * @param lockResultExecution apply lockResult return executable Flux
     * @param <T> Flux type
     * @return executable Flux
     */
    <T> Flux<T> lockThenExecuteMany(@NotNull Duration duration, @NotNull Function<Boolean, Flux<T>> lockResultExecution);

}
```

* 5 .If You Need More Detail About This Please Read The Source Code and test cases.

# Reference

* RedisLockRegistry: https://docs.spring.io/spring-integration/docs/5.3.6.RELEASE/reference/html/redis.html#redis-lock-registry
* Trigger Mono Execution After Another Mono Terminates: https://stackoverflow.com/questions/50686524/how-to-trigger-mono-execution-after-another-mono-terminates 