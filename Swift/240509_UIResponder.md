# 📝 TIL - 240509 UIResponder
## 학습 내용
[1. UIResponder 란?](#1-UIResponder-란)</br>
[2. UIResponder의 사용](#2-UIResponder의-사용)</br>
[3. 이벤트 FirstResponder 결정](#3-이벤트-FirstResponder-결정)</br>
[4. 참고 자료](#3-참고-자료)</br>

## 🎯 학습 목표
|상태|목표|
|---|---|
|✅|UIResponder에 대해 설명할 수 있다.|
|✅|UIResponder 처리를 할 수 있다.|
</br>

### 1. UIResponder 란?
UIResponder은 이벤트 핸들링에 응답하는 추상적인 인터페이스 입니다.</br>
UIResponder의 인스턴스인 Responder 객체는 UIKit 앱에서 이벤트 처리의 핵심(backbone)을 구성합니다.</br>
이벤트가 발생하는 경우 UIKit은 이를 처리하기 위해 앱의 responder objects로 이벤트를 할당합니다.</br>

이벤트에는 터치 이벤트, 모션 이벤트, 리모트 컨트롤 이벤트 그리고 press 이벤트 등 다양한 이벤트들이 포함됩니다.</br>
이러한 이벤트들을 처리하기 위해서는 Responder은 반드시 관련 메소드를 오버라이드 해야 합니다.</br>
예를 들어 터치 이벤트를 처리한다고 할 때는 `touchesBegan(_:with:)`, `touchesMoved(_:with:)`, `touchesEnded(_:with:)` 그리고 `touchesCancelled(_:with:)` 메소드를 오버라이드 해야합니다.</br>

이벤트를 처리하기 위해서 이미 존재하는 메소드들을 활용하는 방법도 있지만, UIResponder은 처리되지 못한 Event를 처리할 수 있는 앱의 요소에 전달하는 역할도 하고 있습니다.</br>
만약 주어진 Responder가 이벤트를 처리하지 않는 경우 Reponder은 ReponderChain의 다음 요소에 해당 Event를 전달합니다.</br>
UIKit은 사전에 정의되어있는 법칙에 따라 이벤트를 처리할 다음 객체를 정합니다.</br>
예를 들어서 특정 뷰가 이벤트를 처리하지 못하는 경우 이 이벤트의 처리에 대한 책임은 해당 뷰의 SuperView로 이전됩니다.</br>
여기서 해당 SuperView도 이벤트를 처리하지 못한다면 SuperView의 SuperView로, 그리고 그 또한 처리하지 못한다면 해당 View의 ViewController로 이벤트 처리 책임은 넘어가게 됩니다.</br>
이렇게 이벤트 처리 책임이 체인처럼 이전된다 하여 우리는 이를 __UIResponder Chain__ 이라고 부릅니다.

<img src = "https://github.com/Diana-yjh/TIL/blob/main/Resources/UIResponder/UIResponder_UIResponderChain.png" width = "550" /></br>

UIResponder 클래스의 인스턴스로는 우리가 일반적으로 사용하는 UIView, UIViewController 그리고 UIApplication 등이 있습니다.</br>
UIKit의 경우 이벤트가 들어오는 경우 제일 적절한 대상에게 해당 이벤트를 넘기고 이 대상을 __FirstResponder__ 라고 부릅니다.</br>

처리되지 않은 이벤트는 활성화되어있는 ResponderChain의 Responder 들에게 넘어가게 되고 동적으로 어떤 Responder가 이벤트를 처리할 지 결정하게 됩니다.</br>

### 2. 이벤트 FirstResponder 결정
|이벤트 타입|FirstResponder|
|---|---|
|Touch 이벤트|Touch 이벤트가 발생한 뷰|
|Press 이벤트|Focus 되어있는 객체|
|Shake-motion 이벤트|너(또는 UIKit)이 지정한 객체|
|Remote-control 이벤트|너(또는 UIKit)이 지정한 객체|
|Editing menu 이벤트|너(또는 UIKit)이 지정한 객체|

### 3. UIResponder의 사용
UIReponder에 대해 알아본 결과 뭔가 Event가 들어오면 앱 내부의 UIReponder 인스턴스들이 누가 이벤트를 처리할 것인지 알아서 잘(?) 해주는 듯 한데 실제로 개발할 때 이 부분을 고려해야하는 경우가 있을까요?</br>
물논 대답은 예상한대로 __있습니다__ 입니다.</br>

<img src = "https://github.com/Diana-yjh/TIL/blob/main/Resources/UIResponder/UIResponder_Signup.png" width = "300"/></br>

간단한 예시 중 하나는 회원가입 화면처럼 여러개의 Textfield가 존재하는 화면에서 를 들 수 있는데요.</br>
우리가 여러 앱을 사용했던 경험에 의하면 Textfield에 글씨를 채워넣고 키보드의 `완료`을 누르면 알아서 다음 Textfield로 커서가 넘어가는 것을 경험해봤을 것입니다.</br>
이전의 우리는 이러한 작업이 자동으로 이루어졌다 라고 생각할 수 도 있겠지만 이 작업 또한 UIResponder를 사용한 자연스러운 UI 구현입니다.</br>

우선 우리가 첫 번째 Textfield인 `이메일`을 선택하였을 때 키보드가 나오죠.</br>
이 때 우리 앱에서 이벤트를 처리해주는 FirstResponder은 `이메일` Textfield가 됩니다.</br>
이벤트를 제일 우선적으로 처리해줄 객체라는 것이죠.</br>
firstResponder로 설정된 `이메일` Textfield는 이벤트 처리를 위한 다른 FirstResponder가 설정되지 않는 이상 계속 키보드를 물고 있습니다.</br>
빈 화면을 눌러도 키보드는 내려가지 않죠 ^^(~~아니 이거도 처리해줘야하냐고~~)</br>

이 때 완료를 누른 이후 다음 Textfield인 `이메일 재입력` field에 FirstResponder를 넘겨주면 커서는 우리가 원하는대로 다음 `이메일 재입력` Textfield로 넘어가게 됩니다.</br>

```swift
func textFieldShouldReturn(_ textField: UITextField) -> Bool {
    if emailTextField.text != "" {
        reEnterEmailTextField.becomeFirstResponder()
        return true
    }
    return false
}
```

이렇게 오늘은 UIResponder에 대해 알아봤습니다.</br>

### 4. 참고 자료
[Swift Docs - UIResponder](https://developer.apple.com/documentation/uikit/uiresponder)</br>
[Swift Docs - Using responders and the responder chain to handle events](https://developer.apple.com/documentation/uikit/touches_presses_and_gestures/using_responders_and_the_responder_chain_to_handle_events) </br>
