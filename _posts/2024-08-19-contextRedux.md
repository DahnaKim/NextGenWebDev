---  
layout: post  
title:  "Redux & Context API"  
categories: [ React Native ]  
image: assets/img/20240819_01.png  
tags: [featured]  
---  
  
# Managing App-Wide State with Redux & Context API  
  
React Native에서 Redux와 React의 Context API는 상태 관리에 사용되는 도구로, 둘 다 글로벌 상태를 다루는 데 유용하다.  
  
## Redux  
애플리케이션의 상태를 관리하기 위한 예측 가능한 상태 컨테이너.   
주로 대규모 애플리케이션에서 사용되며, 상태가 어떻게 변하는지를 명확하게 추적할 수 있도록 도와준다.  
Action: 상태에 대한 의도를 설명하는 객체. 타입과 함께 필요한 데이터를 포함한다.  
Reducer: 상태를 변경하는 순수 함수. 이전 상태와 액션을 받아 새로운 상태를 반환한다.  
Store: 애플리케이션의 모든 상태를 포함하고 있는 객체. createStore를 사용하여 스토어를 생성하고, 상태를 구독하거나 업데이트할 수 있다.  
Middleware: 액션이 리듀서에 도달하기 전에 중간에 끼어들어 추가적인 작업을 수행할 수 있다. 예를 들어, redux-thunk를 사용하면 비동기 작업을 처리할 수 있다.  
slice : Redux Toolkit에서 제공하는 기능 중 하나로, Redux 상태 관리를 간소화하고 더 직관적으로 사용할 수 있도록 도와준다. Redux의 상태와 리듀서를 한 곳에서 정의하고 관리할 수 있게 해준다. 이를 통해 상태 관리의 복잡성을 줄이고 코드의 가독성을 높일 수 있다.  
  
## React's Context API  
컴포넌트 트리 전체에 전역 데이터를 전달할 수 있는 간단한 방법을 제공한다. Redux만큼 강력하지는 않지만, 작은 규모의 애플리케이션이나 간단한 전역 상태 관리에는 충분함  
Context: 전역적으로 접근할 수 있는 데이터를 생성. React.createContext를 사용하여 생성한다.  
Provider: Context를 구독하는 컴포넌트에게 값을 전달하는 역할을 한다. Provider 컴포넌트는 트리의 상위에 위치하여 하위 컴포넌트들이 Context에 접근할 수 있게 한다.  
Consumer: Context의 값을 사용하는 컴포넌트이다. useContext 훅을 사용하여 쉽게 값을 가져올 수 있다.  
  
## Redux와 Context API의 비교  
- 복잡성  
Redux: 보일러플레이트 코드가 많아 설정 및 사용이 복잡할 수 있다. 하지만 대규모 애플리케이션에서는 상태 관리가 명확해지고 확장성이 좋다.  
Context API: 설정이 간단하고 작은 프로젝트나 단순한 상태 관리에는 매우 유용하다. 하지만 상태가 복잡해지면 사용이 어려워질 수 있다.  
- 상태 관리의 목적  
Redux: 상태의 변화를 추적하고, 복잡한 상태 로직을 명확하게 관리하기 위해 설계되었다. 상태가 변화할 때마다 로그를 남기거나, 개발자 도구를 통해 상태를 추적할 수 있는 점이 큰 장점.  
Context API: 간단한 전역 상태를 쉽게 공유하기 위한 용도로 설계되었다. 상태 변화의 추적 기능이 부족하고, 복잡한 로직 관리에는 한계가 있다.  
- 비동기 작업 처리  
Redux: 미들웨어를 통해 비동기 작업을 처리할 수 있다. 예를 들어 redux-thunk나 redux-saga를 사용하여 복잡한 비동기 로직을 쉽게 관리할 수 있다.  
Context API: 비동기 작업 처리 기능이 내장되어 있지 않아, 이를 처리하기 위해서는 별도의 로직을 작성해야 한다.  
- 퍼포먼스  
Redux: 리덕스는 상태가 업데이트될 때 필요한 최소한의 리렌더링만 발생하도록 설계되었다.  
Context API: 상태가 업데이트될 때 관련된 모든 하위 컴포넌트가 리렌더링될 수 있어 퍼포먼스 이슈가 발생할 수 있다.  
- 결론  
작은 프로젝트나 단순한 상태 공유의 경우 Context API가 적합하다.  
복잡한 상태 관리, 비동기 작업, 또는 상태 추적이 필요한 대규모 애플리케이션에서는 Redux가 더 적합함.  
  
