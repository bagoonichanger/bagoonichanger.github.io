---
layout : single
title : "[Swift]TodoList"
categories : Swift
tag : [Swift]
toc : true
author_profile : false
sidebar :
    nav : "docs"
---

<img src="https://raw.githubusercontent.com/bagoonichanger/bagoonichanger.github.io/upload_Image/images/202209182240751.png" alt="Simulator Screen Shot - iPhone 13 - 2022-09-18 at 22.39.04" style="zoom: 20%;" />



## TableView

### DataSource

>  **테이블 뷰를 생성하고 수정하는데 필요한 정보를 테이블 뷰 객체에 제공**

![스크린샷 2022-09-18 오후 10.44.49](https://raw.githubusercontent.com/bagoonichanger/bagoonichanger.github.io/upload_Image/images/202209182244775.png)

### Delegate

> **테이블 뷰의 시각적인 부분을 설정하고, 행의 액션 관리, 액세서리 뷰, 지원 그리고 테이블 뷰의 개별 행 편집을 지원**



![스크린샷 2022-09-18 오후 10.54.35](https://raw.githubusercontent.com/bagoonichanger/bagoonichanger.github.io/upload_Image/images/202209182254862.png)



## CompactMap

#### Map

```swift
let possibleNumbers = ["1", "2", "three", "///4///", "5"]

let mapped = possibleNumbers.map { str in Int(str) }
// [Optional(1), Optional(2), nil, nil, Optional(5)]
```

#### compactMap

```swift
let possibleNumbers = ["1", "2", "three", "///4///", "5"]

let compactMapped = possibleNumbers.compactMap { str in Int(str) }
// [1, 2, 5]
```

`map` 을 사용했을 때는 `Optional` 이지만

`compactMap` 을 사용하니 `Int` 에 해당하는 값만 들어감
-> `compactMap` 이 옵셔널 바인딩의 기능(`map` 의 원래 기능 + 옵셔널 제거)

## UserDefaults

- UserDefaults 는 런타임 환경에서 동작하면서, 앱이 실행되는 동안 기본 저장소 (default database)에 접근해 데이터를 기록하고, 가져오는 역할을 하는 인터페이스
- 앱의 기본 데이터베이스에 영구적으로 데이터를 저장할 수 있는 인터페이스
- **key - value 쌍**으로 저장
- **Singleton** 객체, 앱 전체에서 하나의 인스턴스로 사용
- `float`, `double`, `NSData`, `NSString` 등의 데이터 타입 저장, 불러오기
- 사용자 기본 설정과 같은 단일 데이터 값에 적합



```swift
open class var standard: UserDefaults { get }
    
// 데이터 불러오기
open func object(forKey defaultName: String) -> Any?
open func string(forKey defaultName: String) -> String?
open func array(forKey defaultName: String) -> [Any]?
    
// 데이터 저장하기
open func set(_ value: Any?, forKey defaultName: String)
    
// 데이터 지우기 
open func removeObject(forKey defaultName: String)
```



## UIAlertController

앱 실행 도중에 사용자에게 메시지를 전달하고 의사를 입력받기 위한 목적으로 제공되는 객체



#### **UIAlertView** VS **UIActionSheet**

**UIAlertView(알림창- 화면 중앙) +  UIActionSheet(액션시트 - 화면 하단) = UIAlertController**



 UIAlertView은 **모달(Modal) 방식**

>  모달(Modal) - 창이 닫힐 때까지 그 창을 제외한 화면의 다른 부분은 반응할 수 없도록 잠기는 것



####  메세지창 구현

```swift
// 메시지창 컨트롤러 인스턴스 생성
let alert = UIAlertController(title: "알림", message: "알림 샘플 코드 입니다.", preferredStyle: UIAlertController.Style.alert)



// 메시지 창 컨트롤러에 들어갈 버튼 액션 객체 생성
let defaultAction =  UIAlertAction(title: "default", style: UIAlertAction.Style.default)
let cancelAction = UIAlertAction(title: "cancel", style: UIAlertAction.Style.cancel, handler: nil)
let destructiveAction = UIAlertAction(title: "destructive", style: UIAlertAction.Style.destructive){(_) in
    // 버튼 클릭시 실행되는 코드
    
}

//메시지 창 컨트롤러에 버튼 액션을 추가
alert.addAction(defaultAction)
alert.addAction(cancelAction)
alert.addAction(destructiveAction)

//메시지 창 컨트롤러를 표시
self.present(alert, animated: false)
```

##### UIAlertAction

 **title**은 사용자에게 보이는 버튼의 이름을 설정



 **style**은 버튼 타입을 결정합니다. 타입의 종류는 default, cancel, destructive 세 가지

1 .**cancel** :아무것도 실행되지 않은 채 메시지 창의 액션이 취소된다는 것을 뜻하며, 메시지 창 내에서 한 번만 사용(두 개 이상 사용시 런타임 오류 발생). 

2 .**destructive**: 주의해서 선택해야 할 버튼에 사용. 이 타입이 적용된 버튼은 빨간색으로 강조되며, 중요한 내용을 변경하거나 삭제해서 되돌릴 수 없는 결정을 하는 버튼에 적용

3 .**default**: 위 두 가지 스타일 이외에 일반적인 버튼에 사용

 

**handler**는 버튼을 클릭했을 때 실행될 구문. 구문 실행을 위해서 함수 또는 클로저를 사용. 아무일도 일어나지 않는 다면 생략하거나, handler: nil 과 같이 표시.
