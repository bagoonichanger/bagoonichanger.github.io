---
layout : single
title : "[Swift]Diary"
categories : Swift
tag : [Swift]
toc : true
author_profile : false
sidebar :
    nav : "docs"
---

<img src="https://raw.githubusercontent.com/bagoonichanger/bagoonichanger.github.io/upload_Image/images/202209271511373.png" alt="Simulator Screen Shot - iPhone 13 - 2022-09-27 at 14.57.25" style="zoom:20%;" /><img src="https://raw.githubusercontent.com/bagoonichanger/bagoonichanger.github.io/upload_Image/images/202209271511449.png" alt="Simulator Screen Shot - iPhone 13 - 2022-09-27 at 14.57.27" style="zoom:20%;" />



## DateFormatter

 Date는 "yyyy-MM-dd HH:mm" (ex: 2022-09-27 09:12:34 +0000) 이러한 형태

```swift
private func dateToString(date: Date) -> String {
    let formatter = DateFormatter()
    formatter.dateFormat = "yy년 MM월 dd일(EEEEE)"
    formatter.locale = Locale(identifier: "ko_KR")
    return formatter.string(from: date)
  }
```



## Prepare

-  segue가 수행되려고 함을 뷰 컨트롤러에 알리는 작업

- 뷰컨트롤러 전환 전에 데이터를 처리할 수 있는 메서드

```swift
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    if let wireDiaryViewContoller = segue.destination as? WriteDiaryViewController {
      wireDiaryViewContoller.delegate = self
    }
  }
```



## UICollectionView

### UICollectionView와 관련된 클래스 및 프로토콜

- #### 최상위 포함 및 관리 (Top level containment)

  **UICollectionView**

  컬렉션뷰의 콘텐츠가 보이는 영역을 정의

  **UICollectionViewController**

  컬렉션뷰를 관리하는 뷰 컨트롤러(선택적으로 사용 가능)

  #### 콘텐츠 관리(protocol)

  **UICollectionViewDataSource**

  컬렉션뷰와 관련된 중요한 객체이며, 필수적으로 제공. 데이터 소스는 컬렉션뷰의 콘텐츠를 관리하고 해당 콘텐츠를 표시하기 위한 뷰를 제공

  ```swift
  /**필수 메서드**/
  
  //지정된 섹션에 표시할 항목의 개수를 묻는 메서드.
  func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int
  
  //컬렉션뷰의 지정된 위치에 표시할 셀을 요청하는 메서드.
  func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell
   
  
  /**주요 선택 메서드**/
  
  //컬렉션뷰의 섹션의 개수를 묻는 메서드. 이 메서드를 구현하지 않으면 섹션 개수 기본 값은 1.
  optional func numberOfSections(in collectionView: UICollectionView) -> Int
  
  //지정된 위치의 항목을 컬렉션뷰의 다른 위치로 이동할 수 있는지를 묻는 메서드.
  optional func collectionView(_ collectionView: UICollectionView, canMoveItemAt indexPath: IndexPath) -> Bool
  
  //지정된 위치의 항목을 다른 위치로 이동하도록 지시하는 메서드.
  optional func collectionView(_ collectionView: UICollectionView, moveItemAt sourceIndexPath: IndexPath, to destinationIndexPath: IndexPath)
  델
  ```

  

  **UICollectionViewDelegate**

  사용자와의 상호작용과 셀 강조 표시 및 선택 등을 관리

  ```swift
  //지정된 셀이 사용자에 의해 선택될 수 있는지 묻는 메서드.
  optional func collectionView(_ collectionView: UICollectionView, shouldSelectItemAt indexPath: IndexPath) -> Bool
  
  //지정된 셀이 선택되었음을 알리는 메서드.
  optional func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath)
  
  //지정된 셀의 선택이 해제될 수 있는지 묻는 메서드. 선택 해제가 가능한 경우 true로 응답하며, 그렇지 않다면 false로 응답.
  optional func collectionView(_ collectionView: UICollectionView, shouldDeselectItemAt indexPath: IndexPath) -> Bool
  
  //지정된 셀의 선택이 해제되었음을 알리는 메서드.
  optional func collectionView(_ collectionView: UICollectionView, didDeselectItemAt indexPath: IndexPath)
  
  //지정된 셀이 강조될 수 있는지 묻는 메서드. 강조해야 하는 경우 true로 응답하며, 그렇지 않다면 false로 응답.
  optional func collectionView(_ collectionView: UICollectionView, shouldHighlightItemAt indexPath: IndexPath) -> Bool
  
  //지정된 셀이 강조되었을 때 알려주는 메서드.
  optional func collectionView(_ collectionView: UICollectionView, didHighlightItemAt indexPath: IndexPath)
  
  //지정된 셀이 강조가 해제될 때 알려주는 메서드.
  optional func collectionView(_ collectionView: UICollectionView, didUnhighlightItemAt indexPath: IndexPath)
  ```

  

  #### 표시(Presentation)

  UICollectionReusableView / UICollectionViewCell

  컬렉션에 표시된 모든 뷰는 UICollectionReusableView 클래스의 인스턴스여야 합니다. 이 클래스는 컬렉션뷰에서 사용 중인 뷰 재사용 메커니즘을 지원합니다. 새로운 뷰를 만드는 대신, **뷰를 재사용**하여 성능을 향상시킵니다. 

  

  #### 레이아웃(Layout)

  **UICollectionViewLayout**

  UICollectionViewLayout의 서브클래스는 레이아웃 객체라고 하며 컬렉션 뷰 내부의 셀 및 재사용 가능한 뷰의 위치, 크기 및 시각적 속성을 정의

  **UICollectionViewLayoutAttributes**

  레이아웃 프로세스 중에 컬렉션뷰에 셀과 재사용가능한 뷰를 표시하는 위치와 방법을 정의.

  **UICollectionViewUpdateItem**

  레이아웃 객체 아이템이 삽입, 삭제, 혹은 컬렉션뷰 내에서 이동할 때마다 레이아웃 객체는 UICollectionViewUpdateItem 클래스의 인스턴스를 받음.

  

  #### 플로우 레이아웃(Flowlayout)(protocol)

  **UICollectionViewFlowLayout / UICollectionViewDelegatFlowLayout**

  그리드 혹은 다른 라인기반(lined-based) 레이아웃을 구현하는 데 사용. 클래스를 그대로 사용하거나 동적으로 커스터마이징할 수 있는 플로우 델리게이트 객체와 함께 사용할 수 있음

