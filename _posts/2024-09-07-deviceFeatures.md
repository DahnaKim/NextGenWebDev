---  
layout: post  
title:  "Using Native Device Features"  
categories: [ React Native ]  
image: assets/img/20240907_01.png  
tags: [featured]  
---  
  
# Using Native Device Features  
  
**native-features 프로젝트, 주석 참조**  
  
`npm install @react-navigation/native`    
`expo install @react-navigation/native-stack`    
  
**카메라**   
`npx expo install expo-image-picker`  
app.json 마지막에   
  
```  
 "plugins": [  
      [  
        "expo-image-picker",  
        {  
          "cameraPermission": "The app needs access to your camera in order to take photos of your favorite places."  
        }  
      ]  
    ]  
```  
  
iOS에서는 카메라 권한을 따로 직접 설정해줘야 함  
  
**위치**  
`npx expo install expo-location`  
  
**지도 미리보기 기능**  
- Google Maps Static API 사용  
https://developers.google.com/maps/documentation/maps-static/overview  
  
- URL of Maps Static API  
https://maps.googleapis.com/maps/api/staticmap?center=${lat},${lng}&zoom=14&size=400x200&maptype=roadmap  
&markers=color:red%7Clabel:S%7C${lat},${lng}  
&key=${GOOGLE_API_KEY}  
  
**상호작용할 수 있는 지도 추가하기**  
`npx expo install react-native-maps`  
  
**선택된 장소를 사람이 알 수 있는 주소로 변환하기**  
- Google geocoding  
https://developers.google.com/maps/documentation/geocoding/requests-reverse-geocoding  
https://maps.googleapis.com/maps/api/geocode/json?latlng=${lat},${lng}&key=${GOOGLE_API_KEY}  
  
**데이터 저장**  
`npx expo install expo-sqlite`  
  
- expo-module-scripts패키지 설치  
`npm install expo-module-scripts --save-dev`  
  
  
- ❌(더이상 사용되지 않음)`expo install expo-app-loading` 대신  
✅ `expo install expo-splash-screen` : expo-splash-screen에서 제공하는 SplashScreen.preventAutoHideAsync()와 SplashScreen.hideAsync() 메서드를 사용해서 스플래시 화면을 제어해야 함  (App.js에서 AppLoading 대신)  
  
  
### 데이터베이스의 트랜잭션 단위  
데이터베이스 작업을 트랜잭션 단위로 실행하는 것은 데이터베이스에서 여러 작업을 하나의 단위로 묶어서 실행하는 방식이다. 이를 통해 데이터의 일관성, 무결성, 안전성을 보장할 수 있다. 트랜잭션은 보통 여러 SQL 작업(삽입, 수정, 삭제 등)을 하나로 묶어서 처리하며, 트랜잭션 내에서 작업이 성공적으로 완료되면 데이터를 실제로 변경하고, 실패 시에는 변경된 데이터를 롤백(되돌리기)하여 데이터베이스의 상태를 이전 상태로 복구한다.  
  
**트랜잭션의 핵심 요소 (ACID 원칙)**  
- Atomicity (원자성):  
트랜잭션은 "모두 성공하거나 모두 실패해야 한다"는 것을 의미한다. 트랜잭션 내의 작업이 일부만 실행되고 중간에 오류가 발생하면, 그 작업들은 모두 취소된다. 즉, 작업이 완전히 성공하거나, 실패 시 아무것도 반영되지 않는다.  
- Consistency (일관성):  
트랜잭션이 시작되기 전과 완료된 후의 데이터 상태가 일관성이 있어야 한다는 원칙. 트랜잭션이 완료된 후에도 데이터베이스는 정의된 규칙(무결성 제약 조건 등)을 만족해야 한다.  
예를 들어, 은행 계좌 간 송금을 할 때, A 계좌에서 금액이 빠져나가면 B 계좌에는 반드시 그 금액이 입금되어야 한다. 그렇지 않으면 데이터베이스는 일관성을 잃게 된다.  
- Isolation (고립성):  
각 트랜잭션은 독립적으로 실행되어야 하며, 동시에 실행되는 트랜잭션들이 서로의 작업에 간섭해서는 안 된다. 한 트랜잭션이 완료되기 전에는 다른 트랜잭션에서 그 결과를 볼 수 없도록 고립된 상태에서 작업이 수행된다.  
예를 들어, 하나의 트랜잭션이 데이터를 수정하고 있을 때, 다른 트랜잭션은 그 수정 작업이 완료되기 전까지는 수정 중인 데이터를 볼 수 없다.  
- Durability (지속성):  
트랜잭션이 성공적으로 완료되면, 그 결과는 영구적으로 데이터베이스에 저장되어야 한다. 서버가 갑자기 종료되거나 시스템에 장애가 발생하더라도 트랜잭션의 결과는 유지되어야 한다.  
예를 들어, 데이터를 삽입한 후 시스템이 꺼지더라도 삽입된 데이터는 손실되지 않고 유지된다.  
  
**객체 스프레드 연산자(...)**   
객체의 속성들을 복사하여 새로운 객체를 만드는 데 사용되는 JavaScript의 문법  
  
- 리액트에서 useCallback과 useMemo와 같은 훅을 사용해 메모이제이션을 적용할 수 있다. 의존성 배열에 지정된 값이 변하지 않으면 함수가 재생성되지 않도록 함.  
- && 연산자는 JavaScript에서 조건부 실행을 위한 문법. selectedLocation이 존재할 때만 && 뒤의 코드가 실행되어 마커가 렌더링된다.  
-interface: 주로 객체의 구조를 정의하는 데 사용된다. 특히 클래스나 객체의 형태를 설명할 때 많이 사용된다.  
다른 인터페이스를 확장할 수 있으며, 여러 인터페이스를 동시에 확장할 수 있다.  
- type: 보다 일반적인 타입을 정의할 때 사용되며, 객체뿐만 아니라 유니언, 튜플, 기본 타입 등의 다양한 구조를 정의할 수 있다.  
type은 확장할 수 있지만 &(교차 타입)을 사용해야 한다.  
  
<video controls width="600">            
  <source src="/NextGenWebDev/assets/img/20240831_01.mp4" type="video/mp4">            
  Your browser does not support the video tag.            
</video>       
    