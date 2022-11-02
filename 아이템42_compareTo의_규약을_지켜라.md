# 아이템 42 - compareTo의 규약을 지켜라
- compareTo 메서드는 Any 클래스에 있는 메서드가 아니며 수학적인 부등식으로 변환되는 연산자임
- compareTo 메서드는 Cornparable<T> 인터페이스에도 들어 있음
- 어떤 객체가 이 안터페이스를 구현하고 있거나 compareTo라는 연산자 메서드를 갖고 있다는 의미는 해당 객체가 어떤 순서를 갖고 있으므로, 비교할 수 있다
는 것
- compareTo는 다음과 같이 동작해야 함
    - 비대칭적동작: a >= b이고b >= a라면,a == b여야 함
        - 비교와 동둥성 비교에 어떠한 관계가 있어야 하며, 서로 일관성이 있어야 함
    - 연속적동작: a >= b이고b >= C라면, a >= C여야 함
        - 마찬가지로 a > b이고b > C라면,a > C여야 함
        - 이러한 동작을 하지 못하면, 요소 정렬이 무한반복에 빠질 수 있음
    - 코넥스적동작:  두 요소는 어떤 확실한 관계를 갖고 있어야 함
        - 즉, a >= b 또는 b >= a중에 적어도 하나는 항상 true여야 함
        - 두 요소 사이에 관계가 없으면, 퀵 정렬과 삽입 정렬 둥의 고전적인 정렬 알고리즘을 사용할 수 없음
        - 대신 위상 정렬과 같은 정렬 알고리즘만 사용할 수 있음

## compareTo를 따로 정의해야 할까?
- 코틀린에서 compareTo를 따로 정의해야 하는 상황은 거의 없음
- 일반적으로 어떤 프로퍼티 하나를 기반으로 순서를 지정하는 것으로 충분하기 때문
    - sortedBy를 사용하면, 원하는 키로 컬렉션을 정렬할 수 있음
    - 여러 프로퍼티를 기반으로 정렬해야 한다면 sortedWith 함수를 사용
- 객체가 자연스러운 순서인지 확실하지 않다면, 비교기 (comparator)를 사용

## compareTo 구현하기
- compareTo를 구현할 때 유용하게 활용할 수 있는 톱레벨 함수가 있음
- 두 값을 단순하게 비교하기만 한다면, compareValues 함수를 다움과 같이 활용
    ```kotlin
    class User(
    val name: St「ing, val surname: String
    ): Comparable<User> {
        override fun compareTo(other: User): Int =
        compareValues(surname, other.surname)
    }
    ```
- 더 많은 값을 비교하거나, 선택기 (selector)를 활용해서 비교하고 싶다면, 다음 과 같이 compareValuesBy를 사용
- 함수가 다음 값을 리턴해야 한다는 것을 기억
    - 0: 리시버와 other가 갇온 경우
    - 양수: 리시버가 other보다 큰 경우
    - 음수: 리시버가 other보다 작은 경우
