---
layout : single
title : "[Swift]Pomodoro"
categories : Swift
tag : [Swift]
toc : true
author_profile : false
sidebar :
    nav : "docs"
---

## DispatchSourceTimer

A dispatch source that submits the event handler block based on a timer

```swift
var timer: DispatchSourceTimer?

//원하는 쓰레드 지정
//UI와 관련된 부분은 모두 메인 쓰레드에서 담당
timer = DispatchSource.makeTimerSource(flags: [], queue: DispatchQueue.main)
    
//바로시작을 원하니까 dedline은 .now(), 반복되는 시간(타이머니까 1초)
timer?.schedule(deadline: .now(), repeating: 1)

//원하는 함수호출 지정
timer?.setEventHandler(handler: {
    원하는 이벤트 함수
})

//타이머 시작
timer?.resume()

//타이머일시정지
timer?.suspend() // 일시정지된 상태에서 종료를 누르면 Runtime Error 가 발생. 따라서 취소하기 전에 타이머의 상태를 resume 으로 만들어줘야함

//종료(nil을 반드시 해줄것)
timer?.cancel()
timer = nil // 안하면 계속 시간감
```



## AudioToolbox

The AudioToolbox framework는 녹음, 재생 및 스트림 구문 분석을 위한 인터페이스를 제공.
iOS에서 프레임워크는 오디오 세션을 관리하기 위한 추가 인터페이스를 제공.

### AudioServicesPlaySystemSound

``` swift
func AudioServicesPlaySystemSound(_ inSystemSoundID: SystemSoundID)	
```

**inSystemSoundID**

재생할 시스템 사운드. 이 함수를 사용하기 전에 AudioServicesCreateSystemSoundID(*:*:) 함수를 호출하여 시스템 사운드를 얻어야 함

>  https://iphonedev.wiki/index.php/AudioServices



## Animate

지정된 기간, 지연, 옵션 및 완료 처리기를 사용하여 하나 이상의 뷰에 변경 사항을 애니메이션화

```swift
class func animate(
    withDuration duration: TimeInterval,
    delay: TimeInterval,
    options: UIView.AnimationOptions = [],
    animations: @escaping () -> Void,
    completion: ((Bool) -> Void)? = nil
)
```

### Parameters

- **`Duration`**
  이 시간만큼 애니메이션이 동작. 단위는 초이고, 예를 들어 `withDuration : 1.0` 이면 애니메이션이 1초동안 동작.
- **`Delay`**
  이 시간만큼 딜레이를 가진 다음에 애니메이션이 동작. 이것도 역시 초단위.
- **`Options`**
  애니메이션에 다양한 옵션을 줄 수 있다. [UIView.AnimationOptions](https://developer.apple.com/documentation/uikit/uiview/animationoptions) 에 다양한 옵션들이 있음
- **`Animations`**
  Duration 동안 애니메이션이 동작하는 구간. 절대 널 값이 올 수 없음!
  (원문 : **This block takes no parameters and has no return value. This parameter must not be NULL.**)
- **`Completion`**
  Duration 동안의 애니메이션이 끝나고 나서 동작하는 구간.

```swift
UIView.animate(withDuration: 0.5, animations: {
                self.timerLabel.alpha = 1
                self.progressView.alpha = 1
                self.datePicker.alpha = 0
            })
```



### CGAffineTransform

2D 그래픽을 그리는 데 사용하기 위한 아핀 변환 행렬.

아핀 변환 행렬은 그래픽 컨텍스트에서 그리는 개체를 회전, 축척, 변환 또는 왜곡하는 데 사용
CGAfineTransform 유형은 아핀 변환을 생성, 연결 및 적용하기 위한 기능을 제공
아핀 변환은 3x3 행렬로 표현

이동, 축척 또는 회전에 따라 도면을 조작하는 가장 직접적인 방법은 함수 translateBy(x:y:), scaleBy(x:y:) 또는 rotate(by:)를 각각 호출하는 것

일반적으로 나중에 다시 사용하려는 경우에만 아핀 변환을 만들어야 함

### Translate

```swift
let moveAnimation = CGAffineTransform(translationX: 0, y: -200)
```

### Scale

```swift
  let scaleAnimation = CGAffineTransform(scaleX: 2.0, y: 2.0)      		
```

### Rotate

```swift
let rotateAnimation = CGAffineTransform(rotationAngle: .pi)        
```
