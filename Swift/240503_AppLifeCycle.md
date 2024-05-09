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
|✅|UIApplicationDelegate에 대해 설명할 수 있다.|
|✅|UISceneDelegate에 대해 설명할 수 있다.|
|✅|App의 Lifecycle의 각 단계를 설명할 수 있다.|
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

#### Scene이란?
- Scene에는 UI의 인스턴스를 나타내는 window와 view controller가 들어있다.
- 각 scene에 해당하는 UIWindowSceneDelegate를 갖고 있으며 이 객체는 UIKit과 앱 간의 상호작용을 조정하는데 사용된다.
- Scene들은 같은 메모리와 앱 프로세스 공간을 공유하면서 서로 동시에 실행되기 때문에 하나의 앱은 여러 Scene과 Scene Delegate 객체를 동시에 활성화 할 수 있다.

### 3. AppDelegate와 SceneDelegate가 나뉜 이유
<img src = "https://github.com/Diana-yjh/TIL/blob/main/Resources/AppLifeCycle/AppLifeCycle_Scenes.jpg" width = "550" ></br>
큰 디스플레이, 즉 아이패드가 발달하면서 화면분할 이라는 기능을 지원하게 되었습니다.</br>
그에 따라 하나의 앱이 두 개의 화면으로 떠 있거나 서로 다른 앱을 동시에 화면에 띄우는 것이 가능해졌죠.</br>

따라서 우리는 앱 전체가 아닌 Scene에 따라 생명주기를 관리해야 하는 필요성이 생겼고, 따라서 iOS13 부터는 Lifecycle에 관련된 함수들이 ```AppDelegate```에서 ```SceneDelegate```로 이동하였습니다.</br>
(물논 iOS 13 이상에서도 설정 후 ```AppDelegate```로 Lifecycle 관리 가능)</br>

### 4. App의 Lifecycle
<img src = "https://github.com/Diana-yjh/TIL/blob/main/Resources/AppLifeCycle/AppLifeCycle_AppLifeCycle.png" width= "550"></br>

앱에서는 각각의 Scene 상태에 따라 아래와 같은 작업을 수행합니다.</br>
- UIKit이 scene을 앱에 처음 붙인 경우, scene의 초기 UI를 확인하고 scene이 필요로 하는 데이터를 로드합니다.
- Forground-active 상태로 변경되었을 때 UI를 확인하고 유저와의 상호작용을 준비합니다.
- Forground-active 상태에서 벗어나면 데이터를 저장하고 앱의 활동을 정지합니다.
- Background 상태에 진입하면 중요한 작업들을 종료하고 상당수의 메모리를 해제합니다.
- Scene의 연결이 끊긴 경우 scene과 관련된 공유자원을 정리한다.
</br>

#### 앱의 상태
- __Not Running/Unattached:__ 앱이 아직 실행되지 않는 제일 첫 번째의 상태.
- __Forground Inactive/ResignActive:__ 앱이 실행 중이지만 사용자로부터 이벤트를 받을 수 없는 상태. 앱 실행중 전화, 알림 등 일시적인 interruption이 발생하는 경우에도 이 상태로 진입한다.
- __Forground Active:__ 앱이 실제 실행 중이고 사용자 이벤트를 받아 상호작용할 수 있는 상태.
- __Background:__ 홈 화면으로 나가거나 다른 앱으로 전환되어 현재 앱에서는 실질적인 동작을 하지 않는 상태. Forground Active에서 Inactive를 거친 다음에 이 상태로 진입.
- __Suspended:__ Background 상태에서 백그라운드 작업 등 아무런 작업도 하지 않고 있다면 앱은 Suspend 상태로 변경되고 이는 iOS에 의해 메모리가 부족한 경우 순차적으로 정리된다.

### 5. 참고 자료
[Apple Docs - UIApplicationDelegate](https://developer.apple.com/documentation/uikit/uiapplicationdelegate)</br>
[Apple Docs - UISceneDelegate](https://developer.apple.com/documentation/uikit/uiscenedelegate)</br>
[Apple Article - Managing your app's life cycle](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle)</br>
[WWDC - Architecting Your App for Multiple Windows](https://developer.apple.com/videos/play/wwdc2019/258/)</br>
[Charlie님의 블로그](https://velog.io/@tmdckd232/Swift-App-Delegate-Scene-Delegate)
