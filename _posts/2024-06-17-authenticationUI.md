---      
layout: post 
title:  "Authentication UI"
categories: [ Clone Coding ] 
image: assets/img/20240617_4.png
tags: [featured] 
---   
  
# Authentication UI  
  
## Home screen  
  
**layout.tsx에서 템플릿 리터럴과 `${}` 구문을 사용해서 문자열을 동적으로 생성**  
  
```  
<body className={inter.className}>{children}</body>   
  
에서 아래로 변경  
  
 <body className={`${inter.className} bg-gray-900 text-white`}>{children}</body>  
  
```  
  
**모든 링크 글자색을 바꾸고 싶을 때**   
- globals.css에서 @layer base에 적용  

<br>
  
## Create Account Screen  
  
- app 하위에 create-account폴더 생성 - page.tsx 작성  
  
- 중복되는 버튼 역시 globals.css의 @layer components에 넣어주기  
  
![1]({{ site.baseurl }}/assets/img/20240617_1.png)  
  
![3]({{ site.baseurl }}/assets/img/20240617_3.png)  
  
- 아이콘 만들기  
[무료 아이콘 라이브러리](https://heroicons.com)  
`npm install @heroicons/react` 설치  

<br>
  
## Form Components  
- tailwind는 기본적으로 components 폴더도 보도록 되어 있음(config에서 확인가능)  
  
- app과 같은 라인에 components폴더 생성 후 form-input.tsx, form-btn.tsx 작성  
  
**props란**  
"properties"의 줄임말.   
props는 부모 컴포넌트가 자식 컴포넌트에게 데이터를 전달하는 방법이다. 이를 통해 컴포넌트 간에 데이터를 주고받을 수 있으며, 컴포넌트의 재사용성을 높일 수 있다.  
  
**props를 선언할 때 interface or type?**  
type이든 interface든 똑같은 동작을 하지만, 타입스크립트의 공식 문서에는 type의 기능이 필요하기 전 까지는 interface를 사용할 것을 권장함  
  
  
```  
{errors.map((error, index)=>( <span key={index} className="text-red-500 font-medium">{error}</span>  ))}  
  
```  
  
오류 메시지를 담은 errors 배열을 map 함수로 순회하여 각각의 오류 메시지를 <span> 요소로 렌더링한다.  
* key={index}: 각 오류 메시지에 고유한 키를 부여하여 리스트 렌더링 최적화  
* className="text-red-500 font-medium": 오류 메시지를 붉은 색과 중간 굵기 글씨로 표시  
  
- disabled상태일 때도 고려해서 작성해줘야 함  
사용자가 버튼을 여러 번 클릭해서 submit하는 것을 막을 수 있다.  
  
![4]({{ site.baseurl }}/assets/img/20240617_4.png)  

<br>
  
## Log in Screen  
- components 폴더에 social-login.tsx파일 생성  
  
**React Fragment란?**  
React에서 컴포넌트는 JSX를 통해 UI를 구성하는데, JSX는 여러 요소를 반환할 때 반드시 하나의 부모 요소로 감싸야 한다. 그러나 이 부모 요소가 추가적인 DOM 노드를 생성하여 불필요한 구조를 만들 수 있다. 이런 상황에서 React.Fragment 또는 단축 표기법인 `<> </>`를 사용하면 추가적인 DOM 노드 없이 여러 요소를 그룹화할 수 있다.  
  
- login폴더의 page.tsx 작성(create-account에서 작성한 것 복붙으로 가져와서 수정 작업)  
- sms폴더의 page.tsx 작성(login에서 작성한 것 복붙으로 가져와서 수정 작업)  