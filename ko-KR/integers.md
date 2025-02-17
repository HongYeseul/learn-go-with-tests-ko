# 정수 (Integers)

**[이 챕터의 모든 코드는 여기에서 확인할 수 있습니다.](https://github.com/MiryangJung/learn-go-with-tests-ko/tree/master/integers)**

정수는 예상한 대로 동작합니다. 시도하기 위해 `Add`함수를 작성합니다. `adder_test.go`라는 테스트 파일을 생성한 후 이 코드를 작성합니다.

**주의:** Go의 소스 파일은 각 폴더에 하나의 `package`만 가질 수 있습니다. 파일이 별도로 구성되어 있는지 확인해야 합니다. [여기에 좋은 예시가 있습니다.](https://dave.cheney.net/2014/12/01/five-suggestions-for-setting-up-a-go-project)

## 테스트 먼저 작성하세요.

```go
package integers

import "testing"

func TestAdder(t *testing.T) {
	sum := Add(2, 2)
	expected := 4

	if sum != expected {
		t.Errorf("expected '%d' but got '%d'", expected, sum)
	}
}
```

`%q`가 아닌 `%d`로 형식 문자열을 사용하고 있음을 알 수 있습니다. 왜냐하면 문자열보다는 정수를 출력하기 원하기 때문입니다.

또한 더 이상 main 패키지를 사용하지 않으며 대신 `integers`라는 패키지를 정의했습니다. 이름에서 알 수 있듯이 `Add`와 같은 정수를 처리하기 위한 함수를 그룹화합니다.

## 테스트를 시도해보세요.

`go test` 명령어를 이용해 테스트를 실행합니다.

컴파일 에러를 확인할 수 있습니다.

`./adder_test.go:6:9: undefined: Add`

## 테스트를 실행할 최소한의 코드를 작성하고 실패한 테스트 출력을 확인하세요.

컴파일러를 만족시킬 수 있는 최소한의 코드를 작성합니다. _그것이 전부입니다._ - 우리의 테스트 테스트가 올바른 이유로 실패하는 것을 확인하길 원하는 것을 기억해야 합니다.

```go
package integers

func Add(x, y int) int {
	return 0
}
```

두 개 이상의 같은 타입의 인자를 갖는다면 \(지금 케이스에서는 두 개의 정수\) `(x int, y, int)`보다 `(x, y int)`로 짧게 할 수 있습니다.

이제 테스트를 실행하고 테스트가 올바르게 잘못된 내용을 보고하고 있다는 사실에 만족해야 합니다.

`adder_test.go:10: expected '4' but got '0'`

[이전](hello-world.md#one...last...refactor?) 섹션에서는 *명명된 반환 값*에 대해 배웠지만 여기서는 동일한 값을 사용하지 않습니다. 일반적으로 결과의 의미가 컨텍스트에서 명확하지 않을 때 사용해야 하며, 이 경우에는 `Add`함수에 매개변수를 추가하는 것이 매우 명확합니다. 자세한 내용은 [여기](https://github.com/golang/go/wiki/CodeReviewComments#named-result-parameters) 위키를 참조할 수 있습니다.

## 통과할 만큼 충분한 코드를 작성하세요.

테스트 주도 개발의 가장 엄격한 의미에서 이제 *테스트를 통과하기 위한 최소한의 코드를 작성*해야 합니다. 지나치게 규칙을 찾는 프로그래머는 이렇게 할 것입니다.

```go
func Add(x, y int) int {
	return 4
}
```

아하! 또 망쳤습니다, 테스트 주도 개발은 엉터리가 맞죠?

우리는 또 다른 테스트를 작성할 수 있습니다. 몇 가지 다른 숫자를 가지고 그 테스트가 강제적으로 실패하도록 할 수 있습니다. 그러나 그것은 너무 [고양이와 쥐의 게임](https://en.m.wikipedia.org/wiki/Cat_and_mouse)처럼 느껴집니다.

일단 우리가 Go의 문법에 익숙해지면 개발자를 짜증 나게 하는 것을 멈추고 버그를 찾는 것을 도와주는 *"속성 기반 테스팅"*이라는 기술을 소개할 것입니다.

이제 제대로 코드를 고칩니다.

```go
func Add(x, y int) int {
	return x + y
}
```

테스트를 다시 실행하면 통과해야 합니다.

## 리팩토링

_실제_ 코드에는 우리가 실제로 개선할 수 있는 것이 많지 않습니다.

우리는 이전에 반환 인자의 이름을 문서에 표시했지만 또한 대부분의 개발자의 텍스트 편집기에도 표시하는 방법을 탐구했습니다.

이것은 작성 중인 코드의 편리함에 도움이 되기 때문에 좋습니다. 사용자는 타입 시그니처와 문서만 보고 코드를 사용하는 방법을 이해할 수 있는 것이 좋습니다.

설명과 함께 함수에 문서를 추가할 수 있으며, 표준 라이브러리의 문서를 볼 때와 마찬가지로 Go Doc에 문서가 보입니다.

```go
// Add는 두 개의 정수를 사용하고 합을 반환합니다.
func Add(x, y int) int {
	return x + y
}
```

### 예시

만약 더 멀리 가고 싶다면 [예시](https://blog.golang.org/examples)를 만들 수 있습니다. 표준 라이브러리의 문서에는 많은 예시가 나와 있습니다.

종종 README 파일과 같이 코드 베이스 밖에서 발견될 수 있는 코드 예제는 확인받지 못하기 때문에 실제 코드와 비교하여 오래되고 정확하지 않은 경우가 많습니다.

Go 예시는 테스트와 마찬가지로 실행되기 때문에 코드에서 실제로 수행하는 작업을 확실히 반영할 수 있습니다.

예시는 패키지의 테스트 모임의 일부로 컴파일 \(또한 선택적으로 실행\) 됩니다.

일반적인 테스트와 마찬가지로 예시는 패키지의 `_test.go`파일에있는 함수입니다. `adder_test.go`파일에 아래의 `ExampleAdd`함수를 추가합니다.

```go
func ExampleAdd() {
	sum := Add(1, 5)
	fmt.Println(sum)
	// Output: 6
}
```

(텍스트 편집기가 패키지를 자동적으로 가져오지 않으면 `adder_test.go`에 `import "fmt"`가 누락되어 컴파일 단계가 실패합니다. 어떤 편집기를 사용하던지 이러한 종류의 오류를 자동으로 수정하는 방법을 조사하는 것이 좋습니다.)

코드가 변경되어 예시가 더 이상 유효하지 않으면 빌드가 실패합니다.

패키지의 테스트 모임을 실행하면 추가적인 정리 없이 예시 함수가 실행되는 것을 볼 수 있습니다.

```bash
$ go test -v
=== RUN   TestAdder
--- PASS: TestAdder (0.00s)
=== RUN   ExampleAdd
--- PASS: ExampleAdd (0.00s)
```

`// Output: 6` 주석을 제거하면 함수가 컴파일되지만 예시 함수가 실행되지 않습니다.

이 코드를 추가하면 `godoc` 내부의 문서에 예시가 표시되어 코드에 더 쉽게 접근할 수 있습니다.

이것을 시도하려면 `godoc -http=:6000`을 실행하고 `http://localhost:6060/pkg/`로 이동합니다.

`$GOPATH`의 모든 패키지 목록이 표시되므로 `$GOPATH/src/github.com/{your_id}`와 같은 곳에 코드를 작성했다고 가정하면 예시 문서를 찾을 수 있습니다.

예시와 함께 코드를 공개된 URL에 게시하는 경우 [pkg.go.dev](https://pkg.go.dev/)에서 코드 문서를 공유 할 수 있습니다. 예를 들어, [이것](https://pkg.go.dev/github.com/quii/learn-go-with-tests/integers/v2)은 이 챕터의 최종 API입니다. 이 웹 인터페이스를 사용하면 표준 라이브러리 패키지 및 서드 파티 패키지의 문서를 검색 할 수 있습니다.

## 마무리

우리가 다룬 내용은 아래와 같습니다.

-   테스트 주도 개발 워크플로우의 더 많은 연습
-   정수, 더하기
-   코드 사용자가 사용법을 빠르게 이해할 수 있도록 더 나은 문서 작성
-   테스트의 일부로 확인되는 코드 사용 방법의 예시
