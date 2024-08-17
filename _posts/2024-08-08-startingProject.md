---  
layout: post  
title:  "Deep Dive Real App"  
categories: [ React Native ]  
image: assets/img/20240808_01.png  
tags: [featured]  
---  
  
# Deep Dive Real App  
  
**그림자 설정**  
- React Native에는 boxShadow가 없음  
- Android : elevation  
- iOS : shadowColor, shadowOffset, shadowOpacity, shadowRadius  
<br>

**키보드 제어**  
- keyboardType프로퍼티 사용  
- number-pad : 숫자만 뜸  
<br>  

**커스텀 버튼**  
- 구축할 때 보통 결합한 컨테이너를 이용함  
- iOS 화살표 함수 사용, 배열을 전달해 여러 스타일 적용 가능  
<br>  

**그라데이션**  
- Expo활용  
- `npx expo install expo-linear-gradient`  
<br>  

**배경이미지**  
- React Native에 내장된 ImageBackground 컴포넌트 활용  
<br>  
**입력값 유효성 검사**  
- `parseInt` : 문자열에서 숫자로 변환  
<br> 

**경고창**  
`Alert.alert()` : React Native에서 Alert API를 불러옴  
- alert 메서드 인수( 제목, 메시지, 경고창 버튼 )  
<br>  

**화면 전환하기**  
- `let 변수명` : 헬퍼 변수 추가  
<br>  

**SafeAreaView**  
 - React Native에서 iOS와 Android의 특정 기기들에서 안전한 콘텐츠 영역을 확보하기 위해 사용하는 컴포넌트  
<br>  

**색상을 글로벌 영역에서 관리하기**  
- React Native에서는 자주 사용하는 색상과 같은 특정 상숫값을 보여주는 헬퍼 파일을 만듦  
<br>  

**무작위 수 생성**  
- `Math.floor` : 소수자리를 없애줌     
<br>  

**계단식 스타일**  
- 프로퍼티를 사용 : 적용하고 싶은 스타일이 이미 내부에 적용된 스타일 외에 추가 사항일 때  
컴포넌트 내부에 설정한 스타일과 합칠 수 있다.  
- style 프로퍼티는 스타일 객체뿐만 아니라 스타일 객체의 배열도 취하기 때문  
- 항목은 왼쪽에서 오른쪽으로 파싱되고 평가 됨  
<br>


**아이콘**  
- Expo는 아이콘을 사용할 수 있는 라이브러리 및 패키지를 포함하고 있음  
<br>  
  
**커스텀 글꼴**  
- `expo install expo-font` : 별도의 패키지를 설치  
- 글꼴을 로딩하기 위해 `useFont`사용  
- ❌(더이상 사용되지 않음)`expo install expo-app-loading` :  글꼴이 초기 설정되기 전에 나오는 앱의 로딩화면.  
렌더링할 수 있는 유틸리티 컴포넌트가 나와서 스플래시 화면을 연장하고 특정 조건을 충족하기 전까지 스플래시 화면이 나타나도록 한다.  
✅ `expo install expo-splash-screen` : expo-splash-screen에서 제공하는 SplashScreen.preventAutoHideAsync()와 SplashScreen.hideAsync() 메서드를 사용해서 스플래시 화면을 제어해야 함  
  
<video controls width="600">      
  <source src="/NextGenWebDev/assets/img/20240809_01.mp4" type="video/mp4">      
  Your browser does not support the video tag.      
</video>   