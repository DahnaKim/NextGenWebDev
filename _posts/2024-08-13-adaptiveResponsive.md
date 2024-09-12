---  
layout: post  
title:  "Adaptive & Responsive UIs"  
categories: [ React Native ]  
image: assets/img/20240813_01.png  
tags: [featured]  
---  
  
# Adaptive & Responsive UIs (Building Cross-Platform & Device User Interfaces)  
  
### 동적 너비 설정하기  
- maxWidth, minWidth 등을 사용해서 크기에 따라 반응하도록 설정(부모 컨테이너가 기준)  
  
### Dimensions API   
- import해서 react-native로부터 Dimensions 불러오기 : JavaScript 객체라서 styles를 포함한 JavaScript코드 어디에서나 정보 추출을 위해 사용할 수 있다.   
- get 메서드를 통해 화면의 치수를 지님  
- 안드로이드에서 screen은 상태표시줄 포함, window는 제외한 사이즈  
- `padding: deviceWidth < 450 ? 12 : 24` : deviceWidth가 450픽셀보다 작으면 padding 값으로 12 아니면 24  
- 이미지 크기 조정 가능   
- 하드 코딩은 앱이 어떤 크기의 화면에서 실행될지 모르기 때문에 좋은 방식이 아님  
  
### 화면 방향에 따라 크기를 동적으로 설정하기  
- app.json에 `"orientation": "portrait"` 방향이 세로로 잠겨 있음 default로 변경하고 재실행  
- Dimensions API 사용의 한계점 : 처음으로 구문 분석되어 실행될 때 단 한 번만 실행됨  
게임을 시작한 후에 화면 방향을 조정하면 아래에 있는 코드는 재실행되지 않는다.  
사용중에 방향 전환을 하는 경우는 사용하지 않는 게 나을 수 있음  
- 반응형 코드를 작성하는 것이 좋다  
- 기기의 방향이나 크기가 변결될 때 반응해야 하는 코드는 여러번 실행되기 때문에 모두 컴포넌트 함수로 가야 함  
- `useWindowDimensions` : recat-native에서 제공하는 기기의 크기나 치수가 바뀔 때 반응할 수 있는 Hook  
  
### KeyboardAvoidingView를 통해 화면 콘텐츠 관리하기  
- 가로화면에서 iOS 키보드가 닫히지 않는 문제 -> import `KeyboardAvoidingView`  
- 키보드가 열릴 때마다 입력 요소 및 다른 요소가 화면 위로 올라가 키보드가 열려도 액세스 할 수 있다.   
- behavior prop을 설정해줘야 작동함  
- 효과적으로 이동시키기 위해 스크롤이 가능한 컨테이너 안에 넣기 -> KeyboardAvoidingView를 ScrollView로 감싼다.  
  
  
### 가로 모드 UI 개선하기  
- 디자인이 다른 가로형 UI를 따로 작성하기   
  
### Platform API를 통해 특정 플랫폼용 코드 작성하기  
- react native에서 제공, import하기  
- 삼항식 사용법 : `borderWidth: Platform.OS === 'android' ? 2 : 0` : android면 borderWith를 2, 아니면 0  
- select 메서드 사용 :  `borderWidth: Platform.select({.ios: 0, android: 2 })`  
- 플랫폼별 파일 작성하기 : import 패스는 플랫폼명 포함하지 않음 (ex : ../components/ui/Title)  
  
### 상태 바 스타일링하기  
`import {StatusBar} from 'expo-status-bar'`  
  
**컴포넌트가 2개 이상일 때 `fragment`로 감싸는 이유**  
항상 단일한 루트 엘리먼트를 반환해야 하기 때문  
불필요한 DOM요소를 추가하지 않고 여러 컴포넌트를 그룹화할 수 있다.  
불필요한 DOM 요소가 많아지면 브라우저가 DOM 트리를 더 많이 관리해야 하므로, 성능에 영향을 줄 수 있다. 또한, 스타일링과 레이아웃 관리 측면에서 복잡도가 증가할 수 있다. 따라서 가능하면 불필요한 DOM 요소 생성을 피하는 것이 좋다.  
  
<video controls width="600">      
  <source src="/NextGenWebDev/assets/img/20240813_01.mp4" type="video/mp4">      
  Your browser does not support the video tag.      
</video>   