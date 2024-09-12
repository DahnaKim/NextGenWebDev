---  
layout: post  
title:  "React Native Without Expo"  
categories: [ React Native ]  
image: assets/img/20240909_04.png  
tags: [featured]  
---  
  
# React Native Without Expo  
  
### Expo  
- 애뮬레이터에 설치해서 개발 과정을 미리 볼 수 있는 장점이 있음  
- 하지만 실제 실행 가능한 앱이 아님  
**독립형 앱을 개발하고 퍼블리싱 하는 방법도 있음**  
  
`Expo "Managed Workflow"` : Expo Go사용, 앱에 내장된 요소가 많아서 네이티브 기기 기능 등 특정 기능 작업이 쉽다. 서드 파티 패키지를 사용할 때 따로 구성하지 않아도 됨.  
크로스 플랫폼 독립형 앱 구축이 가능함  
`Expo "Bare Workflow"`: 바로 사용 가능한 구정을 줄이고 더 많이 제어하려고 할 때 사용  
예를 들어 Swift나 Objective-C Android, Kotlin 등에서 네이티브 코드를 작성한 후 React Native코드와 섞어야 할 경우 베어 워크플로우가 유용함. Expo 기능이 더 적음  
역시 크로스 플랫폼 독립형 앱을 구축하고 Expo가 제공하는 내장 서비스를 사용할 수 있다.  
`React Native CLI` : 명령 줄 인터페이스  
모든 작업을 스스로 해야해서 설정이 복잡해질 수 있음  
서드 파티 패키지를 추가해서 네이티브 기기 기능을 이용하려면 구성할 게 많아서 번거로울 수 있다.  
제어 범위가 넓어진다는 이점이 있지만 손이 많이 감  
서드 파티 제공자의 클라우드 빌드를 통해 여러 플랫폼을 위한 앱을 구축할 수 있지만 Expo와 달리 내장된 기능은 아니라서 설정하기 쉽지 않음. 로컬 환경에서 앱을 직접 구축해야 한다.  
  
  
### Bare Workflow  
- 터미널에 `expo init`  
- app name 설정  
- minimal 선택  
- VS Code를 열면 이전과 다르게 ios와 android폴더가 있음  
- ios의 Info.plist, android의 AndroidManifest.xml : 앱을 구성할 때 필요한 파일   
  
**시뮬레이터 열기**  
- android device연결 후 터미널에 ANDROID_HOME 환경 변수 설정  
`export ANDROID_HOME=/Users/사용자명/Library/Android/sdk`  
`exportPATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools`  
- `npm run android` 실행  
![1]({{ site.baseurl }}/assets/img/20240909_01.png)  
  
- Xcode에서 /Users/dana/Desktop/React_Native/bare-expo/ios/bareexpo.xcworkspace 파일을 열고. 업데이트 후 `npm run ios`  
![2]({{ site.baseurl }}/assets/img/20240909_02.png)  
  
### Bare Workflow를 통해 네이티브 기기 기능 사용하기  
- Expo 패키지를 모두 사용할 수 있지만 추가 설정이 필요하다.  
- Expo location을 사용할 경우 공식 문서를 참조해서 추가적인 설정을 해주면 됨  
![3]({{ site.baseurl }}/assets/img/20240909_03.png)  
![4]({{ site.baseurl }}/assets/img/20240909_04.png)  
  
### 베어 워크플로우로 전환하기  
- Managed workflow에서 네이티브 코드를 추가하는 등 더 다양한 구성을 취하고자 할 경우 언제든 베어 워크플로우로 전환할 수 있다.  
- `expo eject`  
- 리버스 URL 형태인 식별자로 Android, ios 패키지 이름 작성 (ex : com.academind.rncourse)  
- 위에서 설정했던 **시뮬레이터 열기**부터 똑같이 설정해주면 됨  
  
### React Native CLI를 통해 프로젝트 생성하기  
- `npm uninstall -g react-native-cli @react-native-community/cli` 이전 패키지 정리 후  
- `npx @react-native-community/cli@latest init 프로젝트명` 프로젝트 생성  
- `npm start`  
- **시뮬레이터 열기**과정으로 시뮬레이터 열기  
- ✅ 작업중 npm start 터미널 열어놔야 함  
  
### Expo를 사용하지 않는 앱 & 네이티브 기기 기능  
- React Native 프로젝트를 베어 워크플로우로 바꾸면 Expo패키지들을 다시 사용할 수 있음  
- 혹은 공식문서 참조해서 설치하고 진행하면 됨  
  
  