---  
layout: post  
title:  "Publishing React Native Apps"  
categories: [ React Native ]  
image: assets/img/20240911_01.png  
tags: [featured]  
---  
  
# Publishing React Native Apps  
  
- Expo로 관리된 앱과 Expo를 사용하지 않는 앱 구분  
- Expo를 사용하면 Expo가 제공하는 서버로 빌드 프로세스를 아웃소싱할 수 있음  
- Expo클라우드 서비스를 사용해서 iOS, Android 각각의 환경에 맞게 빌드  
  
- Expo를 사용하지 않을 경우 : 앱 바이너리를 로컬에서 빌드(내 기기에 직접 빌드)해서 각 앱스토어에 직접 제출  
앱 바이너리 : 앱의 실행 파일. 개발자가 작성한 소스 코드가 컴파일되어 기기에서 실행 가능한 형태로 변환된 결과물  
  
### Configuring for Production  
- 가장 중요한 것 : 권한  
- 앱스토어에서 사용될 앱 이름, 고유 식별자 설정  
- 앱 버전 지정  
- 환경 변수 시크릿(Expo가 관리)  
- 앱 아이콘, 스플래시 화면   
- 🌟공식문서 자세히 읽어보기🌟  
![1]({{ site.baseurl }}/assets/img/20240911_02.png)   
  
**환경변수와 시크릿(secrets)**  
- 환경변수 : 글로벌 변수. 앱을 구축하는 시점에서 설정되거나 개발 단계부터 설정될 수 있으며 코드 내 여러 부분에서 사용됨  
- 시크릿(secrets) : 코드베이스 중 노출되면 안 되는 특정한 값을 말한다.  
- [문서참조](https://docs.expo.dev/build-reference/variables/)  
  
### 아이콘 & 스플래시 화면 추가하기  
- [문서참조](https://docs.expo.dev/develop/user-interface/splash-screen-and-app-icon/)  
![2]({{ site.baseurl }}/assets/img/20240911_03.png)   
  
### EAS를 통해 Expo 앱 구축하기  
![3]({{ site.baseurl }}/assets/img/20240911_04.png)   
- `sudo npm install -g eas-cli` : Install the latest EAS CLI  
EAS로 구축하려는 프로젝트와 빌드를 조직하고 관리하는 데 사용되는 추가 도구가 설치됨  
- `eas login`  
- `eas build:configure` : Configure the project  
- 빌드 실행 :   
앱스토어로 보내기 전에 시뮬레이터나 실제 기기에 설치할 수 없는 앱스토어에 최적화된 빌드와 시뮬레이터나 실제 기기에 설치할 수 있는 빌드가 있음  
https://docs.expo.dev/build-reference/apk/  
![4]({{ site.baseurl }}/assets/img/20240911_05.png)   
- `eas build -p android --profile preview` : 변경한 설정 적용  
- Keystore : 앱에 서명할 때 필요하며 앱을 보호할 뿐만 아니라 앱을 퍼블리싱하고 업데이트할 권한이 있는 유일한 당사자인지 확인  
`Generate a new Android Keystore? … yes선택` -> Expo가 EAS서버에서 서명 키스토어를 관리하도록 함  
- 빌드완료 후 APK 파일을 다운로드할 수 있음  
- 이 apk는 시뮬레이터에서 테스트하기엔 적합하지만 Google Play 스토어에는 최적화되지 않았음  
- Android에 조금 더 최적화된 유형인 production 프로필을 사용하면 Google Play 스토어에 업로드할 수 있는 바이너리 파일인 aab 파일을 얻을 수 있다.  
- eas build 명령어를 실행할 때, 명시적으로 빌드 프로필을 지정하지 않으면 기본적으로 production 프로필을 사용함  
- AAB 파일(Android App Bundle): 안드로이드 앱을 배포할 때 사용하는 파일 형식.aab 파일은 구글 플레이 스토어에 앱을 최적화하여 제출하기 위한 파일이다. 이 파일은 기기별로 필요한 리소스만 선택하여 다운로드할 수 있게 하여 앱 크기를 줄여주는 역할을 한다.  
production 프로필을 사용하면, 앱스토어 제출에 최적화된 빌드 설정을 자동으로 적용하여 AAB 파일이 생성된다. 이를 통해 구글 플레이 스토어에 바로 제출할 수 있는 빌드가 만들어진다.  
- 앱을 수동으로 업로드하는 대신 EAS Submit을 사용해서 제출할 수 있다.  
(https://docs.expo.dev/submit/introduction/)  
  
### iOS용 EAS (배포시 강의 239 참고)  
- 관련 설정 추가 : 시뮬레이터에 설치할 수 있는 빌드 생성  
![5]({{ site.baseurl }}/assets/img/20240911_06.png)   
강의와 달라서 진행시 문서 참고하기  
(https://docs.expo.dev/build-reference/simulators/)  
빌드 후 링크에서 .app파일을 다운로드해서 시뮬레이터에서 실행 (Mac 시뮬레이터에서만 작동됨)  
- 배포하려면 Apple 개발자 프로그램 멤버십이 있어야 함  
- Google Play 스토어에 앱을 퍼블리싱 하려면 $25를 내고 개발자 계정을 만들어야 함  
![6]({{ site.baseurl }}/assets/img/20240911_07.png)   
- `eas build --platform ios` : 빌드 명령어 넣기  
- 프로필과 인증서를 수동으로 설정하기 https://docs.expo.dev/app-signing/local-credentials/#android-credentials  
  
- 런치 화면(Launch Screen)과 스플래시 화면의 차이  
런치 화면은 앱이 로드될 때 자동으로 표시되며, 실제 앱이 준비되면 사라진다.  
스플래시 화면은 추가적인 마케팅 요소로서 사용되며, 앱이 로드된 후에도 일정 시간 동안 강제로 표시할 수 있는 화면  
  
### Expo 없이 iOS, Android 구축하기 (강의 240, 241)  