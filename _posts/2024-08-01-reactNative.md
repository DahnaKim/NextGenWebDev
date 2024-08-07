---  
layout: post  
title:  "Setup"  
categories: [ React Native ]  
image: assets/img/20240801_03.png  
tags: [featured]  
---  
  
## React Native란  
  
리액트 네이티브(React Native)는 Facebook에서 개발한 오픈 소스 프레임워크로, 자바스크립트를 사용하여 모바일 애플리케이션을 개발할 수 있도록 해줍니다. 리액트 네이티브는 웹 개발에서 사용되는 리액트(React) 라이브러리를 기반으로 하며, iOS와 Android와 같은 다양한 모바일 플랫폼에서 동작하는 네이티브 애플리케이션을 만들 수 있습니다.  
리액트 네이티브의 핵심은 "한 번 코딩하면, 어디서든 실행 가능하다"는 원칙을 따릅니다. 즉, 하나의 코드베이스로 두 가지(혹은 그 이상의) 플랫폼에서 동작하는 애플리케이션을 개발할 수 있습니다.  
리액트 네이티브는 다음과 같은 특징을 가지고 있습니다:  
1. 컴포넌트 기반 개발: 리액트와 마찬가지로 리액트 네이티브는 컴포넌트 기반으로 개발합니다. 화면의 각 요소는 하나의 컴포넌트로 정의되며, 이 컴포넌트들은 독립적으로 관리될 수 있습니다.  
2. 네이티브 컴포넌트 사용: 리액트 네이티브는 View, Text, Button 등과 같은 네이티브 컴포넌트를 제공합니다. 이러한 컴포넌트는 각각의 플랫폼에 맞는 네이티브 UI 요소로 변환되어 실행됩니다.  
3. JavaScript로 로직 처리: 앱의 논리적인 부분은 JavaScript로 작성됩니다. 이 JavaScript 코드는 네이티브 환경에서 실행되며, UI와 상호작용합니다.  
4. 핫 리로딩(Hot Reloading): 개발 중에 코드 변경 사항이 즉시 반영되어 빠른 피드백 루프를 제공하는 기능입니다. 이는 개발 생산성을 크게 향상시킵니다.  
5. 광범위한 커뮤니티와 생태계: 리액트 네이티브는 매우 활발한 커뮤니티와 생태계를 가지고 있어, 다양한 라이브러리와 툴을 쉽게 활용할 수 있습니다.  
이러한 특징 덕분에, 리액트 네이티브는 웹 개발자들이 비교적 쉽게 모바일 애플리케이션 개발에 도전할 수 있도록 도와주며, 동시에 네이티브 앱과 유사한 성능과 사용자 경험을 제공합니다.  
   
![1]({{ site.baseurl }}/assets/img/20240801_01.png)  
리액트 네이티브는 리액트와 리액트 네이티브 앱 코드를 작성하면, 해당 코드를 실제 네이티브 앱으로 컴파일합니다. 예를 들어, "const App = props => { ... }" 형태의 코드를 작성하면, 이것이 네이티브 코드로 변환되어 실제로 네이티브 앱에서 실행됩니다. 리액트 네이티브는 리액트의 구조를 사용하지만, HTML 대신 네이티브 컴포넌트를 사용하여 iOS와 Android에서 모두 동작하는 앱을 개발할 수 있도록 돕습니다.  
  
![2]({{ site.baseurl }}/assets/img/20240801_02.png)  
UI 요소는 리액트 네이티브에서 제공하는 컴포넌트를 통해 작성되며, 이들은 네이티브 뷰로 컴파일됩니다. 반면에, 논리는 JavaScript로 작성된 코드로, 리액트 네이티브에 의해 호스팅되는 JavaScript 스레드에서 실행됩니다. 이러한 논리는 네이티브 코드로 컴파일되지 않고, JavaScript로 유지되어, 실제 네이티브 앱 환경에서 UI와 상호작용하게 됩니다.  
  