### UICollectionViewFlowLayout

#### 플로우 레이아웃 구성 단계

1. 플로우 레이아웃 객체를 작성해 컬렉션뷰의 레이아웃 객체로 지정.
2. 셀의 너비와 높이를 구성(3가지 방법)
3. 필요한 경우 셀의 간격을 조절.
4. 원할 경우 섹션 헤더 혹은 섹션 푸터를 크기를 지정.
5. 레이아웃의 스크롤 방향을 설정

#### 셀의 너비와 높이를 구성(3가지 방법)

1.  UICollectionViewFlowlayout클래스의 itemSize프로퍼티를 통해 설정하는 방법.

   이 방법은 모든 셀들을 해당 프로퍼티에 설정한 사이즈로 변경.

   ```swift
   let flowlayout = UICollectionViewFlowLayout()
   let threePicture: CGFloat = UIScreen.main.bounds.width / 3.0
   flowlayout.itemsize = CGSize(width: threePicture - 30, height: 100.0)
   ```

   

2. UICollectionViewDelegateFlowLayout 프로토콜의 collectionView(_:layout:sizeForItemAt:) 메서드를 통해 설정하는 방법.

   이 방법은 셀마다 크기를 다른 게 사이즈를 설정.

   ```swift
   func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {
       return CGSize(width: (UIScreen.main.bounds.width / 3) - 30, height: 100.0)
     }
   ```

   

3. 셀에 오토 레이아웃을 지정하고 셀 스스로 크기를 결정해서 UICollectionViewlayout객체에 알려주는 방법.

   이 방법은 estimatedItemSize프로퍼티를 사용해 셀의 최소 크기를 미리 설정하는 방법입니다.

   ```swift
   let flowlayout = UICollectionViewFlowLayout()
   let threePicture: CGFloat = UIScreen.main.bounds.width / 3.0
   flowlayout.estimatedItemSize = CGSize(width: threePicture - 30, height: 100.0)
   ```

   

