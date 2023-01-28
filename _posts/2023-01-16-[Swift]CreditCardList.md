---
layout : single
title : "[Swift]CreditCardList"
categories : Swift
tag : [Swift]
toc : true
author_profile : false
sidebar :
    nav : "docs"
---

<img src="https://raw.githubusercontent.com/bagoonichanger/bagoonichanger.github.io/upload_Image/images/202301161718879.png" alt="Simulator Screen Shot - iPhone 14 Pro - 2023-01-16 at 17.05.48" style="zoom:33%;" />

## Lottie Animation

> Lottie는 JSON 기반의 애니메이션 파일 형식

LottieFiles App : https://apps.apple.com/us/app/lottiefiles/id1231821260 

LottieFiles 사용법 : https://lottiefiles.com/kr/blog/tutorials/kr-how-to-add-lottie-animation-ios-app-swift

```swift
@IBOutlet weak var lottieView: LottieAnimationView!

override func viewDidLoad() {
        super.viewDidLoad()

        let animationView = LottieAnimationView(name: "money")
        lottieView.contentMode = .scaleAspectFit
        lottieView.addSubview(animationView)
        animationView.frame = lottieView.bounds
        animationView.loopMode = .loop
        animationView.play()
}
```

