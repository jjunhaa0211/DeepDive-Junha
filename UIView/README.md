# UIView

View들은 UI 구성에 근보적인 빌딩 블럭입니다.
UIView class는 모든 view를 위한 공통적인 동작을 정의합니다.
하나의 view 객체는 자신의 bounds 사각형에 content를 렌더링하고 관련 상호작용을 처리합니다.
UIView class는 인스턴스화하거나 고정된 배경 색깔을 보이기 위해 사용될 수 있는 concrete class입니다. 또한 좀 더 복잡한 형태의 content를 그리기 위해 subclassing할 수도 있습니다.
label, image, button, 그 외 일반적인 인터페이스 요소들을 표시하기 위해 직접 형태를 만들기보단 UIKit 프레임워크가 제공하는 view subclass들을 사용할 수 있습니다.

## View의 역활

> View 객체들은 App이 유저와 상호작용하는 메인 경로이므로, 아래와 같이 상당히 많은 책임을 갖고 있습니다.
> 
- Drawing / Animation
    - View는 UIkit이나 CoreGraphics를 사용하여 자신의 사각형 영역에 content를 그립니다.
    - 일부 View 프로퍼티를 새로운 값으로 애니메이션할 수 있습니다.
- Layout / Subview 관리
    - View는 subview를 가질 수 있습니다.
    - View는 subview의 크기와 위치를 조정할 수 있습니다.
    - 오토레이아웃을 사용하여 view 계층 변화에 따라 view들을 resizing / repositioning 할 수 있습니다.
- Event Handling
    - View는 UIResponder의 subclass이며 터치와 같은 이벤트들에 응답할 수 있습니다.
    - View는 GestureRecognizer를 도입하여 일반적인 제스처를 처리할 수 있습니다.

## View 계층

View는 다른 view 안에 nest 되어 view 계층은 view 계층을 이룰 수 있습니다. view 계층은 서로 관련된 content를 조직하는 편리한 방법입니다.

view를 nesting 하는 것은 view 간 부모-자식 관계를 형성합니다.
부모 view는 여러 subview를 가질 수 있고 각 subview는 하나의 부모 view를 가집니다.

디폴트로 subview의 보이는 영역은 superview의 bounds를 넘어 확장될 수 있습니다만, clipsToBounds 프로퍼티를 사용하여 그러지 않도록 변경할 수 있습니다.

frame과 bounds 프로퍼티는 각 view의 기하학적 구조를 정의합니다. frame은 superview의 좌표계에서의 origin과 영역을 정의합니다. bounds는 해당 view의 내부적인 영역을 정의하며 custom drawing code와는 거의 배타적입니다.

center 프로퍼티는 frame / bounds를 변경하지 않고 view를 재배치하는 편리한 방법을 제공합니다.

## View 만들어보기

> 코드로 view를 만드는 방법
> 

view를 생성할 때, 일반적으로 superview와의 상대적인 initial size/position을 명시하게 됩니다. 아래 코드는 superview를 기준으로 왼쪽 위 코너에 위치를 하도록 하는 코드입니다.

```swift
let rect = CGRect(x: 10, y: 10, width: 100, height: 100)
let view = UIView(frame: rect)
```

> subview 관리
> 

subview를 추가하기 위해선, superview에서 addSubview를 호출합니다. 개수에 제한이 없고 서로 겹칠 수 있습니다. addSubview는 새로운 view를 기존에 있던 subview들의 top에 위치시키는데 index를 조절하고 싶다면 insertSubview를 사용할 수 있고 또한 이미 추가된 두 subview의 위치를 스왑하고 싶다면 exchageSubview를 사용할 수 있습니다.

view를 생성한 후에는 오토레이아웃을 설정하여 view 계층 변화에 맞춰 size / position이 어떻게 변경될지 정의합니다.

## View Drawing Cycle

### draw()

View를 drawing 하는 것은 필요할 순간에 수행됩니다. view가 처음 보여지거나 레이아웃이 변경되며 전체 혹은 일부가 보여질 때, 시스템은 view에게 자신의 content를 draw할 것을 요청합니다. UIKit 혹은 Core Craphics를 사용한 custom content를 포함한 view에 대해선, 시스템이 draw() 메서드를 출하게 되어 있습니다. 

이 메서드를 현재 graphics context에 view의 content를 그릴 수 있습니다. (current graphics context는 이 메서드가 호출되기 전에 시스템에 의해 자동으로 만들어집니다.)
이것은 content의 정적인 visual 표현을 생성하며 이후 화면에 표시될 수 있습니다.

### Redrawing 요청

만약 content가 변경되어 view를 다시 그리는게 필요하다면, 시스템에게 이를 알리는 것은 개발자의 몫입니다.
이는 setNeedsDisplay() 메서드를 호출하여 가능합니다. 
이 메서드는시스템에게 다음 drawing cycle에 해당 view를 업데이트해야 함을 알립니다. 이를 위해 다음 drawing cycle까지 기다리므로 이 메서드를 여러 view에 대해서 쌓아 한번에 업데이트되게 할 수 있습니다.

<aside>
☝🏿 만약 OpenGL ES를 사용하여 drawing한다면 UIView 대신 GLKView를 사용해야합니다.

</aside>

## Animations

> 일부 view 프로퍼티 변경은 애니메이션화될 수 있습니다. 즉, 프로퍼티를 현재값→새로운값으로 변경하는 행위가 애니메이션을 만들게 됩니다.
> 

애니메이션이 가능한 프로퍼티

- frame
- bounds
- center
- transform
- alpha
- backgroundColor

