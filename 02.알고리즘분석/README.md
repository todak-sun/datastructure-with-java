List 인터페이스의 구현체인 ArrayList, LinkedList는 상황에 따라 더 효율적으로 동작한다.

이 중 무엇을 선택할지 결정하는 방법 중 하나로, 둘 다 시도해보고 각각 얼마나 걸리는지 알아보는 **프로파일링(profiling)** 이 있다. 하지만 이는 아래와 같은 문제점을 가진다.

- 알고리즘을 비교하려면 모두 구현해야함
- 결과는 하드웨어의 SPEC에 의존함
- 데이터에 의존함

알고리즘 분석(analysis of algorithm)을 사용하여 이러한 문제를 해결할 수 있다. 이 전에, 아래의 가정이 필요하다.

- 컴퓨터 하드웨어의 세부사항을 다루지 않기 위해, 보통 알고리즘을 이루는 더하기와 곱하기, 숫자 비교 등의 기본연산을 식별한다. 그리고 각 알고리즘에 필요한 연산 수를 센다.
- 입력 데이터의 세부사항을 다루지 않기 위해, 기대하는 입력 데이터에 대한 평균 성능을 분석한다. 또는 최악의 시나리오를 분석한다.
- 한 알고리즘이 작은 문제에서는 최상의 성능을 보이지만, 큰 문제에서는 다른 알고리즘이 적합할 수 있다는 가능성을 여러둔다. 이때에는 큰 문제에 초점을 맞추로독 한다.

알고리즘을 나타내는 범주
- 상수시간 : 실행시간이 입력 크기에 의존하지 않을 때. 배열(Array)의 인덱스로 접근하는 행위.
- 선형: 실행시간이 입력의 크기에 비례. 
- 이차: 실행시간이 n^2^에 비례.

# 선택 정렬

```java
import java.util.Arrays;

public class SelectionSort {
  // array에 i, j 인덱스의 위치를 바꾼다.
	public static void swapElements(int[] array, int i, int j) {
		int temp = array[i];
		array[i] = array[j];
		array[j] = temp;
	}

	// start부터 시작해 가장 작은 값을 찾는다.
	public static int indexLowest(int[] array, int start) {
		int lowIndex = start;
		for (int i = start; i < array.length; i++) {
			if (array[i] < array[lowIndex]) {
				lowIndex = i;
			}
		}
		return lowIndex;
	}

	// 선택정렬을 수행하는 메서드
	public static void selectionSort(int[] array) {
		for (int i = 0; i < array.length; i++) { 
      // array의 길이만큼 반복하며
			int j = indexLowest(array, i); 
      // array 내부에서 i부터 가장 작은값을 가지는 요소의 인덱스를 받아오고
			swapElements(array, i, j); 
      // 교체한다.
		}
	}

	public static void main(String[] args) {
		int[] array = {2, 5, 6, 1, 3};
		selectionSort(array);
		System.out.println(Arrays.toString(array));
    // 결과 : [1, 2, 3, 5, 6]
	}
}
```

# 빅오 표기법

모든 상수 시간 알고리즘은 O(1)이라고 표현할 수 있다.
모든 선형 알고리즘은 O(n)이라고 표현할 수 있다.
모든 이차 알고리즘은 O(n^2^)으로 표현할 수 있다.

# 실습

자바 배열을 사용하여 요소를 저장하는 List 인터페이스를 구현해보자.

```java

public class MyArrayList<T> implements List<T> {
    int size;             // 현재 가지고 있는 요소들의 개수(크기)
    private T[] array;    // 실제로 가지고 있는 요소의 배열

		@Override
    public boolean add(T element) {
        if (size >= array.length) {
            // 큰 배열을 만들고 요소들을 복사한다.
            T[] bigger = (T[]) new Object[array.length * 2];
            System.arraycopy(array, 0, bigger, 0, array.length);
            array = bigger;
        }
        array[size] = element;
        size++;
        return true;
    }
		
		@Override
    public T get(int index) {
        if (index < 0 || index >= size) {
            throw new IndexOutOfBoundsException();
        }
        return array[index];
    }
}
```

### add 메서드가 true를 반환하는 이유
공식문서에서는 List의 add메서드가 어떠한 요소룰 추가하는 작업을 통해 내부의 변화가 생긴다면 true를, 그렇지 않다면 false를 리턴해야한다고 설명한다.

때문에 구현체가 이를 어떻게 구현하느냐에 따라 true 또는 false를 리턴하게 될수도 있다. 만약, 필요에 의해 구현한 List의 구현체가 특정한 값(예를들면 null)을 받지 않도록 되어있다면, 요소에 추가하지 않기 때문에 내부 상태의 변화로 이어지지 않고, false를 반환할 것이다.

> 참고 : [공식문서](https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html#add-java.lang.Object-)

### set 메서드
공식문서에서는 List의 set 메서드가 특정한 위치에 존재하는 요소를 대체한다고 설명한다. 또, 반환하는 값은 대체하기 전 해당 인덱스에 존재하던 요소로 한다고 설명하고 있다.

따라서, 다음과 같이 구현할 수 있다.
```java
@Override
public T set(int index, T element) {
		T old = get(index);
		array[index] = element;
		return old;
}
```

[자바 공식문서](https://docs.oracle.com/javase/8/docs/api/java/util/List.html#set-int-java.lang.Object-)

