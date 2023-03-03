## 코드가 더러워지는 원인

1. 메서드가 여러가지 다른 일을 수행한다.
2. 낮은 수준의 원시연산(배열 조작, 산술연산 등)을 수행한다.
3. 주석과 적절한 메서드명과 변수명 같이 사람이 읽을 수 있는 무언가가 부족하다.

## 규칙 1. 다섯줄 제한

- 메서드는 중괄호`{}`를 제외하고 5줄 이상이 되면 안된다.
- 20줄의 하나의 메서드보다는 각각 5줄을 갖는 4개의 메서드가 훨씬 보기 편하다.
- 이분탐색과 같이 줄여나가자
  - 20줄의 코드가 있는 상황
  - 처음 10줄, 마지막 10줄로 2분할한다. 이때 주석을 활용한다.
  - 다시 나눈 것을 각각 5줄이 되도록 2분할한다.
  - 주석의 의미를 함수명으로 바꾸자
- `if`의 일부 분기만 `return` 문을 가지고 있을 경우 메서드를 추출하는데 방해가 될 수 있다.
  - 이럴 경우 메서드의 끝에서 시작해 위로 작업해나가자
  - 이때, `if`, `else if`,`else`구조를 분할하면 안된다.
- 매개 변수중 하나를 반환값으로 할당해야하는 경우
  1. 새로운 메서드의 마지막에 `return p;`
  2. 새로운 메서드를 호출하는 쪽에서 `p = newMethod();`를 호출한다.

### 예시) 배열의 최소항목 찾기

```java
static void minimum(int[][] arr){
	int result = Integer.MAX_VALUE;
	for(int x=0; x<arr.length; x++){
		for(int y =0; y<arr.length; y++){
			if(result>arr[x][y]){
				result = arr[x][y];
			}
		}
	}
	return result;
}
```

```java
static void minimum(int[][] arr){
	int result = Integer.MAX_VALUE;
	for(int x=0; x<arr.length; x++){
		for(int y =0; y<arr.length; y++){
				result = updateMin(result, arr, x,y,)
		}
	}
}

static int updateMin(int result, int[][] arr, int x, int y){
	if(result>arr[x][y]){
		result = arr[x][y];
	}
	return result;
}
```

## 규칙 2. 호출과 전달 중 하나만 하자

- **함수 내에서 가능한 행위**
  1. 객체에 있는 메서드를 호출
  2. 객체를 인자로 전달

     **⇒ 단, 둘 다 하면 안된다.**
- 많은 메서드를 도입하고, 많은 매개변수를 전달하기 시작하면 책임이 고르지 않게 될 수 있음

### 예제). 배열의 평균을 구하는 함수

```java
public int average(int[] arr){
	return sum(arr)/arr.length;
}
```

- 높은 수준의 추상화 `sum(arr)`와 낮은 수준의 추상화 `arr.length`를 전부 사용함
- 함수의 추상화는 동일한 추상화 수준에 있어야한다.
  - 로버트 c. 마틴의 책 clean code를 참조

### 함수의 추상화 레벨이란?

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/92ea8d1f-f212-4cb3-a1e5-fae7555f13e6/Untitled.png)

- 함수 내부 행동들의 추상화 레벨을 의미함

  - 일종의 계층이라 이해하면 편하다.

- 높은 추상화
  ```java
  getHtml()
  getFile()
  ```
- 중간 추상화
  ```java
  String pagePathName = Path.Parser.render(pagePath)
  ```
- 낮은 추상화
  ```java
  .append("\n")
  ```
- 함수 내부 행동들의 추상화 레벨이 섞여있다면, 소스를 읽을 때 코드를 읽는 사람이 해당 함수가 어느 범위까지 구현되었는지 헷갈린다.
- 추상화는 최대한 높은 수준을 지향하고, 중간은 지양하자.

## 좋은 함수 이름의 속성

1. 함수의 의도를 정직하게 설명해야한다.
2. 함수가 하는 모든 것을 완전히 담아야한다.
3. 도메인에서 일하는 사람(=실제로 쓰는 사람)이 이해할 수 있어야한다.
   - 작업 중인 도메인에서 흔히 쓰는 단어를 사용할 것
   - 팀원, 고객 등 코드에 대해 이야기할 때 더 쉽게 이해할 수 있다.

## 규칙3. if문은 함수의 시작에만 배치하자

- if문이 있는 경우 해당 if문은 함수의 첫번째 항목이여야 한다.

### 예제 2~n까지의 모든 소수를 출력하는 함수

```java
public void reportPrimes(int n){
	for(int i=2; i<n; i++){
		if(isPrime(i)){
			System.out.println(i);
		}
	}
}
```

- 함수는 두가지 행위를 한다.
  1. 숫자를 반복한다.
  2. 숫자가 소수인지 확인한다.

```java
public void reportPrimes(int n){
	for(int i=2; i<n; i++){
		reportIfPrime(i);
	}
}

public void reportIfPrime(int n){
	if(isPrime(n)){
		System.out.println(n);
	}
}
```
