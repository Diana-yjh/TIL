# 📝 TIL - 240522 CleanArchitecture Ch.3
해당 글은 마틴 파울러의 `클린아키텍처 3부 - 설계 원칙` 을 토대로 작성되었습니다.


## 학습 내용
[1. SRP(단일 책임 원칙)](#-1-SRP단일-책임-원칙)</br>
[2. OCP(개방 폐쇄 원칙)](#-2-OCP개방-폐쇄-원칙)</br>
[3. LSP(리스코프 치환 원칙)](#-3-LSP리스코프-치환-원칙)</br>
[4. ISP(인터페이스 분리 원칙)](#-4-ISP인터페이스-분리-원칙)</br>
[5. DIP(의존성 역전 원칙)](#-5-DIP의존성-역전-원칙)</br>
[6. 참고 자료](#6-참고-자료)

</br>

SOLID 원칙은 함수와 데이터 구조를 클래스로 배치하는 법, 그리고 이들 클래스를 서로 결합하는 법을 설명한다.</br>
SOLID 원칙의 목적은 아래와 같다.</br>
- 변경에 유연하다
- 이해하기 쉽다
- 많은 소프트웨어 시스템에 사용될 수 있는 컴포넌트의 기반이 된다

</br>

### ✅ 1. SRP(단일 책임 원칙)
Single Responsible Principle의 약자인 SRP는 __단일 모듈의 변경 이유는 하나, 오직 하나뿐이여야 한다__ 라는 의미를 가지고 있습니다.</br>
즉 __하나의 모듈은 하나의, 오직 하나의 액터에 대해서만 책임져야한다.__ 라는 의미이죠.</br>

여기서 모듈은 소스 파일을 의미하며 단일 액터를 책임지는 코드를 함께 묶어주는 힘을 우리는 __응집성__ 이라고 합니다.</br>

SRP의 위반 사례로는 아래와 같은 것이 있다.</br>

#### 1. 사례 1: 우발적 중복

<img src = "https://github.com/Diana-yjh/TIL/blob/main/Resources/CleanArchitecture_Ch3/CleanArchitecture_Ch3_SRP1.png" width = "350"/></br>

위 이미지에서 `Employee` 클래스는 하나의 클래스 내부의 세 가지 메소드가 서로 다른 액터를 책임지기 때문에 SRP를 위반한다.</br>
서로 다른 액터를 책임지는 메서드가 하나의 클래스 내부에 묶여버린 것이다.</br>

<img src = "https://github.com/Diana-yjh/TIL/blob/main/Resources/CleanArchitecture_Ch3/CleanArchitecture_Ch3_SRP2.png" width = "350"/></br>

여기서 `calculatePay` 메소드와 `reportHours` 메소드는 편의 메소드 `regularHours`를 공유하고 있습니다.</br>

이때 CFO가 `calculatePay` 내부의 `regularHours`의 수정을 원합니다.</br>
`reportHours`를 맡고 있는 COO는 원하지 않구요.</br>

여기서 바로 문제점이 드러나 버립니다.</br>

CFO의 요청에 따라 `regularHours`가 수정되어 버린다면 COO는 날벼락을 맞고 말죠.</br>

#### 2. 사례 2: 병합
이번에는 CTO가 `Employee` 클래스의 내용을 조금 수정했다고 합시다.</br>
COO도 본인 팀 나름 계획을 세워 `Employee` 수정을 진행하였고요.</br>

이때 역시 동일한 클래스에 서로 다른 액터가 변경을 시도함에 따라 문제가 발생합니다.</br>

#### 3. 해결
우리가 모두 짐작하듯 이 문제의 해결방법은 간단합니다.</br>
각각의 메서드를 각기 다른 클래스로 분리하는 것이죠.</br>

물논 이렇게 하면 개발자가 세 가지 클래스를 인스턴스화 하여 추적해야한다는 번거로움이 발생합니다.</br>

<img src = "https://github.com/Diana-yjh/TIL/blob/main/Resources/CleanArchitecture_Ch3/CleanArchitecture_Ch3_Farcade.png" width = "500"/></br>

이 대책으로 __퍼사드 패턴__ 이 존재합니다.</br>
퍼사드 패턴은 각 클래스의 객체를 생성한 뒤 요청하는 메서드를 가지는 객체로 일을 위임시킵니다.</br>

이렇게 알아본 SRP는 __메서드와 클래스 수준의 원칙__ 입니다.</br>
컴포넌트 수준에서는 __공통 폐쇄 원칙__, 그리고 아키텍쳐 수준에서는 __아키텍처 경계 생성을 책임지는 변경축__ 이 됩니다.</br>

</br>

### ✅ 2. OCP(개방 폐쇄 원칙)
Open-Close Principle의 약자인 OCP는 __소프트웨어 개체는 확장에는 열려있어야 하고 변경에는 닫혀있어야 한다__ 는 의미를 가지고 있습니다.</br>
SOLID 원칙의 가장 근본적인 원칙이라고 할 수 있죠.</br>

OCP의 이해를 돕기 위한 사고실험은 아래와 같습니다.</br>

#### 사고 실험
예시를 하나 들어봅시다.</br>
재무재표를 웹페이지에 보여주는 시스템이 있습니다.</br>
웹 페이지는 데이터 스크롤, 음수는 빨강으로 표시 하는 기능을 가지고 있죠.</br>

여기서 우리는 해당 페이지의 데이터를 페이지 번호가 있고, 페이지마다 적절한 머리글과 바닥글이 있고, 표의 각 열에는 레이블이 있으며 음수는 괄호로 감싼 형태의 보고서를 출력받기 원한다고 하자.(~~많이도 바란다~~)</br>
이 경우 우리는 거의 모든 코드를 새로 짜야합니다.</br>

이렇게 비효율적인 상황을 방지하기 위해서 우리가 택할 수 있는 방법은 __서로 다른 목적으로 변경되는 요소를 적절히 분리하기(SRP)__, __이들 요소 사이의 의존성을 체계화 허가(DIP)__ 가 있습니다.</br>
이렇게 클래스를 단위로 분할하고 이들 클래스를 이중선으로 표시한 컴포넌트 단위로 구분한 것이 아래와 같습니다.</br>

<img src = "https://github.com/Diana-yjh/TIL/blob/main/Resources/CleanArchitecture_Ch3/CleanArchitecture_Ch3_OCP1.png" width = "500" /></br>

위 구조는 `Controller`, `Interactor`, `Database` 그리고 `Presenter`와 `View`를 담당하는 컴포넌트들로 구성되어 있습니다.</br>
여기서 화살표들은 __의존성__ 을 나타내는데 모든 화살표는 이중선과 한 방향으로만 교차하고 있습니다.</br>
즉 모든 컴포넌트의 관계는 __단방향__ 으로 이루어진다는 것이죠.</br>

위 그림을 단순화 하여 정리하면 아래와 같아집니다.</br>

<img src = "https://github.com/Diana-yjh/TIL/blob/main/Resources/CleanArchitecture_Ch3/CleanArchitecture_Ch3_OCP2.png" width = "450" /></br>

위의 그림을 보면 모든 화살표는 `Interactor`로 흐르며 이것은 즉 `Interactor`는 다른 컴포넌트의 변경으로부터 자유로우며 제일 고수준의 __업무 규칙__ 을 포함한 컴포넌트임을 의미합니다.</br>

결국 OCP는 아키텍트의 기능이 어떻게, 왜, 언제 발생하는지에 따라 기능적으로 분리하고 이를 컴포넌트의 계층 구조로 조직화 한다는 것이죠.</br>
이를 통해 우리는 __저수준 컴포넌트의 변경으로부터 고수준 컴포넌트를 보호__ 할 수 있습니다.</br>

! 위 그림에서 Gateway 인터페이스의 존재는 __의존성 역전__ 을 위한 것.</br>
! Requester는 Controller가 Entities에 대해 너무 많이 아는 것을 막아 추이 종속성을 방지하기 위한 것.(내부 은닉)</br>

</br>

### ✅ 3. LSP(리스코프 치환 원칙)

<img src = "https://github.com/Diana-yjh/TIL/blob/main/Resources/CleanArchitecture_Ch3/CleanArchitecture_Ch3_LSP.png" width = "450"/></br>

위의 그림은 `License` 클래스를 호출하는 `Billing` 어플리케이션을 나타내고 있습니다.</br>
여기서 `License` 클래스는 LSP를 준수하죠.(~~냅다 결론~~).</br>

그 이유는 `Billing`이 `License` 하위타입에 의존하는게 아닌 상위타입인 `License` 자체에 의존하고 있기 때문에 `Personal License`와 `Business License`는 언제나 치환될 수 있기 때문입니다.</br>

이해가 안된다면 또 다른 예시인 __정사각형/직사각형 문제__ 를 살펴보겠습니다.</br>

#### 정사각형 / 직사각형 문제

<img src = "https://github.com/Diana-yjh/TIL/blob/main/Resources/CleanArchitecture_Ch3/CleanArchitecture_Ch3_RectangleSquare.png" width = "450"/></br>

여기서 `User`은 `Rectangle(직사각형)` 클래스를 호출할 수 있고 `Rectangle`의 하위타입으로 `Square(정사각형)`가 존재하고 있습니다.</br>

```swift
var rectangle = ...
rectangle.setWidth(5)
rectangle.setHeight(2)
print(rectangle.area() == 10)
```

이때 위와 같은 코드가 존재한다고 할 때 위 코드의 `Rectangle` 자리에 `Square`가 들어간다면 잘 작동할까요?</br>
물논 답은 "아니다" 입니다.</br>
`Rectangle`과 달리 `Square`은 높이와 너비가 동일하다는 특성을 가지고 있고 높이와 너비를 어떻게 설정하든 하나의 설정은 또 다른 하나로 변경되어버릴 것입니다.</br>
즉 `print`문은 25나 4라는 말도안되는 결과를 내뱉게 되죠.</br>
이 경우 우리는 __Rectangle은 LSP를 만족하지 않는다__ 라고 결론 지을 수 있습니다.</br>

POP(객체지향 프로그래밍)이 처음 나타났던 초창기에는 이 대안책으로 __상속__ 이 제안되었지만 이후 인터페이스와 구현체에도 적용되는 더 광범위한 소프트웨어 설계원칙이 지향됩니다.</br>

</br>

### ✅ 4. ISP(인터페이스 분리 원칙)

<img src = "https://github.com/Diana-yjh/TIL/blob/main/Resources/CleanArchitecture_Ch3/CleanArchitecture_Ch3_ISP.png" width = "400"/></br>

위의 그림을 보면 각각의 유저들은 OPS 클래스 내부의 메소드를 사용하기 위해 OPS 클래스를 호출하고 있습니다.</br>
이때 유저가 하나의 메소드만 사용한다고 해도 OPS 클래스 자체를 호출하고 있으므로 다른 메소드 들에 대해서도 의존성이 생겨버리죠.</br>
이렇게 불필요한 의존성으로 인해 OPS 내부 메소드의 변경이 일어날 때마다 모든 유저는 코드를 재 컴파일하여 사용해야합니다.</br>
이는 아래와 같이 오퍼레이션을 분리함으로써 해결할 수 있다.</br>

<img src = "https://github.com/Diana-yjh/TIL/blob/main/Resources/CleanArchitecture_Ch3/CleanArchitecture_Ch3_ISP2.png" width = "400"/></br>

이때 고려해야할 점은 동적 타입 언어(파이썬, 루비 등)은 런타임에서 추론이 발생하므로 재컴파일과 재배포가 필요 없어 위와 같은 상황에서 자유롭습니다.(하지만 Swift는 정적 언어 ^^)</br>
그리고 같은 정적 언어라고 해도 ISP는 언어 종류에 따라 영향받는 정도가 다릅니다.</br>

결론은 정적인 언어에서 필요 이상으로 많은 것을 포함하고 있는 모듈에 의존하는 것은 해로운 일이라는 것 입니다.</br>

</br>

### ✅ 5. DIP(의존성 역전 원칙)
의존성 역전 원칙은 __소스코드의 의존성이 추상(Abstraction)에 의존하며 구체(Concretion)에는 의존하지 않는 시스템__ 을 지향합니다.</br>

뛰어난 소프트웨어 설계자와 아키텍트들은 인터페이스의 변동성을 낮추기 위해 애쓰며 인터페이스의 변경 없이 구현체에 기능을 추가하고자 노력하죠.</br>
이것이 소프트웨어 설계의 기본입니다.</br>

DIP에서 전달하고자 하는 내용은 아래와 같습니다.</br>

#### DIP 실천법
- __변동성이 큰 구체 클래스는 참조하지 마라__: 대신 추상 인터페이스 참조를 지향하며 일반적으로는 추상 팩토리를 사용하도록 강제합니다.
- __변동성이 큰 구체 클래스로부터 파생하지 마라__: 정적 타입 언어에서 상속은 강력한 동시에 변경하기 어렵습니다. 따라서 상속은 신중하게 사용해야 합니다.
- __구체 함수를 오버라이드 하지 마라__: 구체함수는 소스코드 의존성을 필요로 합니다. 이러한 구체함수를 오버라이드 하는 경우 의존성을 상속해버리기 때문에 대신 추상함수를 선언하고 구현체들에게 각자의 용도에 맞게 구현하도록 지시하는 것이 지향됩니다.
- __구체적이며 변동성이 크다면 절대로 그 이름을 언급하지 마라__: 이는 DIP 그 자체를 설명하는 내용 입니다.

#### 팩토리
위의 실천법을 준수하기 위해서 변동성이 큰 구체적인 객체의 생성은 늘 주의가 필요합니다.</br>
이를 위해서 우리는 __추상 팩토리__ 를 사용할 수 있습니다.</br>

<img src = "https://github.com/Diana-yjh/TIL/blob/main/Resources/CleanArchitecture_Ch3/CleanArchitecture_Ch3_DIP.png" width = "400"/></br>

여기서 곡선은 아키텍처 경계를, 화살표는 소스코드의 의존성을 의미하며 곡선은 구체 컴포넌트와 추상 컴포넌트를 분리합니다.</br>
여기서 주의해야할 점은 소스 코드의 제어흐름은 의존성과 반대 방향으로 곡선을 가로지릅니다.</br>
이를 우리는 __의존성 역전__ 이라고 부르죠.</br>

그럼 위 그림을 좀 더 구체적으로 알아볼까요?</br>

위 그림에서 `Application`은 `Concrete Impl`을 사용하고자 합니다.</br>
결국 `Concrete Impl`의 인스턴스를 생성해야 하는데 그러면 의존성이 생겨버리므로 최대한 지양하고자 하죠.</br>
해결방법으로 `Application`은 `ServiceFactory` 인터페이스에서 `makeSvc` 메소드를 호출합니다.</br>
이 메소드는 `ServiceFactoryImple`에서 구현되며 `ServiceFactoryImple` 구현체는 `ComcreteImpl`의 인스턴스를 생성한 뒤 `Service` 타입으로 반환하죠.</br>

위 그림에는 구체 컴포넌트에 대한 구체 의존성이 하나 존재합니다.</br>
이는 주로 `main` 함수를 포함하여 Main이라고 부릅니다.</br>

이렇게 우리는 소스코드에서 DIP 위배는 모두 없앨 수는 없지만 최대한 지양하고자 합니다.</br>

DIP는 아키텍처 다이어그램에서 가장 눈에 드러나는 규칙입니다.</br>
여기서 고려해야 할 점은 의존성은 아키텍처 경계 곡선을 기준으로 더 추상적 엔티티가 있는 쪽으로만 흐른다는 것입니다.</br>
우리는 이를 __의존성 규칙__ 이라고 부릅니다.</br>

</br>

### 6. 참고 자료
Clean Architecture - 3부
