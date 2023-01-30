---
layout : single
title : "[Swift]Drink"
categories : Swift
tag : [Swift]
toc : true
author_profile : false
sidebar :
    nav : "docs"
---

<img src="https://raw.githubusercontent.com/bagoonichanger/bagoonichanger.github.io/upload_Image/images/202301281232904.png" alt="Simulator Screen Shot - iPhone 14 Pro - 2023-01-28 at 12.31.13" style="zoom:33%;" />

## Property List

객체 직렬화를 위한 XML 포맷에 맞추어 key-value 형식으로 저장(plist)

> 객체 직렬화 : 객체의 내용을 바이트 단위로 변환하여 파일에 기록하거나 네트워크를 통해 전달이 가능하도록 하는것

**저장타입**

 \- 문자열 : <string>
 \- 숫자 : <integer>
 \- 실수 : <real>
 \- 배열 : <array>
 \- 딕셔너리 : <dict>
 \- 날짜, Base64* : <data>

> Base64 : 8bit로 된 바이너리 데이터 -> 아스키 코드



## UserDefaults

**UserDefaults에는 property list 객체만 저장**

-> **PropertyListEncoder**, **PropertyListDecoder**를 사용

- 객체를 **PropertyList**로 변환하기 위해서 **PropertyListEncoder**를 사용

- **PropertyList**를 객체로 변환하기 위해서 **PropertyListDecoder**를 사용

```swift
 @IBAction func alertSwitchValueChanged(_ sender: UISwitch) {
        guard let data = UserDefaults.standard.value(forKey: "alerts") as? Data,
              var alerts = try? PropertyListDecoder().decode([Alert].self, from: data) else { return }
        
        alerts[sender.tag].isOn = sender.isOn
        UserDefaults.standard.set(try? PropertyListEncoder().encode(alerts), forKey: "alerts")
        
        if sender.isOn {
            userNotificationCenter.addNotificationRequest(by: alerts[sender.tag])
        } else {
            userNotificationCenter.removePendingNotificationRequests(withIdentifiers: [alerts[sender.tag].id])
        }
    }
```

## Local Notification

### 기본구조

`content` `trigger` `request`

#### Content

사용자에게 어떤 내용을 보여줄지에 대한 정보를 담고 있습니다.

 `title`, `body`, `badge number`, `userInfo`, `attachments` 등

#### Trigger

`time`, `calendar`, `location`

일정 시간이 지난 후에 작동되길 원한다면 `time`
특정한 날짜에 작동하기 원한다면 `calendar` 
특정 위치에 진입할 경우 혹은 나갈 경우에 작동하기 원한다면 `location`

#### Request

`content`와 `trigger`를 가지고 로컬 푸쉬를 등록하기 위해 생성

```swift
import UserNotifications

extension UNUserNotificationCenter{
  //사용자에게 알림을 보냄
    func addNotificationRequest(by alert: Alert){
        let content = UNMutableNotificationContent()
        content.title = "물 마실 시간이에요💦"
        content.body = "세계보건기구(WHO)가 권장하는 하루 물 섭취량은 1.5~2리터 입니다."
        content.sound = .default
        content.badge = 1
        
        let component = Calendar.current.dateComponents([.hour, .minute], from: alert.date)
        let trigger = UNCalendarNotificationTrigger(dateMatching: component, repeats: alert.isOn)
        let request = UNNotificationRequest(identifier: alert.id, content: content, trigger: trigger)
        
        self.add(request, withCompletionHandler: nil)
    }
    
}
```

```swift
import UIKit
import NotificationCenter
import UserNotifications

@main
class AppDelegate: UIResponder, UIApplicationDelegate {
    
    let userNotificationCenter = UNUserNotificationCenter.current()
    
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        //앱이 완전히 기동되기 전에 할당
        UNUserNotificationCenter.current().delegate = self
        // 사용자에게 알림 권한을 요청
        let authroizationOptions = UNAuthorizationOptions(arrayLiteral: [.alert, .badge, .sound])
        userNotificationCenter.requestAuthorization(options: authroizationOptions){_ ,error in
            if let error = error {
                print("ERROR: notification authrization request \(error.localizedDescription )")
            }
        }
        return true
    }
    
    // MARK: UISceneSession Lifecycle
    
    func application(_ application: UIApplication, configurationForConnecting connectingSceneSession: UISceneSession, options: UIScene.ConnectionOptions) -> UISceneConfiguration {
        // Called when a new scene session is being created.
        // Use this method to select a configuration to create the new scene with.
        return UISceneConfiguration(name: "Default Configuration", sessionRole: connectingSceneSession.role)
    }
    
    func application(_ application: UIApplication, didDiscardSceneSessions sceneSessions: Set<UISceneSession>) {
        // Called when the user discards a scene session.
        // If any sessions were discarded while the application was not running, this will be called shortly after application:didFinishLaunchingWithOptions.
        // Use this method to release any resources that were specific to the discarded scenes, as they will not return.
    }
    
    
}

//알림을 받고난 후의 이벤트 핸들링(앱이 완전히 기동되기 전에 할당)
extension AppDelegate: UNUserNotificationCenterDelegate {
    func userNotificationCenter(_ center: UNUserNotificationCenter, willPresent notification: UNNotification, withCompletionHandler completionHandler: @escaping (UNNotificationPresentationOptions) -> Void) {
        completionHandler([.banner, .list, .badge, .sound])
    }

    func userNotificationCenter(_ center: UNUserNotificationCenter, didReceive response: UNNotificationResponse, withCompletionHandler completionHandler: @escaping () -> Void) {
        completionHandler()
    }
}
```



