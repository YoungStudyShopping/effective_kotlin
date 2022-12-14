## 복잡한 객체를 생성하기 위한 DSL을 정의하라

- DSL은 복잡한 객체, 계층구조를 갖고 있는 객체들을 정의할때 유용.
- DSL을 활용하면 복잡하고 계층적인 자료구조를 쉽게 만들 수 있음.
- DSL 내부에서 코틀린이 제공하는 모든 것을 활용할 수 있음.
- 코틀린의 DSL을 type-safe이므로 여러가지 유용한 힌트를 활용 가능.

## 사용자 정의 DSL 만들기

- 리시버를 사용하는 함수 타입에 대한 개념을 이해해야 함.
- 함수 타입은 함수로 사용할 수 있는 객체를 나타내는 타입.
- 함수 타입 종류
    - () -> Unit - 아규먼트를 갖지 않고, Unit을 리턴하는 함수입니다.
    - (Int) -> Unit - Int를 아규먼트로 받고, Unit을 리턴하는 함수입니다.
    - (Int) -> Int - Int를 아규먼트로 받고, Int를 리턴하는 함수입니다.
    - (Int, Int) -> Int - Int 2개를 아규먼트로 받고, Int를 리턴하는 함수입니다.
    - (Int) -> () -> Unit - Int를 아규먼트로 받고, 다른 함수를 리턴하는 함수입니다. 이 때 다른 함수는 아규먼트로 아무것도 받지 않고, Unit을 리턴합니다.
    - (0- > Unit) -> Unit - 다론 함수를 아규먼트로 받고, Un it을 리턴하는 함수
입니다. 이 때 다른 함수는 아규먼트로 아무것도 받지 않고, Un it을 리턴합
니다.

- 함수 타입을 만드는 방법
    - 람다표현식
    - 익명함수
    - 함수레퍼런스

### 예시 1)

프로퍼티 타입이 지정되어 있으므로, 람다와 익명함수의 아규먼트 타입을 추론할 수 있음.

```kotlin
fun plus(a: Int, b: Int ) = a + b
```
- 람다
```kotlin
val plus1: (Int, Int) ->Int = { a, b -> a + b }
```
- 익명
```kotlin
val plus2: (Int , I nt) ->I nt = fun(a, b) = a + b
```
- 함수 레퍼런스
```kotlin
val plus3: (Int , Int )->Int = ::plus
```

### 예시2)

1번과 다르게 아규먼트 타입을 지정하여 함수 형태를 추론하게 할 수 있음

```kotlin
val plus4 = { a: Int, b: Int -> a + b }
```
```kotlin
val plus5 = fun( a: Int , b: Int) = a + b
```

- 함수타입은 `함수를나타내는객체`를 표현하는 타입.
- 익명 함수는 일반적인 함수처럼 보이지만, 이름을 갖고 있지 않음.
- 람다 표현식은 익명 함수를 짧게 작성할 수 있는 표기방법.


---

- 확장함수는 어떻게 만들 수 있는가?
```kotlin
fun Int.myPlus(other : Int) = this + other
```

- 익명 확장 함수는 어떻게 만들 수 있는가?
```kotlin
val myPlus = fun Int.(other: Int) = this + other
```

- 위 함수의 타입은?
    - 확장 함수를 나타내는 특별한 타입이 되며, 이를 리시버를 가진 함수 타입 이라고 부름. 
    - 일반적인 함수 타입과 비슷하지만, 파라미터 앞에 리시버 타입이 추가되어 있으며, 점(.) 기호로 구분되어 있음.

- 이와 같이 함수는 람다식, 구체적으로 리시버를 가진 람다 표현식을 사용해서
정의할 수 있음.
    - 스코프 내부에 this 키워드가 확장 리시버를 참조 가능.
    ```kotlin
    val myPlus : Int.(Int) -> Int = { this + it }
    ```

- 리시버를 가진 익명 확장 함수와 람다 표현식을 호출하는 방법
    - 일반적인 객체처럼 invoke 메서드를 사용
    - 확장함수가 아닌 함수처럼 사용
    - 일반적인 확장 함수처럼 사용.

- 리시버의 특징은 위와 같이 this의 참조 대상을 변경 할 수 있다는 점.
- this는 apply 함수에서 리시버 객체의 메서드와 프로퍼티를 간단하게 참조할 수 있음.

- 리시버를 가진 함수는 DSL을 구성하는 가장 기본적인 블록
- DSL은 다음과 같온 것을 표현하는 경우에 유용.
    - 복잡한자료구조
    - 계층적인구조
    - 거대한양의 데이터