# 📝 TIL - 240521 DependencyInjection
## 학습 내용
[1. Dependency Injection이란?](#1-Dependency-Injection이란)</br>
[2. Dependency Injection의 사용](#2-Dependency-Injection의-사용)</br>
[3. Dependency Injection 정리](#3-Dependency-Injection-정리)</br>
[4. 참고 자료](#4-참고-자료)</br>

## 🎯 학습 목표
|상태|목표|
|---|---|
|✅|의존성 주입에 대해 설명할 수 있다.|
|✅|의존성 주입이 코드에 미치는 효과를 설명할 수 있다.|
|✅|의존성 주입을 실제 코드에 적용할 수 있다.|
|✅|의존성 주입을 위한 Protocol의 사용법을 설명할 수 있다.|
|✅|의존성 주입을 적용하기 위해 신경써야하는 사항들을 설명할 수 있다.|

</br>

### 1. Dependncy Injection이란?
Dependency Injection, 즉 의존성 주입은 객체의 생성 단계에서 독립성을 확보해줌으로써 코드의 재사용성을 높이고 의존성을 낮추는 기법입니다.</br>
그럼 코드의 의존성이란 어떤걸 의미하는 걸까요?</br>

```swift
class Dog {
    var sound: String
    
    init(sound: String) {
        self.sound = sound
    }
    
    func dogSound() -> String {
        "\(self.sound) 하고 짖는다."
    }
}

class Maltiz {
    let dog = Dog(sound: "Wang")
    
    func dogSound() -> String {
        return dog.dogSound()
    }
}
```

위의 예시를 보면 Maltiz 클래스는 Dog에 의존하고 있습니다.</br>
위의 코드를 따르면 세상 모든 말티즈는 Wang 하고 짖어야 하며 예외는 존재할 수 없죠.</br>

그러면 의존성을 줄여주기 위해 어떻게 해야할까요?</br>

### 2. Dependency Injection의 사용

#### 1. 생성시 의존성 주입
우선 첫 번째 방법으로는 의존성을 생성시에 넣어주는 방법이 있습니다.</br>

```swift
class Maltiz {
    let dog: Dog
    
    init(dog: Dog) {
        self.dog = dog
    }
    
    func dogSound() -> String {
        return dog.dogSound()
    }
}

Maltiz(dog: Dog(sound: "Wang"))
Maltiz(dog: Dog(sound: "Ang"))
```

코드가 변경되면서 이제 말티즈는 Wang 할 수도 있고 Ang 할 수도 있게 되었습니다.</br>
좀더 다양해졌죠?</br>

하지만 여전히 Maltiz는 Dog에 의존성을 가지고 있습니다.</br>
정말 별난 친구가 있어서 짖지 않고 야옹하고 우는 말티즈는? 존재할 수 없죠.</br>

이떄 우리는 의존성 주입을 위해 Swift에서 제공하는 강력한 기능인 __Protocol__ 을 사용할 수 있습니다.</br>

#### 2. 프로토콜의 사용

```swift
protocol Animal {
    var sound: String { get set }
    
    func animalSound()
}
```

위와 같이 동물이라는 프로토콜을 생성합니다.</br>
이제 동물은 개가 될 수도 있고 고양이가 될 수도 있습니다.</br>
우리 말티즈가 좀 고양이 같을 수 도 있죠(?)</br>

```swift
class Dog: Animal {
    var sound: String
    
    init(sound: String) {
        self.sound = sound
    }
    
    func animalSound() {
        print("\(self.sound)하고 짖습니다")
    }
}

class Cat: Animal {
    var sound: String
    
    init(sound: String) {
        self.sound = sound
    }
    
    func animalSound() {
        print("\(self.sound)하고 울어요")
    }
}

class Maltiz {
    var animal: Animal
    
    init(animal: Animal) {
        self.animal = animal
    }
    
    func animalSound() {
        animal.animalSound()
    }
}
```

이제 Maltiz의 animal에 무엇이 들어가느냐에 따라 우리들의 말티즈는 짖을수도 있고 울수도 있게 되었습니다.</br>

### 3. 의존성 주입 정리
위에서 살펴보았듯이 의존성 주입을 사용하면 우리는 여러 이점을 얻게 됩니다.</br>
- __테스트 용이:__ 의존성 주입을 통해 독립적이 된 코드들은 결국 유닛테스트에서 빛을 발하게 됩니다.
  반대로 의존성을 신경쓰지 않고 짜여진 코드는 종종 테스트 불가능한 코드가 되어버리고 말죠.
- __유연성과 독립성:__ 의존성 주입은 컴포넌트들 간의 강한 연결을 줄여줍니다.
- __가독성과 유지보수성 향상:__ 의존성 주입으로 인해 정리된 코드는 이전보다 가독성과 유지보수성이 향상됩니다.
- __재사용성:__ 의존성 주입이 적용된 코드는 서로 결합되어 있지 않다보니 어디서든 용이하게 재사용될 수 있습니다.

그러면 의존성 주입을 적용하기 위해 우리가 신경써야하는 것은 무엇이 있을까요?
- __프로토콜 사용:__ 위에 예시도 들었듯이 Swift에서 의존성 주입하면 제일 우선 떠오르는 것은 프로토콜입니다.
- __생성 단계에서의 주입:__ 초기화 단계에서 의존성을 주입하는 것은 의존성을 명확하고 확실하게 해줍니다.
- __Service Locator 패턴 지양:__ 서비스 로케이터 패턴은 숨은 의존성을 만들어 개발자의 코드에 대한 이해를 어렵게 만듭니다.
- __강한 의존성 지양:__ Concrete 클래스에 의존하는 것을 지양하고 추상화(프로토콜)에 의존하는 것이 좋습니다.
- __테스트 가능성:__ DI는 유닛테스트를 용이하게 해야 합니다.
- __SOLID 원칙 따르기:__ SOLID 원칙을 신경쓰고 그 중 Dependency Inversion에 주의하세요.
- __의존성 순환 지양:__ 의존성 순환은 이후 초기화에서 문제를 발생시킬 수 있습니다.
- __복잡한 초기화 단계에선 Factory를 사용:__ 복잡한 객체를 생성하는 경우 Factory를 통해 생성하는 것을 고려하는 것이 좋습니다.
- __코드 리뷰 고려:__ 전체적 코드 단계에서 DI가 잘 지켜지고 있는지 코드 리뷰를 종종 진행하는 것이 좋습니다.

와우 생각보다 많네요!</br>
이렇게 오늘은 의존성 주입(Dependency Injection)에 대해 알아보았습니다.</br>

### 4. 참고 자료
[Design Patterns Explained - DI](https://stackify.com/dependency-injection/)</br>
[Dependency Injection in Swift: RealWorld Coding Example](https://vikramios.medium.com/dependency-injection-in-swift-37b44b53fc24)</br>
[Dependency Injection in Swift](https://www.linkedin.com/pulse/dependency-injection-swift-get-set-go-evangelist-apps-limited-dk18e#:~:text=In%20the%20context%20of%20Swift,modular%2C%20testable%2C%20and%20maintainable.)</br>
[망나니 개발자 - 의존성 주입](https://mangkyu.tistory.com/150)</br>