## Redux의 예시  
- 대규모 전자상거래 앱  
사용자 A: 로그인 후 쇼핑 카트에 여러 상품을 추가하고, 이후 다른 페이지로 이동하여 상품 리뷰를 읽고, 결제 페이지로 돌아옴  
사용자 경험: 사용자가 페이지를 이동하거나 새로고침을 하더라도, 쇼핑 카트에 추가한 상품들이 그대로 유지됨. 이는 Redux가 애플리케이션 전체에서 글로벌 상태(예: 로그인 상태, 쇼핑 카트)를 관리하고 있기 때문이다. Redux를 사용하면, 페이지가 변경되더라도 상태가 일관되게 유지되어 사용자에게 안정적이고 일관된 경험을 제공한다.  
추가 예시: 사용자가 결제 페이지에서 쿠폰 코드를 입력하면, Redux가 이 정보를 전역 상태로 관리하여 이후 결제 단계에서 자동으로 적용된다. 따라서 사용자는 어디서나 동일한 쿠폰 코드가 적용된 가격을 확인할 수 있다.  
  
## Context API의 예시  
- 소셜 미디어 앱  
사용자 B: 다크 모드를 선호하는 사용자는 설정 페이지에서 다크 모드를 활성화. 이후 피드 페이지로 이동하면 배경색이 어두운 색으로 변경되어 표시된다.  
사용자 경험: Context API를 사용하여 테마 설정(다크 모드/라이트 모드)을 전역으로 관리한다. 설정 페이지에서 변경된 테마는 앱의 모든 페이지에 걸쳐 동일하게 적용된다. 사용자는 페이지를 이동할 때마다 설정한 테마가 유지되어 일관된 사용자 경험을 느낌  
추가 예시: 사용자 B가 언어 설정을 변경하면, Context API가 이 설정을 전역적으로 적용하여 모든 텍스트가 새로운 언어로 번역된다. 이 경우 사용자는 어디에서나 동일한 언어로 앱을 사용할 수 있게 된다.  
- 비교 정리  
Redux 사용 시: 사용자는 페이지를 자주 이동하거나 새로고침을 하는 상황에서도 쇼핑 카트 상태, 로그인 상태 등과 같은 중요 데이터를 안정적으로 유지할 수 있음  
Context API 사용 시: 사용자는 설정한 테마나 언어가 앱 전체에 즉시 반영되는 경험을 하게 된다. 설정된 값은 비교적 간단한 전역 상태로 유지되어, 사용자가 한 번 설정하면 앱의 모든 곳에서 반영된다.  
  
### context로 앱 전반에 걸친 상태 관리하기  
프로젝트 안의 **store**폴더 : 앱 전반에 걸친 상태에 연관된 논리를 담을 폴더에 흔히 관례로 store 이름을 붙임  
  
### useContext로 생성된 콘텍스트 사용하기  
- 콘텍스트에 정의된 메서드로 액세스 -> ids에 엑세스해서 화면에 뜬 음식 id가 즐겨찾기 ids에 들어 있는지 확인  
  
### Redux & Redux 툴킷 시작하기  
`npm install @reduxjs/toolkit react-redux` 설치  
  
### Redux Slices로 작업하기  
- 앱이 제공하는 기능에 따라 별도의 파일을 사용하는 것이 통상적  
- Context에서 설정한 것과 달리 상태를 변경하는 함수가 초기 상태에 추가되지 않음  
  

<video controls width="600">        
  <source src="/NextGenWebDev/assets/img/20240819_01.mp4" type="video/mp4">        
  Your browser does not support the video tag.        
</video>     
  
  
  
  