# Extension

> Extension은 이미 존재하는 코드를 확장할 수 있는 기능입니다.
> 

### 특징

- 원본 소스코드에 접근 권한이 없어도 확장할 수 있습니다.
- Extension은 Objective-C에서의 category와 비슷한 개념이나, extension은 이름을 갖지 않는다는 차이가 있습니다.
- 계산 프로퍼티 추가 (저장 프로퍼티 및 프로퍼티 옵저버는 안됩니다.)
- 메서드 추가
- 생성자 추가
- subscript 추가
- Nested type 추가
- 프로토콜 생성
- 기존 기능을 override 하지 못합니다.
- Extension은 런타임에 추가하는 개념이 아닙니다.
    - 어느 코드 위치에 선언하든 해당 타입의 모든 인스턴스에 영향을 미칩니다.
- Protocol의 Extension에서는 Nested Type을 확장할 수 없습니다.

### 저장 프로퍼티는 왜 안될까?

저장 프로퍼티가 늘어난다는 것은 타입이 요구하는 메모리 사이즈가 변경됨을 뜻합니다.
Extension은 일부 코드 영역에만 적용할 수 있으므로 하나의 타입에 대해 각 모듈마다 서로 다른 메모리 사이즈를 요구하게 됩니다.
때문에 컴파일러는 어떤 타입이 요구하는 메모리 사이즈를 정의할 때, 모든 모듈을 탐색해서 이 타입에 대한 extension이 있는지 얼마나의 메모리를 먹는지 계산해야합니다.

컴파일러에게 저장 프로퍼티를 추가하는 것이 부담이 적기 때문에 막았다고합니다.

### Extension 구문

> 기본 형태
> 

```swift
extension Test { }
```

> 프로토콜 채택
> 

```swift
extension Test: TestProtocol { }
```

### 생성자

```swift
// 기존의 Person 클래스 정의
class Person {
    var name: String
    var age: Int

    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
}

// Person 클래스에 새로운 생성자를 추가하는 확장(extension)
extension Person {
    // 새로운 생성자 추가
    convenience init(name: String) {
        // 기존 생성자 호출하여 name을 받고, age는 0으로 초기화
        self.init(name: name, age: 0)
    }
}

// Person 클래스의 인스턴스 생성
let person1 = Person(name: "John", age: 30)
print(person1.name) // 출력: John
print(person1.age)  // 출력: 30

// 확장으로 추가한 생성자를 이용한 인스턴스 생성
let person2 = Person(name: "Alice")
print(person2.name) // 출력: Alice
print(person2.age)  // 출력: 0 (새로운 생성자에서 age를 0으로 초기화함)

```

위 코드에서 **`Person`** 클래스에는 **`name`**과 **`age`**를 받는 초기화 함수가 있습니다. 그리고 확장(extension)에서는 이에 추가적인 생성자를 추가합니다. 새로 추가된 생성자는 이름(**`name`**)만을 받고, 나이(**`age`**)는 기본값으로 0으로 초기화됩니다.

이렇게 확장으로 추가한 생성자는 해당 클래스의 모든 저장 프로퍼티를 초기화해야 하며, 위 예제에서는 기존 생성자를 호출하여 이를 수행하고 있습니다.

### Class는 convenience만 가능하다

Swift에서 클래스는 초기화와 관련하여 특정 규칙을 따릅니다. 특히 designated(지정) 및 convenience(편의) 초기화에 관한 규칙이 중요합니다.

1. **Designated Initializers (지정 초기화)**:
    - 클래스의 기본적인 초기화를 담당합니다.
    - 보통 클래스의 모든 프로퍼티를 초기화합니다.
    - 클래스마다 여러 개의 designated 초기화가 있을 수 있지만, 각 초기화는 모든 프로퍼티를 초기화해야 합니다.
    - 서브클래스는 부모 클래스의 designated 초기화를 반드시 호출해야 합니다.
2. **Convenience Initializers (편의 초기화)**:
    - 초기화 과정을 간소화하거나 다양한 초기화 경로를 제공하는 보조적인 역할을 합니다.
    - 동일한 클래스 내의 다른 초기화 메서드를 호출하거나 일부 프로퍼티를 기본값으로 설정할 수 있습니다.
    - designated 초기화와 달리, convenience 초기화는 동일한 클래스 내의 다른 초기화 메서드를 직접 호출할 수 있으며, 슈퍼 클래스의 designated 초기화도 호출할 수 있습니다.

그러나 extension에서 designated 초기화를 추가할 수 없는 이유 중 하나는, 이로 인해 클래스의 계층 구조가 위협될 수 있다는 점입니다. extension에서 designated 초기화를 추가하는 것은 해당 클래스의 서브클래스가 존재하지 않는 상황에서도 클래스의 초기화 과정을 변경할 수 있게 됩니다. 이는 예상치 못한 동작을 초래할 수 있으므로, Swift는 이를 허용하지 않습니다.

### 다른 모듈에 정의된 struct를 확장할 때의 주의사항

다른 모듈에 정의된 struct를 생성자가 extension 하려면, self에 접근할 수 없는 문제가 발생합니다.

original에 정의된 생성자를 호출하여, 저장 프로퍼티를 모두 초기화한 후에 접근 할 수 있습니다. 이는 class가 convenience를 쓰는 것과 유사한ㅂ니다.

### 메소드

```swift
import UIKit

public extension UIView {
    func addSubviews(_ viewsToAdd: [UIView]) {
        viewsToAdd.forEach({addSubview($0)})
    }
}
```

이런식으로 UIView에 함수를 추가할 수도 있습니다.

### Protocol

Extension의 꽃이라고 하면 Protocol일 것 같은데요?

다로 protocol로 만들어 놓고 다른 class나 struct에서 이를 채택하면, 해당 class에서 반드시 Protocol 내에서 이름과 소괄호로만 선언해 놓았던 함수들을 구현해야했습니다. 하지만 Extension을 사용하면 굳이 구현하지 않아도 사전에 Extension으로 처리해놓을 수 있습니다.

```swift
protocol Protocol1 {
	func func1()
}

extension Protocol1 {
	func func1() {
		print("asdf")
	}
}

class Test: Protocol1 {

	func asdf() {
		func1()
	}
}
```

이렇게 extension으로 분리해서 따로 관리할 수 있습니다.