---  
layout: post  
title:  "Push Notifications"  
categories: [ React Native ]  
image: assets/img/20240913_01.png  
tags: [featured]  
---  
  
# Push Notifications  
  
## Lacal Notifications  
- 로컬 알림 : 앱이 설치된 기기에서 설치된 앱이 트리거하는 알림  
앱 내부에서 간단히 알림 일정을 설정하고 전송을 구성하고 기기에서 알림을 수신했을 때 동작을 설정할 수 있다. (ex : 알림, 알람 등)  
  
- Expo Notification을 사용. simulator에서 테스트할 수 없음 실제 기기 필요  
- `npx expo install expo-notifications` : 설치  
- 나중에 배포할 때 문서 꼭 참조해야 함  
https://docs.expo.dev/versions/latest/sdk/notifications/#api  
  
  
### 로컬 알림 - 권한  
Expo Go를 사용할 때는 로컬 알림(혹은 일반 알림)을 전송, 혹은 표시할 권한을 요구할 필요가 없지만 나중에 프로덕션용으로 앱을 구축하게 되면 권한을 요구할 필요가 생길 수 있다.  
Android에서는 변경 사항이 없으며, iOS에서는 expo-notifications가 제공하는 getPermissionsAsync()  메서드를 사용해 현재 권한 상태를 받을 수 있다. requestPermissionsAsync()를 사용해 권한을 요청할 수 있다.  
  
### Push 알림  
다른 기기에서 오는 메시지, ex: 채팅, 마케팅 메시지 등  
PUSH 알림의 기본은 항상 알림이 발생한 기기에서 다른 기기로 전달된다.  
스팸 등을 방지하기 위해 보안이유로 Google과 Apple이 제공하는 백엔드를 사용해서 다른 기기에 PUSH 알림을 보내도록 강제함(Expo 내부에서 Apple과 Google의 서버와 통신해서도 보낼 수 있음)  
앱이 설치된 기기로 PUSH 알림을 전송하기 위해서는 PUSH 알림 서버에 메시지를 보내야 함. 결국 HTTP 요청인 셈  
  
- Expo Notifications 패키지 설치  
- 권한 요청  
- PUSH 알림을 보낼 주소로 동작할 ExpoPushToken이 필요함  
  
- [PUSH 알림 테스트할 때 사용할 수 있는 툴](https://expo.dev/notifications)  
- ExponentPushToken을 포함하는 전체 토큰 붙여넣기  
- Message title, Message body 채우기  
  
✅ EAS를 사용해서 projectId가 있어야 실제 기기에 테스트할 수 있도록 변경됨  
  