---
layout : single
title : "[Swift]LEDBoard"
categories : Swift
tag : [Swift]
toc : true
author_profile : false
sidebar :
    nav : "docs"
---

<img src="https://raw.githubusercontent.com/bagoonichanger/bagoonichanger.github.io/upload_Image/images/202209142337786.png" alt="스크린샷 2022-09-14 오후 11.35.56" style="zoom: 33%;" />



## Delegate는 왜 Weak로 선언할까?

**기본적으로 class의 instance를 가리키는 각각의 reference는 강함 참조(Strong reference)**

![다운로드](https://raw.githubusercontent.com/bagoonichanger/bagoonichanger.github.io/upload_Image/images/202209121616349.png)

두 클래스 인스턴스가 서로 강한 참조를 하게 되면 Retain Cycle(순환 참조) 발생

-> 여전히 객체가 남아있지만, 해당 클래스에 대한 reference를 찾을 수 없어 deallocate가 안됨

 -> **메모리 누수(memory leak) 발생!**



### Weak

weak reference (약한 참조) 를 사용하는 것은 retain cycle을 피할 수 있는 방법 중 하나

**reference를 weak으로 선언한다면, 이 reference가 인스턴스의 deallocate를 방해하지 않는다는 의미**

-> reference가 nil 이면, 인스턴스 deallocate

->(Optional Variable만이 nil이 될 수 있기 때문에, 모든 weak이 붙은 변수들은 반드시 옵셔널 이여야 한다.)



## 화면 전환

- View Controller의 View 위에 다른 View를 가져와 바꿔치기

- View Controller에서 다른 View Controller를 호출하여 전환하기

  > Func present()
  >
  > Func dismiss()

- Navigation Controller를 사용하여 화면 전환하기

  > Func pushViewController()
  >
  > Func popViewController()

- 화면 전환용 객체 세그웨이(Segueway)를 사용하여 화면 전환하기 

  > **Action** VS **Menual**(like code style)
  >
  > - Action Segueway 종류
  >   - show
  >   - Show Detail
  >   - Present Modally
  >   - present As Popover
  >   - Custom
  >      

### 화면만 데이터 전달

- 코드로 구현된 화면 전환 방법에서 데이터 전달하기

- 세그웨이로 구현된 화면 전환 방법에서 데이터 전달하기

- **Delegate 패턴을 이용하여 이전 화면으로 데이터 전달하기(제일 전형적인 방법)**

  
