
># ☀️ 모닝글로리 
- 아침형 인간이 되고픈, 부지런한 삶을 살아가고 싶은 사람들을 위한 미라클 모닝을 위한 앱



<br> <br> 
<img width="520" alt="screenshot" src="https://github.com/user-attachments/assets/15441584-f717-415b-b031-1fda5eaaf23c" />
<br> <br> 
# 🙋‍♀️ 프로젝트 주요 기능 
- 닉네임, 프로필 사진 커스텀
- 유저에게 실시간 날씨와 명언 표시
- 미션 입력, 기상시간 체크
- 미션 완료 시, 달력에 확인표시 기능
- 차트를 통해 진행률 표시
- 후기 글, 사진 포스트/삭제

<br> <br> 

# 🧑🏻‍💻 프로젝트 개발환경
- 1인 개발(iOS)
- 개발기간: 2024.09.12 ~ 2024.09.27(약 2주, 버전 1.0 기준)
- iOS 최소 버전: iOS 16.0+   


<br> <br> 

   
# 🛠 프로젝트 기술스택
    

- 아키텍처 및 디자인 패턴: MVVM, Combine, Repository/Input-Output Pattern

- 네트워킹 및 데이터 처리: Alamofire, Realm
- UI 및 이미지 처리: SwiftUI, WidgetKit



<br> <br> 

# 👉  상세 기능 구현 설명

### - Core Data 데이터 관리 및 UI 업데이트 설계

- 객체 간의 결합도를 낮추고 유지보수의 용이성을 위해 DIP(Dependency Inversion Principle)를 적용하여, 여러 ViewModel이 구체적인 구현(CoreDataRepository)에 의존하지 않고, 추상화된 인터페이스(DatabaseRepository)에 의존하도록 설계     
- 이를 통해 데이터 저장 및 관리 로직과 UI 로직을 분리하여 테스트 및 유지보수를 용이하게 구현

<br>

### - 데이터 상태 변화 관리
  1. CurrentValueSubject를 활용한 상태 관리  
- 앱에서 자주 변경되는 상태를 CurrentValueSubject에 저장하여 최신 상태를 유지
- 구독자가 새로 구독할 때 현재 상태를 즉시 전달하고, 변경 사항이 발생하면 자동으로 업데이트되도록 설정  

2. PassthroughSubject를 활용한 이벤트 처리  
- 버튼 클릭, 알림, 트리거 등 일회성 이벤트는 PassthroughSubject를 사용해 전달 
- 이벤트가 발생할 때마다 새로운 값이 전달되어 필요한 비즈니스 로직을 처리하고, UI에 반영하도록 구현
3. 데이터 바인딩을 통한 UI 업데이트   
- sink와 assign을 사용하여 ViewModel의 상태가 변경될 때마다 UI가 자동으로 업데이트되도록 데이터 바인딩을 구현 
- 이를 통해 코드의 가독성을 높이고, 데이터 상태 변화가 UI에 즉각 반영되도록 구현
4. 실시간 데이터 처리 및 메모리 관리  
- Combine의 Cancellable을 활용해 메모리 누수를 방지, deinit 시 모든 구독을 해제하여 안정적인 메모리 관리를 구현했

<br>

### - 이미지 파일 관리

- 게시글 이미지: 화질이 중요한 게시글 이미지는 pngData로 압축하여 저장해, 이미지 용량을 줄이면서도 고해상도를 유지
- 썸네일 이미지: 썸네일 이미지는 dataType으로 전환 시 pngData가 31KB, jpegData가 22KB로 용량 차이가 크지 않아, 화질이 더 선명한 pngData 형식으로 압축하여 저장


<br>

### - 사용자 위치 관리
- 위치 정보 처리: CoreLocation을 사용하여 사용자 위치를 기반으로 날씨 데이터를 제공하고, 정확한 위치 서비스를 구현
- 위치 업데이트: CLLocationManager를 사용하여 실시간으로 사용자의 위치를 지속적으로 업데이트하고 관리하고, 사용자의 위치가 변동될 때마다 자동으로 반영되도록 설정
- 권한 요청 및 상태 관리: 위치 접근 권한을 효과적으로 관리하고, 사용자가 위치 권한을 변경했을 때도 적절한 대응을 할 수 있도록 권한 상태를 지속적으로 모니터링

<br>

### - 컴파일 최적화
 - private을 통해 외부 접근을 제한하여 캡슐화를 강화하고, final를 통해 클래스와 메서드가 상속되지 않게 선언해 메서드 호출을 인라인으로 대체하여 코드 최적화 수행

<br>

### - 위젯을 통한 정보 전달

디자인 및 사용자 맞춤
-  앱 내의 통계를 위한 그래프를 위젯에서 동일하게 구현

실시간 데이터 동기
- WidgetKit과 App Groups를 활용해 앱과 위젯 간 데이터를 효율적으로 동기화하는 구조를 설계
- UserDefaults(suiteName:)와 WidgetCenter.reloadTimelines()를 통해 실시간으로 데이터를 공유하고 위젯이 항상 최신 상태를 반영하도록 구현
- Timeline 정책을 기반으로 위젯 데이터를 정기적으로 업데이트하며, Small, Medium, Large 크기를 지원하는 사용자 맞춤형 디자인을 제공하여 사용자 경험 개선


<br> <br> 
# 👿 트러블슈팅 


### 문제상황 - 앱과 위젯에서의 정보 동기화 문제

- 앱에서 UserDefaults에 저장한 정보가 위젯에서 로드되지 않는 문제

 - 앱과 위젯 간 데이터가 별도의 저장소에서 관리되며, 데이터 공유를 위한 App Groups를 설정해야 데이터 공유 가능

### 해결 - 앱과 위젯 간 데이터를 공유하기 위해 App Groups를 활성화

```
let sharedDefaults = UserDefaults(suiteName: "group.com.myapp.shared")
```
- App Group UserdDefaults로 데이터 저장하여 앱과 위젯에서 동일한 데이터에 접근하도록 구현

<br>
<br>
<br>




### 문제상황 - 뷰의 구성이 복잡해졌을 때 데이터 공유
#### 구현 사항
-    ToDoView에는 두 개의 하위 뷰인 MissionCards와 AddMissionView가 존재하며, 두 뷰 모두 동일한 뷰모델 ToDoVM의 정보를 사용
- 두 하위 뷰가 같은 방식으로 ToDoVM의 정보를 사용할 경우, 데이터 변경 시 곧 바로 업데이트가 되지 않는 문제 발생


#### 원인
- 각 뷰의 역할 판단
- MissionCards:   
  • 상위뷰의 정보만 받아와서 뷰 상태 업데이트, 뷰모델과 상위 뷰의 상태에 영향을 주지 않음
- AddMissionView:   
  • 단순히 상위 뷰의 데이터를 받는 것이 아니라, 해당 뷰에서 발생한 데이터 변경 사항을 상위 뷰와 뷰모델이 인지하여 즉각적으로 업데이트

#### 해결 방법
- MissionCards:    
• 상위뷰에서 데이터를 @ObservedObject로 받아, 개별 데이터의 변경 사항만 감지하여 독립적으로 뷰 업데이트
- AddMissionView:   
• @EnvironmentObject를 사용하여 ToDoVM 전체에 접근할 수 있도록 구현   
• 데이터를 추가하거나 수정할 경우, 뷰모델이 이를 즉시 반영하며 상위 뷰에서도 곧바로 업데이트가 이루어지도록 설계
