# 📝 TIL - 240511 SwiftPackageManager

## 학습 내용
[1. SwiftPM이란?](#1-SwiftPM이란?)</br>
[2. 참고 자료](#2-참고-자료)</br>

## 🎯 학습 목표
|상태|목표|
|---|---|
|✅|SwiftPM에 대해 설명할 수 있다.|
</br>

### 1. SwiftPM이란?
SwiftPM(Swift Package Manager)은 Swift 코드의 배포를 관리해주는 하나의 도구입니다.</br>
SwiftPM은 애플의 1st party로써 Cocoapods나 Carthage와는 다르게 Xcode11에 내장되어 애플에서 공식적으로 지원합니다.</br>

SPM은 아래와 같은 개념을 가지고 있습니다.</br>

- Modules
  - Swift는 코드를 모듈로 구성하곤 합니다.</br>
    각각의 모듈은 네임스페이스를 특정하고 접근 제어를 설정하여 모듈 사용을 위해 외부에서 접근할 수 있는 부분을 특정합니다</br>
  - 프로그램은 하나의 모듈 또는 다른 모듈의 의존성을 import 해서 구성되고는 합니다.</br>
    이때 대부분의 의존성들은 사용하기 위해 우리는 해당 코드를 다운받고 설정해야합니다.</br>
    만약 너가 특정 문제 해결을 위해 서로 다른 모듈을 사용한다고 할 때 코드는 다른 상황에서 재사용될 수 있습니다.</br>
    즉 모듈을 사용한다는 것은 새로운 기능을 다시 개발하는 것이 아닌 다른 개발자들의 코드를 기반으로 우리가 개발할 수 있도록 도와줍니다.</br>

- Packages
  - 패키지는 Swift 소스 파일과 Package.swift라는 manifest 파일로 이루어져 있습니다.</br>

- Products
  - 타깃은 library 또는 executable로 빌드될 것입니다.</br>
    그 중 executable은 운영체제에 의해 실행될 수 있는 프로그램을 말합니다.</br>

- Dependencies
  - 타깃의 dependencies는 패키지에서 필요로하는 모듈입니다.</br>
    Dependency는 비교적 또는 절대적인 URL과 사용하기 위한 버전을 위한 요구사항으로 이루어져 있습니다.</br>
    Package Manager의 역할은 프로젝트를 위한 dependencies를 다운로드하고 빌드하는 과정의 비용을 감소시키는대 있습니다.</br>
    이는 재귀적인 형태로 패키지 매니저는 모든 dependency 그래프를 만족시키기 위한 모든 것을 다운로드 하고 빌드합니다.</br>
    
### 2. 참고 자료
[Swift Docs - Package Manager](https://www.swift.org/documentation/package-manager/)
