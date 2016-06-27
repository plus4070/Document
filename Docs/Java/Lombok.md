## Lombok

### @AllArgsConstructor

 - 모든 필드 값을 받는 생성자를 생성
 - 접근 제어: AccessLevel지정을 통해 접근 레벨을 제한 할 수 있다. (PUBLIC, PROTECTED, PACKAGE, PRIVATE)

```java
@AllArgsConstructor(AccessLevel.PUBLIC)
```

### @NoArgsConstructor

 - parameter를 받지 않는 생성자를 생성

```java
@NoArgsConstructor
```

### @Getter & @Setter

 - Getter와 Setter 함수를 생성
 - 접근 제어: AccessLevel지정을 통해 접근 레벨을 제한 할 수 있다. (PUBLIC, PROTECTED, PACKAGE, PRIVATE)

```java
@Getter(AccessLevel.PACKAGE)
```

### @Log / @Log4j / @Slf4j

 - 자동으로 logging을 위한 필드인 `private static final Logger log`를 추가한다.
 - Log,Log4j: Log4j를 사용하여 출력
 - Slf4j: Slf4j를 사용하여 출력

### @EqualsAndHashCode

 - 코드에서 객체의 비교 등의 용도로 사용되는 equals(), hashCode() 메소드의 코드를 생성
 - 특정 필드를 제외할 수 있다. (exclude)

```java
@EqualsAndHashCode(exclude={"id", "name"})
```

### @ToString

 - 객체의 내용을 문자열로 변환해주는 toString() 메소드를 생성한다.
 - 특정 필드를 제외할 수 있다. (exclude)


```java
@String(exclude={"uuid"})
```

**출력**
```java
{Object Name}({변수}={값}, {변수}={값}, ...)
```

### @Data

 - 모든 필드에 대한 getter, setter, toString, equals, hashCode, final로 지정됐거나 @NonNull로 명시된 필드에 대한 값을 받는 생성자 메소드 코드를 생성

```java
@Data
```

### @NonNull

 - 변수가 null인지 확인한다.
 - `if (param == null) throw new NullPointerException("param");`과 같은 역할

```java
public boolean isTrue(@NonNull String name)
```

### @Cleanup

 - local variable에 annotaion을 붙히면 현재 code가 종료될때 자동으로 close 함수가 호출됨
 - close()가 없고 다른 method가 cleanup을 수행할 경우 `@Cleanup("dispose")`와 같이 method면 기술

```java
@Cleanup File file = new File();
```

### @Accessors

 - 기존에 생성된 setter들의 return 값을 this로 바꿔준다.
 - `chain=true` parameter를 사용하면 setter의 리턴 타입이 this로 변경된다.

```java
@Accessors(chain=true)
```

```java
ID id = new ID().setName("이름").setAge(15);
```