```swift
//지정된 셀의 크기를 반환하는 메서드
optional func collectionView(_ collectionView: UICollectionView, 
                   layout collectionViewLayout: UICollectionViewLayout, 
            sizeForItemAt indexPath: IndexPath) -> CGSize
            
//지정된 섹션의 여백을 반환하는 메서드.
optional func collectionView(_ collectionView: UICollectionView, 
                   layout collectionViewLayout: UICollectionViewLayout, 
                   insetForSectionAt section: Int) -> UIEdgeInsets

//지정된 섹션의 행 사이 간격 최소 간격을 반환하는 메서드. scrollDirection이 horizontal이면 수직이 행이 되고 vertical이면 수평이 행이 된다.
optional func collectionView(_ collectionView: UICollectionView, 
                   layout collectionViewLayout: UICollectionViewLayout,
                   minimumLineSpacingForSectionAt section: Int) -> CGFloat

//지정된 섹션의 셀 사이의 최소간격을 반환하는 메서드.
optional func collectionView(_ collectionView: UICollectionView, 
                   layout collectionViewLayout: UICollectionViewLayout,
                   minimumInteritemSpacingForSectionAt section: Int) -> CGFloat

//지정된 섹션의 헤더뷰의 크기를 반환하는 메서드. 크기를 지정하지 않으면 화면에 보이지 않습니다.
optional func collectionView(_ collectionView: UICollectionView, 
                   layout collectionViewLayout: UICollectionViewLayout,
                   referenceSizeForHeaderInSection section: Int) -> CGSize

//지정된 섹션의 푸터뷰의 크기를 반환하는 메서드. 크기를 지정하지 않으면 화면에 보이지 않습니다.
optional func collectionView(_ collectionView: UICollectionView, 
                   layout collectionViewLayout: UICollectionViewLayout,
                   referenceSizeForFooterInSection section: Int) -> CGSize	
```

## UUID

- **U**niversally **U**nique **ID**entifier의 약어
-  **범용 고유 식별자**
- 형태 : 12345678–1234–1234–1234–1234567890ab

```swift
UUID().uuidString
```



## Touches 제스쳐

- touchesBegan: 화면을 클릭하기 시작할 때 실행
- touchesMoved: 화면에 드래그 했을 때 실행
- touchesEnded: 화면을 클릭 후 뗄 때 실행
- touchesCancelled: 위 3가지 이벤트가 발생도중에 system alert와 같은게 발생한 경우, touchesCanclled 발생

### 키보드 내리는 방법

**first responder를 resign 시켜버리는 메소드**

```SWIFT
  override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
		self.view.endEditing(true)
 }
```



## NotificationCenter

- NotificationCenter 에 등록된 event 가 발생하면 해당 event에 대한 행동을 취함

- 앱 내에서 메세지를 던지면 아무데서나 이 메세지를 받을 수 있게 하는 역할

- 보통 백그라운드 작업의 결과, 비동기 작업의 결과 등 현재 작업의 흐름과 다른 흐름의 작업으로부터 이벤트를 받을 때 사용

<img src="https://raw.githubusercontent.com/bagoonichanger/bagoonichanger.github.io/upload_Image/images/202209282355386.png" alt="다운로드" style="zoom: 50%;" />

### Notification

NotificationCenter 를 통해 정보를 저장하기 위한 구조체

```swift
var name: NSNotification.Name // Notification 식별 태그
var object: Any? // Notification 관련 객체
var userInfo: [AnyHashable: Any]? // Notification 관련 정보를 담은 dictionary
```

### NotificationCenter

- 등록된 observer 에게 동시에 notification 을 전달하는 클래스

- NotificationCenter 는 notification 을 발송하면 NotificationCenter에서 메세지를 전달한 observer를 처리할 때까지 대기
-  흐름이 동기적

### 구현 순서

- Notification observer 등록

  ```swift
      NotificationCenter.default.addObserver(
        self,
        selector: #selector(editDiaryNotification(_:)),
        name: NSNotification.Name("editDiary"),
        object: nil
      )	
  ```

  

- Responding method 구현

  ```swift
    @objc func editDiaryNotification(_ notification: Notification) {
      guard let diary = notification.object as? Diary else { return }
      guard let index = self.diaryList.firstIndex(where: { $0.uuidString == diary.uuidString }) else { return }
      self.diaryList[index] = diary
      self.diaryList = self.diaryList.sorted(by: {
        $0.date.compare($1.date) == .orderedDescending
      })
      self.collectionView.reloadData()
    }
  ```

  

- Notification을 발송할 객체에 post 메소드 구현

```swift
 NotificationCenter.default.post(
        name: NSNotification.Name("editDiary"),
        object: diary,
        userInfo: nil
      )
```



