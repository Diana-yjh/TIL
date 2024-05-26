# 📝 TIL - 240525 Factory Method

## 학습 내용

[1. 팩토리 메소드 패턴이란?](#-1-팩토리-메소드-패턴이란)</br>
[2. 팩토리 메소드 패턴 예시](#-3-팩토리-메소드-패턴-예시)</br>
[3. 팩토리 메소드 패턴 구현](#-3-팩토리-메소드-패턴-구현)</br>
[4. 팩토리 메소드 패턴의 장단점](#-4-팩토리-메소드-패턴의-장단점)</br>
[5. 참고 자료](#5-참고-자료)</br>

</br>

### 1. 팩토리 메소드 패턴이란?

팩토리 메소드는 부모 클래스에서 객체를 생성할 수 있는 인터페이스를 제공하고 자식 클래스들이 생성될 객체들의 유형을 변경할 수 있도록 하는 생성 패턴입니다.</br>

간단히 이야기하면 **객체의 생성을 부모 클래스가 아닌 자식 클래스에서 하겠다** 라는 의미입니다.</br>
팩토리 메소드의 구조는 아래와 같습니다.</br>

<img src = “https://github.com/Diana-yjh/TIL/blob/main/Resources/FactoryMethod/FactoryMethod.png?raw=true” width = “450” /> </br>

Swift에서 인터페이스는 Protocol 이라고 생각할 수 있습니다.</br>
위 구조에서 Creator과 Project는 추상 타입(Abstract Type)이며 구현에 대한 청사진만을 가지고 있습니다.</br>

실제 구현은 각각의 구체 타입(Concrete Type)에서 하죠.</br>
그래서 각각의 역할을 알아보면

- Product
    모든 Creator가 할 행위에 대한 청사진을 가지고 있습니다.  
- Concrete Product
    Product 인터페이스에 구현된 청사진에 대한 구체적 구현입니다.
- Creator
    새로운 객체를 반환하는 팩토리 메소드를 선언합니다.   
- Concrete Creator
    Creator에서 구현한 메소드에 실제로 객체를 생성하여 넣어주는 구체적 구현입
    

</br>

### 2. 팩토리 메소드 패턴 예시
하나의 예시를 들어보겠습니다.</br>
우리가 운송회사를 운영하고 있다고 합니시다. 우리에게는 트럭이 한대 있죠.</br>
이 트럭은 주차장에 멈춰있기도 하고 고속도로를 달려 배송을 해주기도 하는 등 다양한 역할을 수행합니다.</br>

그리고 몇년 후 열심히 운송회사를 운영한 결과 회사의 규모가 커졌습니다.</br>
이제 해외에서도 고객들이 제품 배송을 원하게 되었어요.</br>
해외 고객들에게 제품을 배송해주기 위해 우리는 배를 사용해야 하는 상황이 되었습니다.</br>
하지만 우리가 여태 구축해 놓은 배송 방법과 과정들은 모두 육지 배송을 하는 트럭에 맞춰져 있죠.</br>
변경이 불가능합니다.</br>

이러한 상황에서 우리는 `Factory Method` 를 사용할 수 있습니다.</br>

`Factory Method`에서 우리는 트럭 객체를 생성할 수 있으며 배를 추가 할 떄는 역시 `Factory Method`에 코드를 추가해주면 됩니다.</br>

결국 객체에 수정이 생기는 경우 `Factory Method`만 수정하면 되는 것이죠.</br>

### 3. 팩토리 메소드 패턴 구현
그럼 위의 예시를 `Swift`로 구현해보겠습니다.</br>
우선 우리는 객체의 행위를 정의해줄 인터페이스가 필요합니다.</br>
우리는 이를 `(Abstract) Product`라고 하기로 했죠.</br>

```swift
protocol Transport {
    func loadStuff() // 물건 적재
    func sortStuff() // 물건 분류
    func deliver() // 배송
}
```

</br>
그 다음 우리는 이렇게 정의한 일을 해줄 객체를 생성합니다.</br>
우리는 이를 `Concrete Product`라고 합니다.</br>
Product 인터페이스에 명시된 일을 구체화 한다는 것이죠.</br>

```swift
struct Truck: Transport { // Truck
    func loadStuff() {
        print("Truck load stuff")
    }

    func sortStuff() {
        print("Truck sort stuff")
    }

    func deliver() {
        print("Truck deliver")
    }
}
```

```swift
struct Ship: Transport { // Ship
    func loadStuff() {
        print("Ship load stuff")
    }

    func sortStuff() {
        print("Ship sort stuff")
    }

    func deliver() {
        print("Ship deliver")
    }
}
```

</br>

이렇게 객체 구현은 끝이 났습니다.</br>
이제 우리는 이 객체를 인스턴스화하여 사용하기만 하면 되는데 이 인스턴스화를 대신해주는 것이 `Factory Method` 입니다.</br>
`Factory Method`도 물논 형식만 명시해주는 `(Abstract) Factory Method`가 존재합니다.</br>

```swift
protocol TransportFactory { // Abstract Factory
    func createTransport() -> Transport // 객체를 생성한 뒤 리턴해야하므로 Transport 리턴타입
} 
```

이후 이 `(Abstract) Factory`에서 명시한 메소드를 직접 구현하는 `Concrete Factory`까지 구현하면 Factory Pattern에 대한 구현은 끝이 납니다.</br>

```swift
struct TruckFactory: TransportFactory { // Truck에 대한 Concrete Factory
    func selectTransport() -> Transport {
        return Truck()
    }
}
```

```swift
struct ShipFactory: TransportFactory { // Ship에 대한 Concrete Factory
    func selectTransport() -> Transport {
        return Ship()
    }
}
```

이렇게 완성된 팩토리 메소드는 아래와 같이 사용할 수 있습니다.</br>

```swift
struct Client {
    func selectTransport(with factory: TransportFactory) -> Transport {
        return factory.selectTransport()
    }
}

let client = Client()

let truckTransport = client.selectTransport(with: TruckFactory())
let shipTransport = client.selectTransport(with: ShipFactory())

tructTransport.loadStuff()
shipTransport.loadStuff()
```

</br>

### 4. 팩토리 메소드 패턴의 장단점
그럼 이런 `Factory Method Pattern`을 사용했을 때 우리가 얻을 수 있는 장점에는 뭐가 있을까요?</br>
__장점__ </br>
- 객체를 생성하는 부분과 코드를 구체화한 부분 사이의 결합을 줄일 수 있습니다.
- 객체 생성부를 분리함으로써 Single Responsibility Principle을 만족시킵니다.</br>
- 존재한 코드를 변경하지 않고 새로운 객체를 추가할 수 있으므로 Open/Closed Principle을 만족시킵니다.</br>
__단점__ </br>
- 코드를 적용하기 위해서는 수많은 서브클래스를 구현해야 하기 때문에 코드가 복잡해질 수 있습니다.</br>

이렇게 오늘 다양한 디자인 패턴의 기본이 되기 때문에 반드시 숙지해야하는 `Factory Method` 패턴에 대해 알아보았습니다.</br>

### 5. 참고 자료
[Refactoring GURU - Factory Method](https://refactoring.guru/design-patterns/factory-method)</br>
[Pingu - Factory Method Pattern](https://icksw.tistory.com/237)</br>
