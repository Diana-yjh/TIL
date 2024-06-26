# 📝 TIL - 240519_CoreData

## 학습 내용
[1. CoreData란?](#1-CoreData란?)</br>
[2. 참고 자료](#2-참고-자료)</br>

</br>

### 1. CoreData란?
CoreData는 하나의 디바이스에 삭제되지 않았으면 하는 데이터 또는 캐시 데이터를 저장하거나 여러 디바이스에 CloudKit을 사용하여 동시다발적으로 데이터를 저장할 때 사용되는 데이터 모델 에디터입니다.</br>
CoreData는 데이터베이스를 직접 관리하지 않고도 데이터를 추상화하여 저장할 수 있도록 도와줍니다.</br>
(CoreData는 DB가 아닙니다!)

CoreData가 지원하는 내용은 아래와 같습니다.</br>

#### ✓ 1-1. 개별 또는 일괄 변경을 취소하거나 재실행
CoreData는 특정 동작이 개별 도는 일괄 변경될 때 이것을 취소하거나 재실행할 수 있게 도와줍니다.</br>

<img src = "https://github.com/Diana-yjh/TIL/blob/main/Resources/CoreData/CoreData.png" width = "500"/></br>

#### ✓ 1-2. 데이터의 백그라운드 처리
Json 파싱을 객체로 변경하는 등의 백그라운드에서의 데이터 처리에 대한 UI를 막아줍니다.</br>
(즉 UI가 업데이트 되기 전, Json파싱 등으로 객체 생성을 대기한 뒤 캐시하거나 저장하여 서버에 갔다오는 횟수를 줄인다는 의미인 듯 합니다.)</br>

#### ✓ 1-3. 동기화
테이블 또는 콜렉션 뷰에 데이터소스를 제공하여 뷰와 데이터간의 동기화를 도와줍니다.</br>
(sync가 맞게 도와준다는 의미인것 같네요)

#### ✓ 1-4. 버전 관리 및 마이그레이션
CoreData는 앱이 개선됨에 따라 데이터모델의 버전을 관리하거나 마이그레이션 하는 것을 도와줍니다.</br>

### 2. 참고 자료
[Apple Docs - Core Data](https://developer.apple.com/documentation/coredata)</br>
[Pingu - CoreData](https://icksw.tistory.com/224)</br>
