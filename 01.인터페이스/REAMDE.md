# 리스트가 두 종류인 이유

List의 구현체
- ArrayList
- LinkedList

어떤 동작은 ArrayList가 빠르거나, 저장공간을 적게 사용한다.  
어떤 상황에서는 LinkedList가 빠르거나, 메모리 사용량이 적다.

어느것이 좋은지는 수행하는 동작에 달려있다.

# 자바 interface

자바의 interface는 메서드의 집합을 의미한다.  
이를 구현(implement)하는 클래스는 이러한 메서드를 제공해야한다.

## 예시

```java
public interface Comparable<T> {
  public int compareTo(T o);
}
```

```java
public final class Integer extends Number implements Comparable<Integer> {
  
  @Override
  public int compareTo(Integer anotherInteger) {
    int thisVal = this.value;
    int anotherVal = anotherInteger.value;
    return (thisVal<anotherVal ? -1 : (thisVal= =anotherVal ? 0 : 1));
  }

  // 다른 메서드 생략
}
```

# List Interface
interface는 List가 된다는 의미가 무엇인지를 정의한다.   
List를 구현하는 클래스는 add, get, remove와 같은 약 20가지의 메서드를 모두 제공해야한다.

ArrayList와 LinkedList는 이러한 메소드를 제공하므로 상호 교환 가능하다.

## ListClientExample
```java
public class ListClientExample {
  
  private List list; // 1. List를 캡슐화
  
  public ListClientExample() {
    list = new LinkedList(); //2. 캡슐화 된 List에 LinkedList 인스턴스를 담는다.
  }

  private List getList() {
    return list; // List 객체에 대한 참조를 반환
  }
  
  public static void main(String[] args) {
    ListClientExample lce = new ListClientExample();
    List list = lce.getList();
    System.out.println(list);
  }
}
```
위 ListClientExample은 LinkedList나 ArrayList와 같은 구현 클래스를 직접 사용하지 않고, 가능한 List 인터페이스를 사용한다.

만약, ArrayList를 활용하고자 한다면, 생성자만 바꿔주면 된다.

이러한 스타일을 인터페이스 기반 프로그래밍(interface-based programming) 또는 인터페이스 프로그래밍이라고 한다. (자바 뿐 아니라, 일반적인 개념의 인터페이스를 의미)

코드는 List와 같은 인터페이스만 의존하고  
ArrayList와 같은 구현체에는 의존하지 않아야 한다. 이러한 방식으로 개발을 한다면  
구현이 변경되어도 인터페이스를 사용하는 코드는 그대로 사용할 수 있다.

# 실습

```java
public class ListClientExample {
	@SuppressWarnings("rawtypes")
	private List list;

	@SuppressWarnings("rawtypes")
	public ListClientExample() {
		list = new LinkedList();
	}

	@SuppressWarnings("rawtypes")
	public List getList() {
		return list;
	}

	public static void main(String[] args) {
		ListClientExample lce = new ListClientExample();
		@SuppressWarnings("rawtypes")
		List list = lce.getList();
		System.out.println(list);
	}
}
```
```java
	@Test
	public void testListClientExample() {
		ListClientExample lce = new ListClientExample();
		@SuppressWarnings("rawtypes")
		List list = lce.getList();
		assertThat(list, instanceOf(ArrayList.class) ); // 실패
		assertThat(list, instanceOf(LinkedList.class) ); // 성공
	}
```