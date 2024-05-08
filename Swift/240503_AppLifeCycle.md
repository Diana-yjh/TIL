# 📝 TIL - 240503 AppLifeCycle
## 학습 내용
[1. UIApplicationDelegate란?](#1-UIApplicationDelegate란?)</br>
[2. UISceneDelegate란?](#2-UISceneDelegate란?)</br>
[3. AppDelegate와 SceneDelegate가 나뉜 이유](#3-AppDelegate와-SceneDelegate가-나뉜-이유)</br>
[4. App의 Lifecycle](#4-App의-Lifecycle)</br>
[5. 참고 자료](#5-참고-자료)</br>

## 🎯 학습 목표
|상태|목표|
|---|---|
||UIApplicationDelegate에 대해 설명할 수 있다.|
||USceneDelegate에 대해 설명할 수 있다.|

</br>

### 1. UIApplicationDelegate란?
UIApplicationDelegate는 우리가 프로젝트 파일을 만들었을 때 디폴트로 생성되는 __AppDelegate.swift__ 에서 사용되며 AppDelegate는 프로토콜 UIApplicationDelegate를 만족시킵니다.</br>
UIApplicationDelegate 는 앱 전반에서 사용되는 shared 객체들을 관리하기 위한 함수를 제공하며 주로 아래와 같은 일을 합니다.</br>
- 앱의 중앙 데이터 구조를 초기화
- 앱의 화면(Scene) 구성
- 앱 외부로부터의 알림(배터리 부족, 다운로드 완료 등)에 대한 응답
- APNs와 같이 앱이 처음 시작되는 시점에서 등록해야하는 기능 등록

- (iOS 12 버전 이전) 앱의 주요 생명주기를 관리
  - 앱이 백그라운드 또는 포그라운드 상태로 변화할 때 함수를 사용해서 상태를 변화
</br>

### 2. UISceneDelegate란?
iOS 13 버전 이후 부터 프로젝트 파일을 생성한 경우 __AppDelegate.swift__ 외에도 __SceneDelegate.swift__ 라는 파일이 생성되었습니다.</br>

```SceneDelegate```에서는 AppDelegate와 비슷하게 LifeCycle을 관리하지만 AppDelegate와 다른점이 있다면 
__Scene__ 에 표시되는 내용을 처리하고 앱이 표시되는 방식을 관리합니다.</br>

```SceneDelegate```에서는 Scene의 상태에 대해 반응하는 메소드 들이 정의 되어 있고 이를 통해 Scene의 상태에 따른 여러 적절한 동작들을 수행해 줄 수 있습니다.</br>

### 3. AppDelegate와 SceneDelegate가 나뉜 이유
큰 디스플레이, 즉 아이패드가 발달하면서 화면분할 이라는 기능을 지원하게 되었습니다.</br>
그에 따라 하나의 앱이 두 개의 화면으로 떠 있거나 서로 다른 앱을 동시에 화면에 띄우는 것이 가능해졌죠.</br>

따라서 우리는 앱 전체가 아닌 Scene에 따라 생명주기를 관리해야 하는 필요성이 생겼고, 따라서 iOS13 부터는 Lifecycle에 관련된 함수들이 ```AppDelegate```에서 ```SceneDelegate```로 이동하였습니다.</br>
(물논 iOS 13 이상에서도 설정 후 ```AppDelegate```로 Lifecycle 관리 가능)</br>

### 4. App의 Lifecycle
- __Unattached:__
- __Forground Inactive:__
- __Forground Active:__
- __Background:__
- __Suspended:__

### 5. 참고 자료
[Apple Docs - UIApplicationDelegate](https://developer.apple.com/documentation/uikit/uiapplicationdelegate)</br>
[Apple Docs - UISceneDelegate](https://developer.apple.com/documentation/uikit/uiscenedelegate)</br>
[Apple Article - Managing your app's life cycle](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle)</br>
[WWDC - Architecting Your App for Multiple Windows](https://developer.apple.com/videos/play/wwdc2019/258/)</br>
