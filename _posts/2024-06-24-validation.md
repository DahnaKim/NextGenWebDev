---  
layout: post 
title:  "Validation" 
categories: [ Clone Coding ]  
image: assets/img/20240624_02.png
tags: [featured] 
--- 
  
# Validation  
  
## Introduction to Zod  
  
- 설치 npm i zod  
  
주요 기능  
1. 스키마 선언 (Schema Declaration):  
    * Zod를 사용하여 데이터의 구조를 선언할 수 있다. 예를 들어, 객체, 배열, 문자열 등의 데이터 형식을 정의할 수 있음  
2. 유효성 검사 (Validation):  
    * 선언된 스키마를 기반으로 입력 데이터를 검증할 수 있다. 데이터가 스키마에 맞지 않으면 오류를 반환한다.  
3. 타입 안전성 (Type Safety):  
    * TypeScript와 잘 통합되어, 선언된 스키마를 기반으로 TypeScript 타입을 자동으로 생성한다. 이를 통해 컴파일 시점에서 타입 오류를 미리 방지할 수 있다.  
4. 기본값 설정 (Default Values):  
    * 스키마에 기본값을 설정할 수 있어, 값이 제공되지 않았을 때 기본값을 사용할 수 있다.  
5. 커스텀 메시지 및 정밀한 오류 처리 (Custom Messages and Detailed Error Handling):  
    * 유효성 검사 실패 시 사용자 정의 오류 메시지를 설정할 수 있으며, 오류의 위치와 원인을 명확하게 전달할 수 있다.  
  
![2]({{ site.baseurl }}/assets/img/20240624_02.png)  
  
<br>  
  
![1]({{ site.baseurl }}/assets/img/20240624_01.png)  
  
- ?는 선택적 속성이라는 것을 의미함 즉, 이 속성은 생략될 수 있다.  
- = [ ] 구문은 errors 속성에 기본값을 설정하는 것이다.  
errors가 전달되지 않으면, 기본적으로 빈 배열([])이 사용된다.  

<br> 
  
## Validation Errors  
  
![3]({{ site.baseurl }}/assets/img/20240624_03.png)  
  
- `safeParse` : 검사 결과를 반환하며, 결과는 success 속성을 가지는 객체  
- `flatten` : 오류 메시지를 플랫(flat)한 구조로 변환하여, 각 필드의 오류 메시지를 키-값 쌍으로 반환한다.  

<br> 
  
## Refinement  
  
![4]({{ site.baseurl }}/assets/img/20240624_04.png)  
  
`z.object` : Zod 라이브러리에서 사용되는 메서드로, 객체 형태의 스키마를 정의할 때 사용됨. 객체의 각 필드에 대한 유효성 검사를 설정할 수 있다.  
  
`.superRefine` : Zod 스키마의 고급 유효성 검사 기능.   
단순한 필드 수준의 유효성 검사 외에도 여러 필드 간의 관계를 검증하거나 복잡한 조건을 검사할 때 유용하다. 스키마 전체의 유효성을 검사할 수 있는 후처리 단계에서 사용된다.  
.superRefine는 콜백 함수를 인수로 받아서, 스키마 전체를 한 번에 검사할 수 있게 한다. 이 콜백 함수는 두 개의 인수를 받는다.:  
값 객체 (values): 현재 유효성 검사를 받고 있는 데이터 객체  
컨텍스트 객체 (ctx): 유효성 검사 오류를 추가하기 위한 메서드를 제공  
콜백 함수 내에서는 필요한 논리를 통해 값을 검증하고, 유효성 검사에 실패할 경우 ctx.addIssue 메서드를 사용하여 오류를 추가할 수 있다. 

<br> 
  
## Transformation  
  
```  
const passwordRegex = new RegExp(  
  /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*?[#?!@$%^&*-]).+$/  
);  
```  
  
- `RegExp` : RegExp객체는 정규 표현식을 생성한다. 문자열이 특정 패턴과 일치하는지 확인하는 데 사용됨  

<br> 
  
```  
.regex(  
  passwordRegex,  
  "Passwords must contain at least one UPPERCASE, lowercase, number and special characters #?!@$%^&*-"  
)  
```  
  
- `.regex()` : 문자열이 특정 정규 표현식과 일치하는지 확인한다. 위의 경우, passwordRegex 정규 표현식을 사용하여 패스워드가 조건을 모두 만족하는지 검사한다. 만약 조건을 만족하지 않으면, 지정한 에러 메시지를 반환함  

<br> 
  
## Refactor  
  
![5]({{ site.baseurl }}/assets/img/20240624_05.png)  
  
`InputProps` : TypeScript 인터페이스로, 컴포넌트에 전달될 프로퍼티의 타입을 정의한다.   
  
`InputHTMLAttributes` :  React의 내장 타입으로, HTML <input> 요소의 모든 표준 속성을 포함하는 타입임. 이 타입을 사용함으로써, 정의하지 않은 모든 HTML 속성(예: type, placeholder, value 등)을 컴포넌트에 전달할 수 있게 된다. 이를 통해 컴포넌트가 더 유연하고 재사용 가능하게 됨  
  
`...rest` : 나머지 연산자(Rest Operator)로, 구조 분해 할당에서 사용되지 않은 나머지 속성을 하나의 객체로 모아준다. 이 방식으로InputHTMLAttributes에 정의된 모든 표준 HTML 속성을 input 요소에 전달할 수 있다. 예를 들어, 컴포넌트를 사용할 때 다음과 같이 추가적인 속성을 전달할 수 있다.  
  
```  
<Input   
name="username"   
type="text"   
placeholder="Enter your username"   
value={username}   
onChange={handleChange}   
/>  
```  
  
여기서 type, placeholder, value, onChange 등의 속성은 모두 ...rest를 통해 input 요소에 전달된다. 이로 인해 컴포넌트는 훨씬 더 유연하게 다양한 속성을 처리할 수 있게 된다.  
  
  