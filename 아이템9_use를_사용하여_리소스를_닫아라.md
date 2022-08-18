## use를 사용하여 리소스를 닫아라.

- AutoCloseable을 상속받는 Closeable 인터페이스를 구현(implement)하고 있습니다.
- 더 이상 필요하지않다면, 명시적으로 close 메서드를 호출해주는 것이 좋습니다.
- use라는 이름의 함수로 포함되어 있습니다. use 함수를 사용해서 앞의 코드를 적절하게 변경하면, 다음과 같습니다. 이러한 코드는 모든 Closeable 객체에 사용할 수 있습니다.

```kotlin
fun countCharactersinfile ( path : String ) : Int {
    val reader = BufferedReader( FileReader(path) )
    reader. use {
        return reader.lineSequence().sumBy { it.length }
    }
}
```

### 정리
- use를 사용하면 Closeable/AutoCloseable을 구현한 객체를 쉽고 안전하게
처리할 수 있습니다. 또한 파일을 처리할 때는 파일을 한 줄씩 읽어 들이는 uselines를 사용하는 것이 좋습니다.