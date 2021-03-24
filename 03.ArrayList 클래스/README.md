분할 상환 분석(amortized analysis)

# MyArrayList 메서드 분류하기

## get
```java
public E get(int index) {
  if (index < 0 || index >= size) {
    throw new IndexOutOfBoundsException();
  }

  return array[index];
}
```
get 메서드는 **상수 시간(O(1))** 이다.

## set
```java
public E set(int index, E element) {
  E old = get(index);
  array[index] = element;
  return old;
}
```
set 메서드도 **상수 시간(O(1))** 이다.
명시적으로 배열의 범위를 검사하지 않고, get 메서드를 통한다.

## equals
```java
private boolean equals(Object target, Object element) {
  if (target == null) {
    return element == null;
  } else {
    return target.equals(element);
  }
}
```
equals 메서드는 **상수 시간(O(1))** 이다.

## indexOf
```java
public int indexOf(Object target) {
  for (int i = 0; i < size; i++) {
    if (equals(target, array[i])) {
      return i;
    }
  }
  return -1;
}
```
배열의 길이만큼 반복문을 돌 며 equals 메서드를 호출한다.   
따라서 **선형(O(n))** 이다.

## remove
```java
public E remove(int index) {
  E element = get(index);
  for (int i=index; i<size-1; i++) {
    array[i] = array[i+1];
  }

  size--;

  return element;
}
```
상수 시간인 get 메서드를 사용하고 index부터 배열에 반복문을 실행한다.
따라서, remove 메서드는 **선형(O(n))** 이다.


# add 메서드 분류하기
## add
```java
public boolean add(E element) {
  if (size >= array.length) {
    // 큰 배열을 만들어 요소들을 복사합니다
    E[] bigger = (E[]) new Object[array.length * 2];
    System.arraycopy(array, 0, bigger, 0, array.length);
    array = bigger;
  }
  array[size] = element;
  size++;
  return true;
}
```
- 배열에 사용하지 않는 공간이 있다면 **상수 시간(O(1))** 이다.
- 배열의 크기를 변경해야 한다면 실행시간이 배열의 크기에 비례하기 때문에 **선형(O(n))** 이다. => System.arraycopy
- 조건에 따라 상수이거나 선형이기 때문에, 일련의 n개 요소를 추가할 때 평균 연산 횟수를 고려하여 메서드를 분류해야한다.

패턴: 어떠한 요소를 n번 추가하면, 요소 n개를 저장하고 n - 2번 복사해야한다. 
따라서 총 연산회수는 2n - 2이며, 이를 평균으로 나타내면 2 - 2/n으로 n이 커질 수록 **상수 시간(O(1))** 에 가깝다고 볼 수 있다.

이렇게, 일련의 호출에서 평균 시간을 계산하는 알고리즘 분류방법을 **분할 상환 분석** 이라 한다.

## 두 인자 버전 메서드 add(int, E)
```java
public void add(int index, E element) {
  if (index < 0 || index > size) {
    throw new IndexOutOfBoundsException();
  }
  // 크기 조정을 통해 요소를 추가합니다
  add(element);
  // 다른 요소를 시프트합니다
  for (int i = size - 1; i > index; i--) {
    array[i] = array[i-1];
  }
  // 올바른 자리에 새로운 값을 넣습니다
  array[index] = element;
}
```
- 단일 인자 버전 메서드인 add(E)를 호출
- 다른 요소를 오른쪽으로 이동
- 올바른 자리에 새로운 요소 삽입

따라서, **선형 시간(O(n))** 이다.

# 문제 크기

## removeAll

```java
public boolean removeAll(Collection<?> collection) {
  boolean flag = false;
  for (Object obj: collection) {
    flag |= remove(obj);
  }
  return flag;
}
```









