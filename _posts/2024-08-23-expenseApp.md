---  
layout: post  
title:  "Expense Tracking App"  
categories: [ React Native ]  
image: assets/img/20240821_01.png  
tags: [featured]  
---  
  
# Expense Tracking App  
  
🛠️**error fix**   
Expo를 사용하면 React Native 프로젝트에서 모듈을 설치하고 구성하는 데 필요한 작업이 자동으로 처리된다. expo가 환경을 설정하고 필요한 네이티브 종속성을 관리할 수 있게 실행한 채로 설치 명령어를 실행해야 누락되지 않고 안정적으로 작동한다.  
  
Xcode simulator는 settings-platforms에서 simulator 버전을 선택해서 따로 install해줘야 함  
![1]({{ site.baseurl }}/assets/img/20240823_03.png)   
  
- 프로젝트 생성 후  
`npm install @react-navigation/native`  
- 추가 의존성 설치     
`expo install react-native-screens react-native-safe-area-context`    
- 네이티브 스택 내비게이션을 구현하기 위한 라이브러리    
`expo install @react-navigation/native-stack`    
- 탭(Tap) 내비게이터    
`npm install @react-navigation/bottom-tabs`    
  
- stack : 한 화면에서 다른 화면으로 이동할 때 새 화면이 위에 추가되는 구조.  
뒤로가기 히스토리를 관리하는 역할을 한다. LIFO(Last In, First Out)구조를 가지며 마지막에 추가된 항목이 가장 먼저 제거되는 특징이 있음  
  
- payload : 특정 동작이나 작업을 수행하기 위해 전달되는 데이터  
  
- `switch (action.type)` : action.type은 Redux나 useReducer와 같은 상태 관리에서 많이 사용되며, 상태를 변경하기 위한 액션의 종류를 나타낸다. switch (action.type)는 action.type의 값에 따라 다른 코드를 실행하게 된다.  
  
  
  
<video controls width="600">          
  <source src="/NextGenWebDev/assets/img/20240820_01.mp4" type="video/mp4">          
  Your browser does not support the video tag.          
</video>     
  
  
  
# Handling User Input (Fetching & Validating User Input)  
  
<video controls width="600">          
  <source src="/NextGenWebDev/assets/img/20240822_02.mp4" type="video/mp4">          
  Your browser does not support the video tag.          
</video>    
  
  
# Sending HTTP Requests  
![2]({{ site.baseurl }}/assets/img/20240821_01.png)    
  
- 모바일 앱을 구축 중이므로 데이터 교환과 관련된 API가 필요  
  
### Firebase  
- 더미 백엔드로 사용해서 자체 백엔드 구축할 필요가 없음(하지만 Node나 PHP등 나중에 다른 백엔드에서도 활용 가능함)  
- 데이터를 보내고 받기만 하면 됨(graphAPI나 REST API를 사용하면 됨)  
- 프로젝트 생성 후 Realtime Database : 사용하기가 상대적으로 좀 더 쉽고 REST API를 제공  
- 우리에게 데이터베이스에 직접 요청을 보내는 것처럼 보이지만 실제로는 API에 요청을 보내는 것이며 Google Firebase가 백엔드에서 데이터베이스 요청으로 변환한다.  
- Location setting : 기본값으로  
- security는 team mode로 시작해야 인증 없이 읽기와 쓰기를 할 수 있다.  
![3]({{ site.baseurl }}/assets/img/20240821_02.png)    
  
### Axios 설치  
- API : 두 시스템 간의 중개자 역할 (예 : 서버, 클라이언트)  
- Axios : HTTP 클라이언트 (예 : 브라우저, 서버 간에 HTTP 요청을 보내고 응답을 받을 수 있도록 도와줌)  
- fetch API를 사용  
- Axios 라이브러리를 많이 씀 : 프로미스 기반의 HTTP 클라이언트로 브라우저와 React Native에서 사용할 수 있다.  
- `npm install axios` 설치  
- 코드 URL 끝에 세그먼트 + .json  
- Firebase에서는 끝에 .json을 붙여 데이터베이스의 특정 노드를 대상으로 한다는 것을 알려줘야 함  
- 세그먼트는 노드나 폴더로 전환된다.  
- 고유 ID는 Firebase가 자동으로 생성해줌  
  
  
  