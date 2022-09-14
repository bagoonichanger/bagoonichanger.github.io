---
layout : single
title : "[Swift]Calculator"
categories : Swift
tag : [Swift]
toc : true
author_profile : false
sidebar :
    nav : "docs"
---

<img src="https://raw.githubusercontent.com/bagoonichanger/bagoonichanger.github.io/upload_Image/images/202209150002763.png" alt="스크린샷 2022-09-14 오후 11.38.51" style="zoom: 50%;" />

## IBInspectable / IBDesignable

> IB : Interface Builder ==> 스토리보드(Stroyboard)를 말하는 것



**@IBInspectable** :  inspector 영역에서 해당 인터페이스 요소의 속성을 변경(추가)할 수 있게 하는 것

  

**@IBDesignable** :  inspector에서 속성 변경 값을 storyboard 에서 실시간 랜더링하기



## truncatingRemainder

% 모듈러스 연산자는 정수형에서만 정의

```swift
var a = 10.0.truncatingRemainder(dividingBy: 3) // 1
```
