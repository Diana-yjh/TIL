# 📝 TIL - 240528 The ModularArchitecture

해당 글은 Tuist 버전 4 를 기준으로 작성되었습니다.</br>

## 학습 내용
[1. The ModularArchitecture?](#-1-The-ModularArchitecture란?)</br>

### 1. The ModularArchitecture란?
The ModularArchitecture은 Tuist3 버전의 MicroFeatures가 rename된 것 입니다.</br>
The modularArchitecture의 핵심 아이디어는 명확하고 간결한 API를 사용하여 상호 연결된 독립적인 기능을 구축하는 것 입니다.</br>
즉 TMA를 사용하면 개발자들은 main앱과는 별개로 그들이 구현하고자 하는 Feature을 더욱 빠르게 빌드, 테스트 그리고 적용할 수 있어야 합니다.</br>

그럼 TMA에서 말하는 모듈이란 무엇일까요?</br>

TMA에서의 모듈은 애플리케이션의 Feature를 나타내며 아래 다섯 Target으로 구성되어 있습니다.</br>
- __Source__: Feature 소스 코드(Swift, Objective-C, C++ ...) 또는 리소스(이미지, 폰트 등) 등 을 포함하고 있습니다.
- __Interface__: Feature의 Public 인터페이스와 모델들을 포함한 조력 타겟입니다.
- __Tests__: Feature 유닛과 테스트 코드를 포함하고 있습니다.
- __Testing__: 테스트에 사용할 수 있는 테스트 데이터를 가지고 있습니다. 모듈 클래스나 프로토콜을 위해 mock을 제공합니다.
- __Example__: 특정 조건에서 Feature을 테스트 해볼 수 있도록 예시 앱을 포함하고 있습니다.(다른 언어, 화면 크기, 세팅 등의 조건)

또한 각 타겟에는 해당 타겟에 맞는 네이밍이 제안됩니다.</br>

|Target|Dependencies|Content|
|---|---|---|
|Feature|FeatureInterface|소스코드와 리소스|
|FeatureInterface|-|Interface와 모델|
|FeatureTest|Feature, Feature Testing|유닛과 실행 테스트|
|FeatureTesting|Feature Interface|테스팅 데이터와 목(mock)|
|FeatureExample|Feature Testing, Feature|테스트 앱|
