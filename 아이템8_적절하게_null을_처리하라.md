## 적절하게 null을 처리하라.

- null은 최대한 명확한 의미를 갖는 것이 좋습니다.
  - ?．,스마트캐스팅 , Elvis 연산자등을 활용해서 안전하게 처리한다.
  - 오류를 throw한다.
  - 함수 또는 프로퍼티를 리팩터링해서 nullable 타입이 나오지 않게 바꾼다.


### 오류 throw하기
- printer가 null 이 되리라 예상하지 못했다면, print 메서드가 호출되지 않아서 이상할 것입니다.

### not-null assertion(!！）과 관련된 문재
- nullable을 처리하는 가장 간단한 방법은 not-null assertion(!！）을 사용하는 것입니다.
- !！은 사용하기 쉽지만, 좋은 해결 방법은 아닙니다.
- !！온 타입은 nullable이지만, null이 나오지 않는다는것이 거의 확실한 
상황에서 많이 사용됩니다
- 지연 세팅이 필요한 경우에는 lateinit 또는 Delegates.notNull을 사용하는 것입니다.
- 명시적 오류는 제네릭 NPE보다는 훨씬 더 많온 정보를제공해 줄수 있으므로 !! 연산자를 사용하는 것보다는 훨씬 좋습니다.

### 의미 없는 nullability 피하기
- 보기에 의미가 없을때는 null을 사용하지 않는것이 좋습니다.
- 요소가 부족하다는 것을 나타내려면, 빈 컬렉션을 사용하세요.

### lateinit 프로퍼티와 notNull 델리게이트
- nullable to not null의 코드들은 테스트 전에 설정될거라는 것이 명확하므로, 의미없는 코드가 사용된다고 할 수 있습니다.
- lateinit 한정자를 사용하는 것입니다. lateinit 한정자는 프로퍼티가 이후에 설정될 것임을 명시하는 한정자입니다.
- 만약 초기화 전에 값을 사용하려고 하면 예외가 발생합니다.
- 처음 사용하기 전에 반드시 초기화가 되어 있울 경우에만 lateinit를 붙이는 것입니다.
  - 프로퍼티가 초기화된 이후에는 초기화되지 않은 상태로 돌아갈 수 없습니다.

- 반대로 lateinit를 사용할 수 없는 경우도 있습니다. JVM에서 Int, Long ,
Double, Boolean과 같은 기본 타입과 연결된 타입으로 프로퍼티를 초기화해야
하는 경우입니다. 이런 경우에는 lateinit보다는 약간 느리지만, Delegates.
notNull을 사용합니다.
