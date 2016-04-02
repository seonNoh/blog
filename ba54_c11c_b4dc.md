# 메서드
###규칙38 : 인자의 유효성을 검사하라 
메서드나 생성자를 구현할 때는 받을 수 있는 인자에 제한이 있는지 따져봐야 한다. 제한이 있다면 그 사실을 문서에 남기고 메서드 앞부분에서 검사하도록 해야 한다. **인자 유효성을 검사하지 않았을 때 최고로 심각한 유형의 문제는, 메서드가 정상적으로 반환값을 내기는 하지만 어떤 객체의 상태가 비정상적으로 바뀌는 경우다.** 그러면 나중에 해당 메서드와는 아무 상관도 없는 부분에서 오류가 뜨는데, 그 시간과 위치는 프로그램을 실행할 때마다 바뀐다. <br>

public 메서드라면, 인자 유효성이 위반되었을 경우에 발생하는 예외를 Javadoc의 @throws 태그를 사용해서 문서화하라 보통 IllegalArgumentException, IndexOutOfBoundsException, NullPointerException이 이용된다. <br>
public이 아닌 메서드는 일반적으로 인자 유효성을 검사할 때 확증문(assertion)을 이용한다. 
```
// 재귀적으로 정렬하는 private 도움 함수 
private static void sort(long a[], int offset, int length) {
	assert a != null;
	assert offset >= 0 && offset <= a.length;
	assert length >=0 && length <= a.length - offset;
	… // 계산 수행 
}
```
확증문은 클라이언트가 패키지를 어떻게 이용하건 확증 조건은 항상 참이 되어야 한다고 주장하는 것이다. 통상적인 유효성검사와는 달리, 확증문은 확증 조건이 만족되지 않으면 AssertionError를 낸다. **또한 통상의 유효성 검사와는 달리, 활성화되지 않은 확증문은 실행되지 않으므로 비용이 0이다. ** 확증문을 활성화시키려면 java 인터프리터에 -ea(또는 -enableassertions) 옵션을 주어야 한다. <br>

생성자는 클래스 불변식을 위반하는 객체가 만들어지는 것을 막으려면, 생성자에 전달되는 인자의 유효성을 반드시 검사해야 한다. cf) 불변 클래스는 설계, 구현이 쉽다. 객체의 상태를 변경하는 어떤 메서드도 제공안한다. 모든 필드는 fianl. 상속 불가. private 선언. <br>

이번 절에서 다룬 내용을 잘못 받아들여 “인자에 제약을 두는 것은 바람직하다”고 믿어버리면 곤란하다. 메서드는 가능하면 일반적으로 적용될 수 있도록 설계해야 한다. 메서드가 받을 수 있는 읹에 제약이 적으면 적을수록 더 좋다. 

###규칙39 : 필요하다면 방어적 복사본을 만들라 
여러분이 만드는 클래스의 클라이언트가 불변식을 망가뜨리기 위해 최선을 다할 것이라는 가정하에, 방어적으로 프로그래밍해야 한다. 