이러한 방식으로 리액트 네이티브는 한 번의 코드 작성으로 여러 플랫폼(iOS, Android)에서 동작하는 앱을 개발할 수 있도록 해줍니다. 네이티브 UI와 JavaScript 논리 사이의 원활한 상호작용을 통해, 개발자는 네이티브 앱의 성능을 유지하면서도 웹 개발의 편리함을 활용할 수 있습니다.  

<br>
  
## Creating a New React Native App  
**CLI** : Command Line Interface 명령 행 인터페이스  
- Expo CLI(Expo)  : Third-party service (free!) 추가적인 유료 서비스를 제공하지만 React Native 앱을 구축하고 출시하기 위해서 비용을 지불할 필요가 없다.  
Managed app development 프로젝트 생성이 수월하고 코드 작성이 비교적 쉬우며 네이티브 기기의 카메라 등 기능을 활용하는 것이 전반적으로 쉬워짐  
가장 좋은 점은 필요하다면 언제든지 Expo 방식을 중지해도 됨  
  
  
- React Native CLI   
Expo가 나타나기 전 React Native 팀과 관련 커뮤니티가 제공한 툴  
편리한 기능도 적고 카메라 등 특정 네이티브 기기 기능을 활용할 때 번거로움  
장점 : Java, Objective-C Swift, Kotlin과 같은 네이티브 소스 코드와 통합하기가 비교적 쉬움  
하지만 애초에 React Native를 사용하는 이유가 이 과정을 생략하기 위해서라 그닥 장점이라고 할 순 없음  

<br>
  
## React Native 프로젝트 생성  
- node.js(LTS) 설치  
- sudo npm install -g expo-cli (macOS의 경우 sudo 붙이기)  
- 설치 후 expo를 쳤을 때 에러 안나오면 정상  
- `npx create-expo-app 프로젝트명` : 새 앱 생성  
  
🛠️ **error fix**  
![4]({{ site.baseurl }}/assets/img/20240801_04.png)  
- npm 캐시에서 파일이 겹치거나, 파일 시스템에 권한이 부족할 때 발생  
- `sudo chown -R 501:20 "/Users/dana/.npm"` : /Users/dana/.npm 디렉터리와 그 하위 파일 및 디렉터리의 소유권을 현재 사용자(UID 501)와 현재 사용자의 기본 그룹(GID 20)으로 변경  
- `npm cache clean --force` : 캐시정리   
- 프로젝트 재생성  

<br>
  
## 생성된 프로젝트 분석  
- package.json의 "react-native-web" : React Native로 웹 앱을 구축할 수는 있지만 잘 지원되지 않음  
- babel.config.js : 코드가 내부적으로 어떻게 변환되는지 설정. 익숙하지 않으면 바꾸지 않는 것이 좋음  
- app.json : React Native 앱의 설정과 실행 방식을 구성  
앱의 미리보기나 실제 앱스토어를 위해 구축될 때 Expo에서 이어서 사용할 파일  
이름이나 배경색을 설정할 수 있다. 설정을 위해 자주 찾게 될 파일  

<br>
  
## 미리보기   
- Expo Go 앱 설치  
- `npx expo start`  

<br>
  
🛠️ **error fix**  
![5]({{ site.baseurl }}/assets/img/20240801_05.png)  
- EMFILE: too many open files, watch 에러는 시스템에서 동시에 열 수 있는 파일의 개수 제한을 초과했을 때 발생하는 오류  
- 해결 방법   
`brew install watchman` : watchman 설치   
  
- Android폰이라 iOS환경을 볼 수 없는 경우 xcode 다운로드(Window나 Linux에서는 지원하지 않음) 강의 1-10 참고  
- Android 미리보기 - Android Studio 설치  
More Actions 옵션 -> Virtual Device Manager -> 애뮬레이터 구축  
![6]({{ site.baseurl }}/assets/img/20240801_06.png)  
  
  