## 배열 조회

**firstIndex(of:)**  -  배열의 앞에서부터 조회해서 첫번째 일치하는 값의 index를 반환

**lastIndex(of:)** -  배열의 뒤에서부터 조회해서 첫번째 일치하는 값의 index를 반환

```SWIFT
var students = ["Ben", "Ivy", "Jordell", "Maxime"]
if let i = students.firstIndex(of: "Maxime") {
    students[i] = "Max"
}
print(students)
// Prints "["Ben", "Ivy", "Jordell", "Max"]"
```



**firstIndex(where:)**  -  배열의 앞에서부터 조회해서 지정된 predicate를 충족하는 첫번째 일치하는 값의 index를 반환

**lastIndex(where:)** -  배열의 뒤에서부터 조회해서 지정된 predicate를 충족하는  첫번째 일치하는 값의 index를 반환

```SWIFT
let students = ["Kofi", "Abena", "Peter", "Kweku", "Akosua"]
if let i = students.firstIndex(where: { $0.hasPrefix("A") }) {
    print("\(students[i]) starts with 'A'!")
}
// Prints "Abena starts with 'A'!"
```

## Compare

- compare(_:) 메서드를 호출한 문자열이 첫 번째 파라미터 보다 작다면 오름차순

```swift

let a: String = "A"
let b: String = "B"

a.compare(b) == ComparisonResult.orderedAscending                   // true
b.compare(a) == .orderedDescending                                  // false

let largeBeepeach: String = "Beepeach"
let smallBeepeach: String = "beepeach"

largeBeepeach.compare(smallBeepeach, options: [.caseInsensitive])      
// NSComparisionResult
largeBeepeach.compare(smallBeepeach, options: [.caseInsensitive]) == .orderedSame // true

```



## Equatable

- Protocol
- 값이 동일한 지 어떤지를 비교 할 수있는 타입
- Equatable 프로토콜을 준수하는 타입은 등호 연산자 (==) 또는 같지 않음 연산자 (!=)를 사용하여 동등성을 비교할 수 있음
- Swift 표준 라이브러리의 대부분 기본 데이터타입(String, Double, Int, Floot, Bool 등)은 Equatable를 따름

### 클래스, 구조체, 열거형의 인스턴스 비교

#### 클래스

- **Equatable 프로토콜을 채택**
- **해당 구조체 내의 모든 프로퍼티(name, age)가 같아야만 true임**

```swift
struct Human: Equatable {
    var name = ""
    var age = 0
}
 
let human1 = Human.init()
let human2 = Human.init(name: "", age: 19)
 
human1 == human2        // false
```

-  이름이 같으면 같은 인스턴스로 취급하고 싶은 경우

  ```swift
  struct Human: Equatable {
      var name = ""
      var age = 0
      
      static func == (lhs: Self, rhs: Self) -> Bool {
          return lhs.name == rhs.name
      }
  }
   
  let human1 = Human.init()
  let human2 = Human.init(name: "", age: 19)
   
  human1 == human2        // true
  ```

#### 구조체

- **클래스의 경우 Equatable을 채택하는 것만으로 자동으로 구현해주지 않음**

- **직접 == 메서드를 구현**해야 함

```swift
class Human {
    var name = ""
    var age = 0
}
 
extension Human: Equatable {
    static func == (lhs: Human, rhs: Human) -> Bool {
        return lhs.name == rhs.name && lhs.age == rhs.age
    }
}
 
let human1 = Human.init()
let human2 = Human.init()
human2.age = 10
 
human1 == human2        // false
```



#### 열거형

- 연관값이 있는 경우(**Equatable을 채택하지도 않아도 자동으로 구현**)

  ```swift
  enum Gender {
      case male
  }
   
  let man = Gender.male
  let man2 = Gender.male
   
  man == man2   // true
  ```

- 연관값이 없는 경우(**Equatable을 채택만 해줘도 자동으로 구현**)

  ```swift
  enum Gender: Equatable {
      case male(name: String)
  }
   
  let man = Gender.male(name: "so")
  let man2 = Gender.male(name: "deul")
   
  man == man2   // false
  
  ```


## IndexPath

- **tableView나 collectionView의 행을 식별하는 인덱스 경로**

- init(row: Int, section: Int)
- indexPath는 [section,row]로 이루어져 있는 행을 식별하는 상대적인 경로

