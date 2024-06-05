# 📝 TIL - 240530 @State, @Binding
## 학습 내용
[1. Single Source of Truth](#-1-Single-Source-of-Truth)</br>
[2. @State](#-2-@State)</br>
[3. @Binding](#-3-@Binding)</br>
[4. 참고 자료](#4-참고-자료)</br>

</br>

SwiftUI에는 데이터 흐름을 관리하기 위해 @State, @Binding, @Published, @ObservableObject 등 다양한 종류의 Property Wrapper가 존재합니다.</br>
이 중 어떤 것을 사용할지는 데이터가 어디서 시작하는지, 다른 뷰 계층들과 어떠한 관계를 가지고 있는지 등을 고려해야 합니다.</br>

오늘은 이 중 대표적인 @State와 @Binding에 대해 알아보겠습니다.</br>

### ❇️ 1. Single Source of Truth
Property Wrapper에 대해 알아보기 전 데이터 흐름에 있어 중요한 개념인 Single Source of Truth에 대해 알아보겠습니다.</br>
UI 프로그래밍에 있어 데이터의 무결성은 무엇보다 중요하며 보장되어야 하는 첫 번째 사항입니다.</br>
의도치 않은 동시적 상황 등으로 인해 변경된 데이터는 버그를 초래하며 시스템이 의도한대로 작동하는 것을 방해합니다.</br>

따라서 데이터의 Single Source of Truth를 보장하고자 우리는 데이터가 하나의 출처에서 관리되는 것을 보장해야 합니다.</br>

### ❇️ 2. @State
@State는 값을 해당 뷰가 소유하고 있는 경우 사용되며 @State로 표시된 값은 모두 SOT(Source of Truth)를 만족시킵니다.</br>
여기서 또 고려해야 할점은 View는 Event의 Squence가 아닌 State의 Function 이라는 사실이죠.</br>

@State는 App, Scene 또는 View에서 정의될 수 있으며 초기 값을 정의해주어야 합니다.</br>
@State로 표시한 값은 해당 값을 포함한 뷰 내부에서만 사용이 보장되므로 주로 private를 붙여 사용합니다.</br>

```swift
struct PlayButton: View {
	@State private var isPlaying: Bool = false // Wrapped Value
	
	var body: some View {
		Button(isPlaying ? "Pause" : "Play") {
			isPlaying.toggle()
		}
	}
}
```

### ❇️ 3. @Binding
@Binding은 값에 대해 소유권을 가지지 않고 읽고 수정이 가능하게 해줍니다.</br>
간단히 말하면 SuperView가 소유하고 있는 @State 값을 SubView에서 수정이 가능하게 해준다는 것이죠.</br>

이때 @Binding은 @State로 부터 유래하기 때문에 초기 값을 가질 필요가 없습니다.</br>
그리고 @State 값을 소유하고 있는 뷰에서 @Binding 값을 사용하기 위해선 Dollar syntax($)를 통해 사용할 수 있습니다.</br>
```swift
struct PlayButton: View { //Subview
    @Binding var isPlaying: Bool // Set the value as Binding

    var body: some View {
        Button(isPlaying ? "Pause" : "Play") {
            isPlaying.toggle()
        }
    }
}

//---------------------

struct PlayerView: View { //Superview
    @State private var isPlaying: Bool = false // Create the state value

    var body: some View {
        VStack {
            PlayButton(isPlaying: $isPlaying) // The value passed to the subview when ever it changed.
            // ...
        }
    }
}
```

만약 @Binding 자리에 @State가 자리하게 되면 어떤 일이 일어날까요?</br>
위의 코드에서 @Binding 자리에 @State가 오게 된다면 우리는 또 다른 Source of Truth를 생성하는 꼴이 됩니다.</br>
따라서 SuperView의 isPlaying과 Subview의 isPlaying은 서로 sync가 맞지 않게 되겠죠.</br>

오늘은 두 개밖에 살펴보지 않았지만 이런 Property Wrapper를 사용한 선언형UI, SwiftUI를 사용함으로써 우리가 얻을 수 있는 이점은 명확합니다.</br>
UIKit의 경우 액션을 통한 특정 값의 변경으로 인해 뷰가 업데이트 되는 전 과정은 ViewController라는 부분에서 처리되었습니다.</br>
이러다보니 ViewController는 비대해지기 일쑤였고 이는 그닥 현명한 방법이 아니였죠.</br>

하지만 선언형UI인 SwiftUI는 이런 복잡한 ViewController가 없이도 이 전과 동일한 작업을 가능하게 하였습니다.</br>

### 4. 참고자료
[Apple Docs - State](https://developer.apple.com/documentation/swiftui/state)</br>
[Apple Docs - Binding](https://developer.apple.com/documentation/swiftui/binding)</br>
[WWDC - Data Flow Through SwiftUI](https://developer.apple.com/videos/play/wwdc2019/226/)</br>
[WWDC - Data Essentials in SwiftUI](https://developer.apple.com/videos/play/wwdc2020/10040/)</br>
[HohyeonMoon - SwiftUI의 데이터 흐름](https://www.hohyeonmoon.com/blog/swiftui-data-flow)</br>
