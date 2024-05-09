# Optional

> 값이 있을 수도 있고 없을 수도 있습니다.
> 

null safety를 보장하는 Swift는 nil의 가능성을 가지고 있는 변수를 나타내기 위해서 사용하고 readability와 code dfficiency에 도움을 줍니다.

optional의 가장 큰 장점은 compiletime에서 잡을 수 있는 에러의 종류를 늘려주니  개발자가 싫어할 이유가 없습니다.

다른 언어들을 null이라고 하지만 swift에서는 nil로 표현됩니다.

- 값이 할당되지 않은 상태
- 기본타입에도 사용 가능

옵셔널 타입

- 일반 변수/상수: nil 대입이 불가능합니다.
- 옵셔널 타입: nil 선언 가능합니다.

```swift
var myOptional: Int?
```

?를 사용해 optional로 변수를 선언해준다는 것을 확인했습니다.

변수 선언시 nil이 허용되지 않기 때문에이다.

```swift
var testNil1: Int = nil // 에러
var testNil2: Int? = nil
```

Optional 변수 타입을 사용하기 위해선 optional binding (if, guard) 등을 사용해야합니다. 이때 nil이나 nil이 아니라면 생략해서는 안됩니다.

참고로 아래 코드는 둘다 똑같은 뜻입니다.

```swift
var test1: Int? = nil
var test2: Optional<Int> = nil
```

## !와 ?의 차이점

! = Implicitly Unwrapped Optional (암시적 추출 옵셔널)

? = Optional

!는 Optional이 아닌 일반 값과 연산이 가능하다. 즉 !로 옵셔널을 선언한 변수의 경우 개발자가 알아서 nil이 아닌 경우에만 이를 사용할 것이라고 믿는 것이다.

!는 runtime error의 가능성이 있습니다.
?로 선언한 경우는 다른 일반 값들과 연산이 불가능하다.

## Optional Binding

> null인지 확인하고 아니라면 값을 사용하는 방법
> 

### 1. if-let을 사용하는 방법

```swift
var UserName: String! = nil

if let name: String = UserName {
	print(name)
} else {
	print("nil")
}
```

if-let 문법하는 법

1. if-let문 안에서만 사용가능한 상수 하나를 만들고
2. 그 상수를 non-nullable type으로 선언해서
3. 만약 Optional 변수와 타입이 매칭되면 첫번째 중괄호를 실행하고, 아니라면 두번째 else 중괄호를 실행하는 식으로 null check를 한다.

이러한 과정을 if let 블록 안에 여러개의 Optional 변수를 바인딩 할 수 있다.

## 2. Force Unwrapping

Optional이 아닌 것처럼 사용 가능하기에, ?로 선언된 optional 뒤에 !를 붙여서 사용하면 된다.

방법 그대로 강제로 Optional이란 껍질을 언래핑해버린다.

```swift
var name: String? = "junha"
print(name!)
```

!의 경우에는 값이 있다고 확신이 들었을 경우가 아니고 nil이 발생하면 터집니다.

|  | 공통점 | 차이점 | 차이점 | 차이점 | 차이점 |
| --- | --- | --- | --- | --- | --- |
| guard let | Optional binding | 전역변수 | else 생략 불가 | 코드가 간결해진다 | return, break,continue,throw등의 ‘제어문 전환 명령을 사용해야함 |
| if let | Optional binding | 지역변수 | else 생략 가능 | 코드가 복잡해진다 |  |

### 옵셔널 뜯어보자

이제 옵셔널이 어떤 것인지 이해하셨나요?

그러면 지금부터 저와 함께 옵셔널의 내부 구조를 공부해봅시다.

옵셔널의 내부 구조는 제네릭 enum입니다.

```swift
public enum Optional<Wrapped>: ExpressibleByNilLiteral {
	case none
	case some(Wrapped)
}
```

1. 값이 있는 경우 (Some): 옵셔널 내부에는 값이 있을 수 있습니다. 이 경우 값을 저장하고 해당 값을 가져올 수 있습니다.
2. 값이 없는 경우 (None): 옵셔널 내부에는 값이 없을 수도 있습니다. 이 경우 옵셔널은 nil 값을 가지며, 이를 통해 값이 없음을 나타냅니다.