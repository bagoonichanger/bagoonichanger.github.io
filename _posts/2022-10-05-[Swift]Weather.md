---
layout : single
title : "[Swift]Weather"
categories : Swift
tag : [Swift]
toc : true
author_profile : false
sidebar :
    nav : "docs"
---

<img src="https://raw.githubusercontent.com/bagoonichanger/bagoonichanger.github.io/upload_Image/images/202210061433844.png" alt="Simulator Screen Shot - iPhone 14 Pro - 2022-10-06 at 14.33.29" style="zoom:33%;" />

## Codable

- 자신을 외부 표현으로 변환(Encode)하거나 외부 표현으로 변환(Decode)할 수 있는 유형

- Codable = Decodable & Encodable



## CodingKey

인코딩 및 디코딩을 위한 키로 사용할 수 있는 유형



```swift
// MARK: - Welcome
struct WeatherInformation: Codable {
    let weather: [Weather]
    let main: Main
    let name: String
}

// MARK: - Main
struct Main: Codable {
    let temp, feelsLike, tempMin, tempMax: Double
    let pressure, humidity: Int

    enum CodingKeys: String, CodingKey {
        case temp
        case feelsLike = "feels_like"
        case tempMin = "temp_min"
        case tempMax = "temp_max"
        case pressure, humidity
    }
}


// MARK: - Weather
struct Weather: Codable {
    let id: Int
    let main, weatherDescription, icon: String

    enum CodingKeys: String, CodingKey {
        case id, main
        case weatherDescription = "description"
        case icon
    }
}
```

## URLSession

- 관련된 네트워크 데이터 전송 작업 그룹을 조정하는 개체
- 특정한 URL을 이용하여 데이터를 다운로드하고 업로드하기 위한 API

### URLSession Life Cycle

1. Session Configuration을 결정하고, Session 을 생성
2. 통실한 URL과 Request 객체를 설정
3. 사용할 Task 를 결정하고 그에 맞는 Completion Handler 나 Delegate 메소드들을 작성
4. 해당 Task 를 실행
5. Task 완료 후 Completion Handler 클로저가 호출이 됨

```SWIFT
func getCurrentWeather(cityName: String) {
        guard let url = URL(string: "https://api.openweathermap.org/data/2.5/weather?q=\(cityName)&appid=d12719ff6add0324fbfe64e247fcd42f") else { return }
        let session = URLSession(configuration: .default)
        session.dataTask(with: url) { [weak self] data, response, error in
            let successRange = (200..<300)
            guard let data = data, error == nil else { return }
            let decoder = JSONDecoder()
            if let response = response as? HTTPURLResponse, successRange.contains(response.statusCode){
                guard let weatherInformation = try? decoder.decode(WeatherInformation.self, from: data) else { return }
                DispatchQueue.main.async {
                    self?.weatherStackView.isHidden = false
                    self?.configureView(weatherInformation: weatherInformation)
                }
            } else {
                guard let errorMessage = try? decoder.decode(ErrorMessage.self, from: data) else { return }
                DispatchQueue.main.async {
                    self?.showAlert(message: errorMessage.message)
                }
            }
            
        }.resume()
    }
```

### URLSessionConfiguration을 통해 URL 생성

- .default: 기본 통신을 할때 사용 (쿠키와 같은 저장 객체 사용)
- .ephemeral: 쿠키나 캐시를 저장하지 않는 정책을 사용할 때 이용
- .background: 앱이 백그라운드 상태에 있을 때 컨텐츠를 다운로드/업로드

### URLSessionTask 작업 유형

- URLSessionDataTask: 기본적인 데이터를 받는 경우, response데이터를 메모리 상에서 처리
- URLSessionUploadTask: 파일 업로드 시 사용, 사용하기 편한 request body 제공
- URLSessionDownloadTask: 실제 파일을 다운받아 디스크에 사용될때 사용
- URLSessionStreamTask: 파일 업로드 시 사용, 사용하기 편한 request body 제공
- URLSessionWebSocketTask: 실제 파일을 다운받아 디스크에 사용될때 사용

