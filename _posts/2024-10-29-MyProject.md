---  
layout: post  
title:  "Reference Note"  
categories: [ React Native ]  
image: assets/img/20241029_01.png  
tags: [featured]  
---  
  
# reference note  
  
## 리렌더링에 값 유지하기  
- 일반변수(const, let) : 컴포넌트가 리렌더링될 때마다 새로운 값으로 초기화  
- useState : 리렌더링이 발생해도 상태 값이 유지되지만, 값이 변경되면 컴포넌트가 다시 리렌더링  
(예시 : 사용자가 폼에 입력한 값이 수정할 때 지워지지 않고 남아있다)  
- useRef : 리렌더링 시에도 값이 유지되며, 값이 변경되더라도 리렌더링을 유발하지 않는다.  
(예시 : 앱에 타이머가 존재하고 앱의 렌더링과 상관없이 계속 시간을 측정하는 경우)  
  
## 콜백(callback)  
- 함수나 작업이 끝난 후 결과에 따라 나중에 호출되는 함수  
- 즉, 어떤 일이 완료되면 그때 호출될 함수  
- 예시 : 예약 작업이 끝나면 확인 문자(콜백)가 전송  
  
**useEffect** 자체는 비동기 함수를 직접 받을 수 없기 때문에, 비동기 작업을 위한 함수를 내부에서 정의한 후 호출하는 방식으로 처리  
  
  
## util, hook, component로 나눌 때  

### util  
- 특정한 기능을 수행하는 작은 단위의 힘수들  
- 예 : 날짜 포맷 변환, 배열이나 객체에서 특정 데이터를 추출할 때  
- 데이터 처리나 변환에 관한 기능이 자주 반복될 때  
- 단순하고 독립적인 작업이라 UI나 상태와 상관없이 어디서든 사용이 가능할 때  
  
### Hook  
- 상태 관리와 효과를 다루는 특정 기능을 가진 함수  
- 상태를 변화시키는 로직이나 API 호출 같은 비동이 작업이 들어가야 할 때  
  
### Component  
- 화면에서 UI를 구성하는 요소로, 사용자에게 어떻게 보여줄지를 담당  
- 버튼, 입력필드, 모달, 특정 화면 기능(사진 추가 기능 등) UI와 연관된 코드를 컴포넌트로 나눔  
- 특정 UI가 반복되거나 독립적으로 동작해야 하는 경우 분리  
  
  
- 달력 추가  
npm install react-native-calendars  
  
- 사진첩 접근  
npx expo install expo-image-picker  
  
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
  
- 내비게이션  
npm install @react-navigation/native  
  
- Expo 최신버전 업데이트  
npx expo install expo@latest  
  
**사진첩 테스트**  
- 이미지 폴더에 넣기  
Android Studio Device File Explorer(안드로이드 스튜디오 디바이스 파일 익스플로러) : View > Tool Windows > Device Explorer  
/sdcard/Pictures/ 이동  
  
- 애뮬레이터 사진앱 이미지 인식이 안될 때  
1. 터미널에서 /Users/<YourUserName>/Library/Android/sdk/platform-tools로 들어가기  
2. nano ~/.zshrc 파일 열기  
3. 마지막 줄에 추가 `export PATH=$PATH:/Users/<YourUserName>/Library/Android/sdk/platform-tools`  
4. Ctrl + O > Enter > Ctrl + X 저장 후 에디터 종료  
5. 환경 변수 적용 `source ~/.zshrc`  
6. 개별 파일 대상 미디어 스캔   
`adb shell am broadcast -a android.intent.action.MEDIA_SCANNER_SCAN_FILE -d file:///sdcard/Pictures/your_image.png`  
![1]({{ site.baseurl }}/assets/img/20241001_01.png)  
  
- 갤러리 앱이 이미지 파일을 인식하려면, 파일이 Android 시스템의 미디어 데이터베이스에 등록되어야 한다.  
새로운 파일을 미디어 데이터베이스에 등록하기 위해 미디어 스캐너라는 프로세스가 필요.  
일반적으로 새 파일이 추가될 때, Android 시스템은 자동으로 미디어 스캐너를 실행하여 새 파일을 데이터베이스에 등록한다. 그러나 에뮬레이터에서는 수동으로 파일을 추가할 때 이 자동 프로세스가 제대로 동작하지 않을 수 있다.  
그래서 명령어를 사용하여 특정 파일을 강제로 스캔하여 미디어 데이터베이스에 등록하도록 해야 한다.  
  
- 화면 간 이동  
npx expo install react-native-screens react-native-safe-area-context  
npm install @react-navigation/stack  
  
  
**ECMAScript 모듈 형식(ESM)**과 **CommonJS 모듈 형식(CJS)**  
자바스크립트에서 모듈을 정의하고 가져오는 두 가지 방식   
이 두 방식은 자바스크립트 모듈을 파일 간에 효율적으로 공유하고 조직화하는 방법을 제공하는데, 각기 다른 환경과 상황에서 사용된다.  
  
