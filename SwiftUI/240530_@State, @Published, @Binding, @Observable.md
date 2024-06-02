# 📝 TIL - 240530 @State, @Published, @Binding, @Observable
## 학습 내용
[1. @State](#-1-@State)</br>
[2. @Binding](#-2-@Binding)</br>
[3. @Observable](#-3-@Observable)</br>
[4. @Published](#-4-@Published)</br>
[5. 참고 자료](#5-참고-자료)</br>

</br>

### ❇️ 1. @State
@State는 SwiftUI를 통해 관리되는 값을 읽고 쓸 수 있는 Property Wrapper 타입입니다.</br>
@State는 뷰 계층에 저장 되어 있는 주어진 값 타입을 SSOT(Single source of truth)로써 사용할 수 있게 해줍니다.</br>
(SSOT: Single source of truth (SSOT) is a concept used to ensure that everyone in an organization bases business decisions on the same data.)</br>

@State는 App, Scene 또는 View에서 정의될 수 있으며 초기 값을 정의해주어야 합니다.</br>
@State로 표시한 값을 멤버와이즈 이니셜라이저에 넣지 않기 위해서는(SwiftUI에서 제공해주는 스토리지 관리와 충돌 가능성이 있음) 해당 값을 private로 선언해주어야 합니다.</br>

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

### ❇️ 2. @Binding
@Binding은 Superview에서 명시된 @State 값에 변경이 일어났을 때 해당 변경을 Subview에 전달해주기 위해 사용합니다.</br>

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

우선 Superview의 변경될 값에 @State를 표시한 뒤 해당 변경을 Subview에 전달해주기 위해 "$"를 사용하여 해당 값을 넘겨줍니다.</br>

### ❇️ 3. @Observable

### ❇️ 4. @Published

