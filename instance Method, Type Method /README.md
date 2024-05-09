# instance Method, Type Method

### 참고로 인스턴스란?

- 클래스, 구조체, 열거형에서 생성된 객체를 인스턴스라고 부릅니다.
- 붕어빵 틀인 클래스로 찍어낸 맛있는 붕어빵이 인스턴스입니다.

> Method란?
인스턴스 혹은 타입에 속하는 함수를 호출하는 것
> 
- 클래스나 구조체에 정의되어있는 함수를 통틀어 메서드라고 부른다.

Swift에는 Instance Method, Type method가 존재합니다.
메서드는 특정 타입들과 관련이 있는 함수입니다. 클래스, 구조체, 열거형은 모두 주어진 타입의 인스턴스와 작용할 수 있도록 캡슐화된 인스턴스 메서드를 선언할 수 있다. 또한 클래스, 구조체, 열거형은 타입 그 자체와 연관된 타입 메서드도 선언할 수 있다. 타입 메서드는 objc의 클래스 메서드와 비슷합니다.

Swift에서는 구조체와 열거형이 메서드를 선언할 수 있다는 점이 c와 objc의 가장 큰 차이가 있습니다. objc에서는 클래스가 메서드를 선언할 수 있는 유일한 타입입니다. Swift에서는 클래스를 선택하든, 구조체를 선택하든, 열거형을 선택하든 여전히 메서드를 선언할 수 있는 다양한 방법이 있습니다.

### 인스턴스 메서드

- 특정 유형의 인스턴스에 속하는 메서드이며 해당 유형의 인스턴스에서만 호출 할 수 있다.
- 인스턴스 메서드를 정의하려면 먼저 메서드가 속한 클래스, 구조체 또는 열거형 내에서 메서드를 선언해야한다.

```swift
class MyClass {
	func myMethod() {
		
	}
}

let myObject = MyClass()
myObject.myMethod()
```

myMethod는 MyClass 클래스의 메서드이며 myMethod 메서드를 호출하기 위해선 MyClass의 인스턴스인 myObject를 선언하고 접근해야합니다.

또한 인스턴스 메서드는 함수처럼 파라미터를 갖고 값을 반환할 수 있고 인스턴스의 속성 및 기타 인스턴스 메서드에 접근하고 수정할 수 있습니다.

```swift
class User {
	var name: String = "박준하"
	var age: Int = 0
	
	func introduceAge(age: Int) {
		self.age = age
		print("\(name)이고 나이는 \(age)입니다.")
	}
	
	func memoryLoss {
		name = ""
		age = 0
	}
}

let user = User()
user.introduceAge(age: 19)
print(user.age) // 19
user.memoryLoss()
print(user.age) // 0
```

introduceAge 메서드는 age라는 파라미터를 갖고 있고 이를 통해 age의 값이 영양이 미칩니다. introduceAge 메서드 뿐만 아니라 memoryLoss 메서드도 age에 영향을 미치는 모습을 볼 수 있습니다.

### 타입 메서드

인스턴스 메서드와는 다르게 타입 메서드는 인스턴스 없이도 접근이 가능합니다.

타입 메서드는 static을 이용하여 선언할 수 있다. 아까 타입 메서드와 클래스 메서드가 비슷한 역활을 한다고 했을데 클래스 메서드는 class를 func앞에 붙임으로써 선언 가능합니다. 이 둘이 언급되는 이유는 거의 인스턴스 없이 타입 자체 메서드에 바로 접근이 가지고 있다는 공통점이 있지만, override의 기능에서 차이가 있습니다. static은 override가 불가능 하고, class의 경우는 override가 가능합니다.

```swift
class Person {
	class func attack1() {
		print("공격1")
	}
	static func attack2() {
		print("공격2")
	}
}

class Junha: Person {
	override class func attack1() {
		super.attack1()
		print("attack1")
	}
	override static func attack2() {
		super.attack2()
		print("attack2")
	}
}

Person.attack1() // 공격 1
Junha.attack1() // 공격 1 followed by attack1
```

위 코드를 보면 Person 클래스를 상속받은 Junha 클래스에서 attack1() 클래스 메서드는 슈퍼클래스의 클래스 메서드를 오버라이드하여 새로운 print문을 추가할 수 있습니다. 하지만 static 메서드의 경우 상속이 거부된다는 오류가 뜬다. 따라서 같은 접근 방식을 취하지만 상속 받을 일이 없는 경우에는 static, 상속 받을 일이 필요한 경우 class 메서드를 활용하게 된다.

### 타입 메서드 TMI

- 타입 메서드는 유틸리티 함수(프로그램을 개발하는 과정에서 자주 사용되는 기능을 함수로 구현할 것)를 만드는데 종종 사용됩니다.
    - ex) 절대값을 구할 수 있는 abs() 같은 함수가 타입 메서드로 이루어져있다. abs()는 Numeric 프로토콜의 타입 메서드이다.
- 타입 인스턴스의 초기화나 타입 인스턴스를 쉽게 만들 수 있도록 한다.
    - ex) Datㄷ()도 Date class의 타입 메서드이다.

### 참고 문서

[https://whowantstoquit.tistory.com/174](https://whowantstoquit.tistory.com/174)

[https://docs.swift.org/swift-book/documentation/the-swift-programming-language/methods/#Instance-Methods](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/methods/#Instance-Methods)

[https://infinitt.tistory.com/399](https://infinitt.tistory.com/399)