1. ECMAScript 모듈 형식 (ESM)  
ECMAScript 모듈 형식은 **ES6 (ECMAScript 2015)**부터 도입된 자바스크립트의 표준 모듈 시스템  
ESM은 자바스크립트의 공식적인 모듈화 방법으로서 브라우저와 최신 자바스크립트 환경에서 사용된다.  
import와 export 키워드를 사용  
동작 방식: 정적으로 분석되어, 실행 전에 모듈의 종속성을 파악할 수 있다.   
따라서 ESM은 **트리 쉐이킹(Tree Shaking)**과 같은 최적화 기법을 지원한다.  
파일 확장자: .mjs 또는 .js로 사용할 수 있다.  
type 설정: package.json에서 "type": "module"을 설정하면 .js 파일도 ESM으로 해석된다.  
2. CommonJS 모듈 형식 (CJS)  
CommonJS는 Node.js가 자바스크립트에 모듈 시스템을 도입하면서 만들어진 형식으로, 현재까지도 Node.js에서 널리 사용되고 있다.  
require와 module.exports 키워드를 사용  
동작 방식: 동적으로 모듈을 불러오는 방식이기 때문에 실행 시점에서 모듈을 로드  
파일 확장자: 일반적으로 .js를 사용  
Node.js 환경: CommonJS는 기본적으로 Node.js 환경에서 사용되며, 서버 사이드 자바스크립트에서 주로 사용됨  
  
3. 혼합 사용 문제  
ESM과 CJS는 서로 다른 모듈 형식이기 때문에, 혼합해서 사용하는 경우 충돌이 발생할 수 있다.   
특히, Babel이나 Webpack 같은 번들러가 두 모듈 형식을 처리하는 방식에 따라 문제가 발생할 수 있다.  
Node.js 최신 버전에서는 ESM을 지원하지만, 프로젝트 내에서 type이 설정된 상태와 그렇지 않은 상태의 혼용 또는 번들링 환경에 따라 ESM 모듈을 제대로 로드하지 못하는 경우가 발생할 수 있다.  
  
4. 변환 방법  
CJS → ESM 변환: 기존의 CommonJS 모듈을 ESM 형식으로 변환하려면 module.exports를 export로 변경하고, require를 import로 변경  
5. ESM과 CJS 사용 시 주의 사항  
프로젝트에 package.json에서 "type": "module"을 지정하면 .js 파일은 ESM 형식으로 처리되고, 지정하지 않으면 CommonJS로 처리된다.  
두 모듈 형식을 함께 사용하려면 Babel이나 Webpack 같은 번들러를 통해 중간 변환 작업을 해야 하는 경우가 많다.  
`React Native와 Expo에서는 기본적으로 CommonJS 형식을 권장`하며, 모듈 형식을 혼합할 때 문제가 생길 수 있으므로 가급적 한 가지 형식만 사용하는 것이 좋다.  
  
  
- 데이터 로컬에 저장(SQLite사용)  
npx expo install expo-sqlite  
npx expo install expo-file-system expo-asset  
(expo-file-system: SQLite 데이터베이스 파일을 백업하거나 접근할 필요가 있을 때 유용하다.  또한, 데이터베이스 외에도 파일로 데이터를 저장하거나 읽고 쓸 때 사용할 수 있다.  
expo-asset: 이미지나 미디어 파일을 효율적으로 관리하고 저장할 때 필요할 수 있다.)  
  
- expo install lottie-react-native  
  
스티커 기능  
- npx expo install react-native-gesture-handler react-native-reanimated  
- babel.config.js의 return에 plugins: ['react-native-reanimated/plugin'], // reanimated 플러그인 추가  
  
  
- 디렉토리 구조 출력하기  
`brew install tree` : 설치  
`tree -a -I 'node_modules|.git'` : 불필요한 폴더나 파일 제외  
  
  
- 스플래시 화면 추가  
1. Expo 프로젝트에서는 app.json 파일을 통해 스플래시 화면을 설정할 수 있다.  
2. Lottie 애니메이션을 넣고 싶은 경우 index.tsx에서 SplashScreen과 Lottie를 커스터마이즈 하면 됨  
  
- `?.`(옵셔널 체이닝 연산자)  
객체가 null 또는 undefined일 수 있을 때 안전하게 접근할 수 있도록 해줌  
  
- `||`(논리 or 연산자)  
앞의 값이 null 또는 undefined일 경우, 뒤에 있는 값을 기본값으로 반환  


- `setTimeout`은 JavaScript에서 특정 코드를 일정 시간이 지난 후에 실행하도록 예약하는 함수  
기본문법 `setTimeout(callbackFunction, delay);`  
작동방식 : JavaScript는 이벤트 루프라는 메커니즘을 통해 setTimeout 같은 비동기 작업을 관리한다. setTimeout은 설정된 시간이 지나면 실행할 함수를 이벤트 큐에 추가하고, JavaScript가 현재 실행 중인 코드가 끝나면 큐에 있는 함수들을 순서대로 실행한다.  
따라서, setTimeout을 사용하면 코드 실행을 잠시 미루는 효과를 얻을 수 있다.  

- `useSharedValue`는 Reanimated 라이브러리에서 제공하는 기능으로, 애니메이션 값의 변경을 리액트 컴포넌트의 리렌더링 없이 효율적으로 관리할 수 있다

  
  
  
  
  