프로퍼티 변경을 애니메이션화하기 위해선 UIViewpropertyAnimator 객체를 생성하여 handler 블록에서 변경하려는 값을 설정하면 됩니다. UIViewPropertyAnimator class로 애니메이션 duration과 timing을 명시하면 실제 애니메이션을 수행해줍니다. 또한 현재 실행중인 애니메이션을 일시정지하는 것도 가능합니다.

## Threading Considerations

UI를 조작하는 것은 항상 메인 스레드에서 수행되어야 합니다.
즉 UIView class의 메서드를 호출하는 코드는 항상 메인 스레드에서 실행되어야 합니다. 이것이 엄격하게 필요하지 않은 유일한 경우는 view 객체를 생성할 때입니다. 하지만 그 외의 모든 조작은 메인 스레드에서 수행되어야합니다.

## Subclassing Notes

UIView class는 유저 상호작용을 요구하는 visual content를 위한 핵심적인 subclassing 포인트입니다. UIView를 subclassing하는 많은 좋은 이유들이 있지만, 기본 UIView class 혹은 표준 system view가 필요한 기능을 제공하지 못하는 경우에만 subclassing하는 것을 권장합니다. 왜냐하면 subclassing은 view를 구현하는데 더 많은 작업이 필요하고 성능을 조정시키기 때문입니다.

## Methods to Override

UIView를 subclassing할 때, override를 반드시 해야만 하는 메서드는 거의 없습니다. 반면, override가 선택사항으로 제공되는 좋은 메서드가 많습니다.
UIView는 매우 configurable한 class이므로 override 없이 복잡한 행동을 구현하는 많은 방법을 갖고 있기 때문입니다. override를 고려할만한 프로퍼티 / 메서드들은 아래와 같습니다.

### Initalizer

- init(frame:)
    - 기복적으로 사용이 추천되는 메서드입니다.
    - custom code를 override로 대체하거나 추가할 수 있습니다.
- init(coder:)
    - 스토리보드나 nib file로 view를 로드하는 경우에 custom을 위해 사용됩니다.
- layerClass
    - 타임프로퍼티인 layerClass는, view가 자신의 backing store를 위해 다른 Core Animation layer를 사용하길 원할 때 예로, view가 큰 스크롤 영역을 표시하는데 타일 형태를 사용한다면, layerClass 프로퍼티를 CATiledLayer class를 할당할 수 있습니다.

### Drawing / Printing

- draw(_:)
    - 뷰가 custom content를 그릴 때 구현합니다. 만약 view가 어떠한 custom drawing도 하지 않는 다면 override하지 않는게 좋습니다.
- draw(_:for:)
    - 프린터로 프린트할 때 content가 다르게 보이길 원한다면 구현

### Layout / Constraint

- requiresConstraintBasedLayout
    - view class가 절절히 동작하기 위해 constraint가 필요하다면 true로 설정합니다.
- updateConstraints
    - subview간 custom constraint를 생성할 필요가 있을 때 구현합니다.
- alignmentRect(forFrame:) / frame(forAlignmentRect:)
    - view가 다른 view에서 align 되길 원하는 경우 구현합니다.
- willMove(toSuperview:) / didMoveToSuperview()
    - view가 view 계층 내에서의 이동을 축적하려는 경우 구현합니다.

### Event Handling

- gestureRecognizerShouldBegin(_:)
    - view가 터치 이벤트를 직접적으로 처리하거나 부착된 gestureRecognizer가 추가 action을 유발하지 못하게 막는 용도로 구현합니다.
- touchesBegan(_:with:) / touchesMoved(_:with:) / touchesEnded(_:with:) / touchesCancellid(_:with:)
    - 터치 이벤트를 직접적으로 처리하려는 경우 구현합니다.

## 하위 분류

많은 view 동작들이 subclassing 없이도 설정될 수 있습니다. override하기 전에 아래 프로퍼티/메서드를 통해 충분히 원하는 동작을 제공할 수 있는지 고려합니다.

### 프로퍼티 / 메서드로 제공

- addConstraint(_:)
    - view와 subview에 자동적인 레이아웃 동작을 정의합니다.
- autoresizingMask
    - superview의 frame이 변경될 때 자동적인 레이아웃 동작을 제공할 수 있습니다. 이는 constraint와 결합될 수 있습니다.
- contentMode
    - frame과는 반대로, view 자체가 아닌 view의 content에게 레이아웃 동작을 제공합니다. 또한, 이 프로퍼티는 content가 소속 view에 따라 어떻게 스케일될지와 캐싱/redraw 여부에 영향을 미칩니다.
- isHidden / alpha
    - content를 hiding이나 alpha를 적용하는 대신, 해당 content가 소속된 view의 투명도를 변경합니다.
- backgroundColor
    - view의 color를 설정하여 새깔을 직접 그리지 않아도됩니다.

## 구조적으로 제공

- Subviews
    - draw() 메서드로 content를 그리는 대신, 해당 content를 image/label의 subview로 embed합니다.
- Gesture recongnizers
    - 터치이벤트를 직접 가로채어 처리하는 대신, gesture recognizer를 사용하여 target 객체로 Target-Action을 보낼 수 있다.
- Animations
    - 애니메이션을 직접 시도하지 않고 관련 built-in support를 사용합니다. Core Animation에 의해 지원되는 애니메이션은 빠르고 사용하기 쉽니다.
- image-based background
    - 상대적으로 정적인 content로 분류되는 view들에 대해선, subclassing하고 이미지를 직접그리기보단 UIImageView를 gesture recognizer와 함께 사용하는 것을 고려합니다. 다른 대안으로 UIView를 만들어 CALayer 객체의 content로 이미지를 할당할 수도 있습니다.