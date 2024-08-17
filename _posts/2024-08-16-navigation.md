---  
layout: post  
title:  "Navigation"  
categories: [ React Native ]  
image: assets/img/20240815_01.png  
tags: [featured]  
---  
  
  
# React Native Apps & Navigation  
  
## 내비게이션 방식  
**웹 브라우저** :  
웹 브라우저에서는 주소(URL)을 통해 페이지를 이동   
사용자는 브라우저의 뒤로 가기 버튼을 눌러 이전 페이지로 돌아갈 수 있으며, 브라우저의 주소창에 새로운 URL을 입력하거나 링크를 클릭하면 다른 페이지로 이동  
URL이 페이지의 주요 상태를 관리하며, 브라우저의 히스토리 스택에 따라 내비게이션이 관리된다.  
  
**React Native** :  
React Native에서는 내비게이션이 기본적으로 JavaScript 내에서 관리된다.   
화면 간 전환은 Stack, Tab, Drawer 등의 내비게이션 컨테이너를 사용하여 이루어짐  
내비게이션 상태는 앱의 내부에서 관리되며, 화면 전환 시 주소가 변경되지 않는다.   
대신, 내비게이션 라이브러리가 내비게이션 스택을 관리  

<br>
  
###  UI 및 사용자 경험  
**웹 브라우저** :  
페이지 전환 시 새로고침이 발생하며, 이는 종종 화면이 깜빡이는 경험을 유발할 수 있다.  
페이지 로딩 속도나 네트워크 상태에 따라 내비게이션 속도가 달라질 수 있음  
**React Native** :  
내비게이션은 네이티브 애니메이션을 사용하여 부드럽고 빠르게 이루어진다.  
대부분의 화면 전환은 앱 내에서 즉각적으로 이루어지며, 사용자 경험이 더 원활하다.  
  
### 내비게이션 상태 관리  
**웹 브라우저** :  
브라우저는 자동으로 히스토리를 관리하여 사용자가 뒤로 가기, 앞으로 가기 버튼을 통해 이전 페이지로 이동할 수 있게 한다.  
URL 파라미터나 쿼리 스트링을 통해 상태를 관리  
**React Native** :  
내비게이션 상태는 주로 React Navigation과 같은 라이브러리로 관리된다.   
상태 관리를 위해 컨텍스트나 리덕스 같은 상태 관리 도구와 함께 사용되기도 함  
화면 전환과 상태 관리를 위한 데이터는 주로 프로퍼티(props)로 전달된다.  
  
### React Nvigation  
- `npm install @react-navigation/native` 설치  
- 라우팅 및 내비게이션을 추가하기 위해 만든 패키지  
- 추가 의존성 설치   
`expo install react-native-screens react-native-safe-area-context`  
- 네이티브 스택 내비게이션을 구현하기 위한 라이브러리  
`expo install @react-navigation/native-stack`  
**주요 기능**  
스택 네비게이션: 화면을 스택 방식으로 관리할 수 있다. 새로운 화면을 푸시(push)하거나 이전 화면으로 팝(pop)할 수 있음  
네이티브 애니메이션: 화면 전환 시 네이티브 애니메이션을 사용하여 부드럽고 자연스러운 전환 효과를 제공  
제스처 지원: 스와이프 제스처로 이전 화면으로 돌아가는 기능을 지원  
헤더 구성: 각 화면마다 커스텀 헤더를 설정하거나 공통 헤더를 정의할 수 있음  
  
### 기본 화면 설정하기  
`<Stack.Navigator>`을 통해 Navigator를 설정하고   
`<Stack.Screen>`을 통해 화면을 등록할 때는 앱이 시작할 때 어떤 화면이 기본으로 표시될지를 설정할 수 있다.  
아무런 설정 없이는 가장 위에 있는 화면, 즉 `<Stack.Navigator>`내의 첫 번째 자식 요소가 초기 화면이 됨  
`<Stack.Screen>`순서를 변경함으로써 초기 화면을 변경할 수 있음  
  
### useNavigation  
- Stack : 화면 위에 다른 페이지를 푸시하고 이전 페이지로 돌아갈 수 있다.  
- Native Stack : 네이티브 플랫폼 요소를 사용해서 네이티브 동작을 흉내 내는 스택보다 성능이 더 높을 수 있다. 보통 네이티브 스택을 선호  
하지만 네이티브 스택을 사용하는 데 문제가 있으면 스택 기반 내비게이션을 제공하는 스택으로 폴백할 수 있음  
- useNavigation훅을 사용하면 Screen으로 등록됐는지와 관계없이 모든 컴포넌트 함수에서 이 훅을 사용할 수 있고 navigation 객체를 제공한다  
- Screen으로 등록되지 않은 컴포넌트 내에서 내비게이션을 해야 할 경우가 있을 때 대안  
  
### 화면 간 데이터 전달을 위해 라우트 매개변수로 작업하기  
- navigation프로퍼티의 navigate 메서드를 사용 : 화면 사이 데이터를 전달. 이동하고자 하는 화면의 이름을 필수 매개변수 값으로 가짐  
선택 사항으로 두 번째 매개변수의 값에 params를 정의할 수 있음(로딩될 화면에 전달해야 하는 매개변수)  
- Screen으로 등록된 컴포넌트는 navigation과 더불어 route프로퍼티를 받으며,   
route프로퍼티에는 params프로퍼티가 있음  
- useRoute Hook : route prop대신 사용할 수 있는 훅  
Screen에 등록되지 않음 중첩 컴포넌트에서 현재 로딩된 정보에 접근할 경우 route 대신 useRoute 훅을 사용한다.  
  
