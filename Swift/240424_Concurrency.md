# 📝 TIL - 240424 Concurrency
## 학습 내용
[1. 병렬 프로그래밍 vs 동시성 프로그래밍](#1-병렬-프로그래밍-vs-동시성-프로그래밍)</br>
[2. async vs sync](#2-async-vs-sync)</br>
[3. GCD(Grand Central Dispatch)](#3-GDCGrand-Central-Dispatch)</br>
[4. main, global()](#4-main-global)</br>
[5. Completion Handler](#5-Completion-Handler)</br>
[6. Async/Await](#6-AsyncAwiat)</br>
[7. 참고자료](#7-참고자료)</br>
## 🎯 학습 목표
|상태|목표|
|---|---|
|✅|병렬 프로그래밍과 동시성 프로그래밍을 설명할 수 있다.|
|✅|async와 sync 차이를 구분할 수 있다.|
|✅|async와 concurrent의 차이를 구분할 수 있다.|
|✅|main 스레드의 특징과 main.sync가 불가능한 이유를 설명할 수 있다.|
|✅|DispatchQueue에서 main과 global()의 차이를 설명할 수 있다.|
|✅|Async/Await의 원리를 설명할 수 있다.|

</br>

### 1. 병렬 프로그래밍 vs 동시성 프로그래밍
- __병렬 프로그래밍:__ CPU가 여러 개 있을 때 가능하며 실제로 동시에 작업을 처리하고잇는 것.
- __동시성 프로그래밍:__ CPU 하나를 기준으로 하며 여러 일을 동시에 처리하는 것으로 보이지만 실제로는 아주 빠르게 Context Switching을 하고 있는 것.</br>
<img src = "https://github.com/Diana-yjh/TIL/blob/main/Resources/Concurrency/%EC%8B%B1%EA%B8%80%EC%BD%94%EC%96%B4%EB%A9%80%ED%8B%B0%EC%BD%94%EC%96%B4.png" width = "300" />
</br>

### 2. async vs sync
- __async(비동기):__ 특정 작업이 끝날 때까지 대기하지 않고 다음 작업을 실행.
- __sync(동기):__ 특정 작업이 끝날 때까지 다른 작업을 하지 않고 대기하는 것.
</br>

그럼 이때 동시성(Concurrency)와 비동기(async)는 동일한 것일까?</br>
정답은 __그렇지 않다__ 이다.</br>
동시성의 경우 여러 작업을 처리하는 방법을 말하고 비동기는 하나의 작업을 처리하는 방법을 이야기한다.
</br>

### 3. GCD(Grand Central Dispatch)
동시성 프로그래밍을 구현하기 위해서는 우리는 여러 개의 Tread 들에 Task를 적당히 배분시켜줘야 한다.</br>
이건 꽤나 골치아픈 작업일 수 있는데 Swift에서는 Task 들을 `DispatchQueue`에 넘겨주면 다 알아서 해준다는 말씀!</br>
DispatchQueue에 넘어간 Task 들은 Queue의 특성에 맞게 FIFO로 빠져나오게 된다.</br>

```swift
DispatchQueue.global().async {
        //Tasks
}
```

### 4. main, global()
DispatchQueue를 작성하다 보면 뒤에 main과 global() 등이 붙는 것을 확인할 수 있다.</br>
main과 global()은 뭘까?</br>
앞서 살펴보았듯이 우리가 DispatchQueue에 Task를 할당하면 DispatchQueue는 해당 Task들을 적절한 스레드에 할당시켜 진행시킨다.</br>
즉, main과 global()은 스레드의 종류를 나타내며 아래와 같은 특징을 가지고 있다.</br>

- __main__ </br>
  Serial 스레드이며 전역적으로 사용되며 RunLoop(Main Run Loop)가 자동으로 설정되어 실행된다.</br>
  __UI 작업은 반드시 메인 스레드에서 처리되어야 한다.__
- __global__ </br>
  Concorrent 스레드이며 Main 스레드에서 파생되어진 Thread이다.</br>
  작업 후 메모리에서 제거되는 특징을 가지고 있다.
</br>

Swift에서 Concurrency Programming을 구현하는 방법에는 ```Completion Handler```를 사용하는 방법과 ```Async/Await```를 사용하는 방법이 있다.</br>

### 5. Completion Handler
Completion Handler를 사용하는 방법은 전통적으로 많이 사용되던 방법이지만 아래와 같은 단점들을 가지고 있다.</br>
- __과도한 중첩코드 발생(장풍코드(?))__
  Completion Handler의 호출이 반복되는 경우 Code는 장풍을 맞은거마냥 가독성이 좋지 않게 된다.</br>
  이 경우 Error Handling까지 더해지면 더더욱 지저분한 코드가 된다.</br>
  <img src = "https://github.com/Diana-yjh/TIL/blob/main/Resources/Concurrency/CompletionHandler_%EC%A4%91%EC%B2%A9%EC%BD%94%EB%93%9C.png" width = "800"/>

- __오류발생__
  Completion Handler를 사용하는 경우 작업 종료 후 completion handler를 호출해주어야 한다.</br>
  호출해 주지 않는 경우 프로그램은 어떠한 에러 표시 없이 중단되는데 호출하지 않아도 컴파일러는 잡아내지 못하므로 에러를 발생시키기 쉽다.</br>
  <img src = "https://github.com/Diana-yjh/TIL/blob/main/Resources/Concurrency/CompletionHandler_%ED%98%B8%EC%B6%9C%EC%83%9D%EB%9E%B5.png" width = "800"/>

- __에러처리가 쉽지 않음__
  Error 처리를 위해 코드를 추가하는 순간 코드의 양이 기하급수적으로 늘고 가독성을 해친다.</br>
  <img src = "https://github.com/Diana-yjh/TIL/blob/main/Resources/Concurrency/CompletionHandler_%EC%98%A4%EB%A5%98%EC%B2%98%EB%A6%AC.png" width = "800" />
  
</br>

### 6. Async/Await

### 참고자료
- [야곰닷넷(Concurrency)](https://yagom.net/courses/swift-concurrency-programming/)
- [WWDC-Meet async/await in Swift](https://developer.apple.com/videos/play/wwdc2021/10132/)
- [Najin님의 차근차근 시작하는 GCD](https://sujinnaljin.medium.com/ios-%EC%B0%A8%EA%B7%BC%EC%B0%A8%EA%B7%BC-%EC%8B%9C%EC%9E%91%ED%95%98%EB%8A%94-gcd-grand-dispatch-queue-1-397db16d0305)
- [Apple 공식문서 - Concurrency](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/concurrency/)
- [야곰 Notion - Swift Concurrency]()
