# 상속

> 추가 또는 재정의 하는 하위 클래스
> 
- 하위 클래스: SubClass
- 상위 클래스: SuperClass

### Base Class

- 다른 클래스의 상속되지 않는 클래스를 기본 클래스(Base Class)라고 합니다.
- 범용 기본 클래스를 상속하지 않습니다. 상위 클래스 지정 없이 정의한 클래스는 자동적으로 빌드 할 때 기본 클래스가 됩니다.
- 타입 이름 뒤에 빈 소괄호를 작성한 초기화 구문으로 새로운 인스턴스를 생성

```swift
let a = AClass()
```

### SubClassing

- 기본 클래스를 기반으로 새로운 클래스를 만드는 작업입니다.
- 기존 클래스의 특성을 상속하므로 수정할 수 있습니다.
    - ~~하지만..이는 리스코프 치환 원칙이 위반됩니다~~
    - 상속된 프로퍼티를 사용할 수 있습니다.

### Overriding

> 하위 클래스는 상속 클래스에서 상속할 인스턴스 메서드, 타입 메서드, 인스턴스 메서드, 타입 메서드 또는 스크립트 자체를 제공해줄 수 있습니다. 이것을 재정의라고 합니다.
> 
- 상속된 내용을 가져올 때는 super라는 접두사를 사용해서 상위 클래스의 지정된 내용을 가져올 수 있습니다.
- 재정의된 프로퍼티는 재정의한 getter 또는 setter 구현 내에서 `**super.함수`** 로 상위 클래스 버전의 함수를 접근할 수 있습니다
- 재정의된 서브 스크립트는 재정의한 서브 스크립트 구현 내에서 `super[someIndex]` 로 상위 클래스 버전의 같은 서브 스크립트에 접근할 수 있습니다.

```swift
class Test {
	public func testCode() {
		print("안녕")
	}
}

class TestView: Test {
	override func testCode() {
		super.testCode() // 출력 안녕
	}
}
```

### Overriding Methods

```swift
class Test {
	public func testCode() {
		print("안녕")
	}
}

class TestView: Test {
	override func testCode() {
		print("testCode") // 출력 testCode
	}
}
```

위 코드는 메서드를 재정의하는 방법입니다.

### Overriding Properties

- getter와 setter 재정의
    - 상속된 프로퍼티에 저장된 또는 계산된 특성은 하위 클래스에서 알 수 없습니다.
    - 상속된 프로퍼티의 특정 이름과 타입만을 알고있습니다.
    - 프로퍼티 재정의 부분에서 setter를 제공한다면 getter도 제공할 수 있습니다.
        - getter 내에서 상속된 프로퍼티의 값을 수정하고 싶지 않다면, super.testCode()를 통해서 반환하면 상속된 값을 쉽게 전달할 수 있습니다.

```swift
class Car: Vehicle {
    var gear = 1
    override var description: String {
        return super.description + " in gear \(gear)"
    }
}
```

### 재정의 방지

> final 표시를 통해서 재정의를 방지할 수 있습니다.
> 

방지 방법

- final var
- final func
- final class
- final subscript

하위 클래스에서 final 메서드, 프로퍼티, 또는 서브 스크립트를 재정의 하면 컴파일 시 컴파일에러가 발생합니다.

클래스를 정의할 때 class 키워드 앞에 final 키워드를 표기하면 final로 전체 클래스를 표시할 수 있습니다.
위와 같이 한다면 하위 클래스의 모든 시도는 컴파일에러가 납니다.