### 이미지 추가, 스타일링  
- 웹 이미지를 사용할 때는 가로세로 길이를 정의하는 스타일을 추가해야 함  
  
### 화면 헤더 & 배경 스타일링  
- React Native는 name을 기본 헤더 제목으로 사용하지만 `options`prop을 사용해 다르게 설정할 수 있음  
- 여러 옵션이 있음 문서 참고  
- `screenOptions` : 모든 화면에 적용할 옵션을 내비게이터에 추가  
  
### 내비게이션 옵션 동적으로 설정하기  
- options prop을 사용해 함수를 전달하는 방법 : 두 개의 데이터가 있는 객체를 받게 됨(route, navigation)  
- 컴포넌트 내부에 옵션을 설정하는 방법 : navigation prop사용  
setOptions 메서드로 객체를 옵션으로, 이전에 본 모든 항목과 프로퍼티를 취함  
useEffect를 사용해야 하며 매끄러운 애니메이션을 위해 useLayoutEffect를 사용할 수 있음  
effect함수를 컴포넌트 함수 실행과 동시에 실행  
  
### 상세 화면에 콘텐츠 출력  
- FlatList를 사용하는 방법 외에도 리스트 항목이 너무 길지 않으면 직접 목록을 매핑  
  
### 헤더 버튼 추가하기  
- 라우팅 설정파일에서 헤더 옵션 추가 : 화면 콘텐츠를 렌더링하는 컴포넌트와 직접 연결할 필요가 없을 때   
- 직접 연결이 필요한 경우 화면 컴포넌트로 가야 함(useLayoutEffect, setOptions메서드 사용)  
  
### 헤더에 아이콘 버튼 추가하기  
- 아이콘은 이미 `@expo/vector-icons`의 아이콘을 사용할 수 있음  
  
### 드로어(Drawer) 내비게이션 추가하기 & 드로어 생성하기  
-  드로어 내비게이터 설치   
`npm install @react-navigation/drawer`  
`expo install react-native-gesture-handler react-native-reanimated`  
- 일관된 디자인으로 만들기 위해서는 배경색을 드로어 내비게이션에서 설정  
  
**type과 interface의 차이**  
- type은 유니언 타입과 복잡한 타입을 정의할 수 있다. interface는 안됨  
- type은 &연산자로 여러 타입을 결합할 수 있다. interface는 extends 키워드를 사용해 확장 가능  
- interface는 동일한 이름으로 여러 번 선언할 수 있으며 이 경우 인터페이스들이 자동으로 병합된다.  
  
~~~  
interface Person {  
  name: string;  
}  
  
interface Person {  
  age: number;  
}  
  
const person: Person = { name: 'John', age: 30 }; // name과 age 모두 포함  
~~~  
  
- type은 동일한 이름으로 두 번 정의할 수 없으며 병합이 자동으로 이뤄지지 않는다.  
- 관습적으로 React Navigation과 같은 라이브러리에서는 매개변수 리스트를 정의할 때 일반적으로 type을 사용한다. 타입을 통해 보다 간결하게 여러 타입을 결합하거나 유연하게 정의할 수 있기 때문  
- 유니언 타입(Union Type)이란 TypeScript에서 변수, 매개변수, 반환값 등이 두 개 이상의 타입 중 하나일 수 있음을 나타내는 타입이다. 유니언 타입을 사용하면 하나의 값이 여러 다른 타입 중 하나일 수 있도록 지정할 수 있다.  `| (파이프)` 기호를 사용하여 여러 타입을 연결해 정의한다.  
  
  
### 탭(Tap) 내비게이터  
`npm install @react-navigation/bottom-tabs`  
  
### 내비게이터 중첩하기  
- 주의점 : 드로어 내비케이터에서는 스택과 다르게 contentStyle이 아니라 sceneContainerStyle  
  
  
✅expo-router를 사용하면 이젠 자체적으로 NavigationContainer를 관리하기 때문에 알려준대로 하면 중복오류가 발생함✅  
  
<br>  
  
**indexOf**  
- JavaScript의 배열 메서드 중 하나로, 배열에서 특정 요소의 첫 번째 인덱스를 반환하는 역할을 한다.  
만약 해당 요소가 배열에 존재하지 않으면 -1을 반환함  
  
- 특징 및 기능  
검색 범위: 배열의 첫 번째 요소부터 마지막 요소까지 순차적으로 검색  
첫 번째 일치 값: 배열에 동일한 요소가 여러 번 포함되어 있어도 indexOf는 첫 번째로 일치하는 요소의 인덱스만 반환한다.  
엄격한 비교: indexOf는 === 연산자를 사용하여 요소를 비교. 즉, 타입이 다른 경우에도 일치하지 않는 것으로 간주된다.  
  
  
<video controls width="600">        
  <source src="/NextGenWebDev/assets/img/20240815_01.mp4" type="video/mp4">        
  Your browser does not support the video tag.        
</video>     
  