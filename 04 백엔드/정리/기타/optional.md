# `Optional`이 필요한 이유
`findById(id)` 메서드는 `Optional<T>`을 반환합니다. 
이유는 해당 `id`에 해당하는 엔티티가 존재하지 않을 경우 `null`을 반환하는 대신 **`Optional.empty()`를 반환하여 NullPointerException(NPE)을 방지하고, 더 안전한 코드 작성을 유도**하기 위함입니다.


# `Optional` 처리 방법

## `orElse()`: 값이 없으면 기본값 반환
`orElse()`는 기본값을 **미리 생성**하므로, 값이 있든 없든 항상 `new Example()`이 실행됩니다.

```java
Example example = exampleRepository.findById(id)
        .orElse(new Example()); // new Example()이 항상 실행됨
```

- **값이 있을 때** → 해당 값을 반환하지만, `new Example()`은 **여전히 실행됨** (비효율적)
- **값이 없을 때** → `new Example()`이 반환됨

---

## `orElseGet()`: 값이 없으면 람다식 실행
값이 없을 때만 `createDefaultExample()`을 실행합니다.

```java
Example example = exampleRepository.findById(id)
        .orElseGet(() -> createDefaultExample());
```

- **값이 있을 때** → 해당 값을 반환 (람다식 실행 안 됨 ❌)
- **값이 없을 때** → `createDefaultExample()` 실행 후 반환

---

## `orElseThrow()`: 값이 없으면 예외 발생

값이 없으면 예외를 던집니다.

```java
Example example = exampleRepository.findById(id)
        .orElseThrow(() -> new EntityNotFoundException("Example not found with id: " + id));
```

- **값이 있을 때** → 해당 값을 반환
- **값이 없을 때** → `EntityNotFoundException` 예외 발생

---

## `ifPresent()`: 값이 존재하면 실행

값이 있으면 특정 동작을 수행합니다.

```java
exampleRepository.findById(id)
        .ifPresent(example -> System.out.println("Found: " + example));
```

- **값이 있을 때** → `"Found: example"` 출력
- **값이 없을 때** → 아무 동작도 하지 않음

---

## `map()`: 값이 있으면 특정 필드 변환

값이 있으면 특정 값을 변환하고, 없으면 기본값을 반환합니다.

```java
String name = exampleRepository.findById(id)
        .map(Example::getName)
        .orElse("Unknown");
```

- **값이 있을 때** → `getName()` 실행 후 반환
- **값이 없을 때** → `"Unknown"` 반환

---

## `flatMap()`: 중첩된 Optional 처리

내부적으로 `Optional`을 반환하는 경우 중첩을 제거합니다.

```java
Optional<Address> address = exampleRepository.findById(id)
        .flatMap(Example::getAddressOptional);
```

- **값이 있을 때** → `getAddressOptional()` 실행 후 반환
- **값이 없을 때** → `Optional.empty()` 반환

---

## `filter()`: 조건에 맞는 경우만 값 유지

조건을 만족하면 값을 유지하고, 만족하지 않으면 `Optional.empty()`를 반환합니다.

```java
Optional<Example> example = exampleRepository.findById(id)
        .filter(Example::isActive);
```

- **값이 있고 조건 충족** → 해당 값을 `Optional`로 반환
- **값이 있지만 조건 불충족** → `Optional.empty()` 반환
- **값이 없을 때** → `Optional.empty()` 반환

---

## `get()`: 값이 없으면 예외 발생 (권장 X)
값이 없을 때 예외가 발생합니다.

```java
Example example = exampleRepository.findById(id)
        .get(); // 값이 없으면 NoSuchElementException 발생
```

- **값이 있을 때** → 해당 값을 반환
- **값이 없을 때** → `NoSuchElementException` 발생 (⚠️ 위험)
