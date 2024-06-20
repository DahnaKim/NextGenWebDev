---        
layout: post   
title:  "Server Actions"  
categories: [ Clone Coding ]   
image: assets/img/20240618_4.png  
tags: [featured]   
---     
  
# Server Actions  
  
## Route Handlers  
  
- 기존 방식 : ReactJS에서 state를 사용해서 데이터를 처리하고 사용자가 로그인 버튼을 누르면 axios나 fetch 등을 사용해서 데이터를 전송  
webhook이나 React Native로 IOS나 Andriod 앱을 만들 때는 이 방법을 알고 있어야 함  
  
- 순서 :   
app에 api 폴더 생성(api/users/route.ts)  
`route.ts` 라는 파일을 생성하면 NextJS에게 이 파일이 API route라고 알려주는 것  
- GET 요청일 때  
  
![1]({{ site.baseurl }}/assets/img/20240618_1.png)  
  
  
- POST 요청일 때 (사용자가 form을 제출하면 body를 읽어야 함)  
  
```  
  
export async function POST(request:NextRequest) {  
    const data = await request.json();  
    return Response.json(data);  
}  
  
```  
  
**authentication처리는?**   
  
![2]({{ site.baseurl }}/assets/img/20240618_2.png)  
  
`const data = await request.json();` 에서 쿠키 확인하고 데이터베이스를 확인한 다음 로그인 결정  
  
![3]({{ site.baseurl }}/assets/img/20240618_3.png)  
  
<br>  
  
## Server Actions  
route 핸들러 ❌  
POST request 핸들러 ❌  
request의 body ❌  
fetch ❌  
=> NextJS가 자동으로 실행  
  
**input에 name 속성이 필요함**  
- components/form-input.tsx에 `name` prop 추가  
- app/login/page.tsx에 use server와 name 추가  
  
![4]({{ site.baseurl }}/assets/img/20240618_4.png)  
  
- `"use server";`는 항상 function의 최상단에 있어야 함  
  
<br>  
  
## useFormStatus  
- Server Action의 경과와 UI가 서로 소통하는 방법  
- (ex) Server Action이 로딩 중일 때는 버튼을 비활성화하기  
1. 사용자에게 Server Action이 시간이 걸린다는 것을 알리기  
`useFormStatus` : form action의 작업 상태를 알려주는 hook  
form이 pending 인지도 알려주고 어떤 데이터가 전송되었는지도 알려줌  
action에 대한 추가적인 정보를 제공해 준다.(ReactJS에서 제공)  
  
` const { pending } = useFormStatus();`  
=> 이 hook은 action을 실행하는 form과 같은 곳에서 사용할 수 없다.  
form의 자식 요소에서만 사용돼야 함. 즉 form의 상태에 따라 변경하고자 하는 component내부에서 사용해야 한다.  
  
![5]({{ site.baseurl }}/assets/img/20240618_5.png)  
  
`use server`와 `use client`  
- "use server";는 Next.js의 서버 측 실행을 명시하는 지시어이다.   
클라이언트 측에서는 실행되지 않고, 서버에서만 실행된다.   
이 지시어가 사용된 handleForm 함수의 예시에서는 서버에서 데이터 처리 로직을 실행하며, 클라이언트에게는 그 결과만 반환하여 서버의 로직을 보호하고 보안을 강화하는 데 도움이 된다.  
예시 코드에서 handleForm 함수는 new Promise를 사용하여 비동기적으로 5초 동안 지연 후 "logged in!"을 콘솔에 출력한다. 실제 환경에서는 이 위치에서 데이터베이스 조회, 사용자 인증 처리 등의 서버 사이드 로직을 수행할 수 있음  
이러한 구조는 Next.js의 SSR 기능과 함께 서버 측 로직을 효율적으로 처리하며, 보안이 중요한 작업을 서버에서만 실행하여 보안을 강화할 수 있다.  
  
- "use client"; 지시어 역시 Next.js 프레임워크에서 사용되며, 해당 코드가 클라이언트 측에서만 실행되어야 함을 명시한다. 서버 사이드 렌더링(SSR) 또는 정적 사이트 생성(SSG) 동안 이 코드는 실행되지 않는다. 이는 클라이언트 특정 API나 창(window) 접근과 같이 서버에서 실행될 수 없는 코드를 포함할 때 유용하다.   
코드에서 FormButton 컴포넌트는 사용자의 폼 제출 상태를 반영하기 위해 useFormStatus를 사용한다. useFormStatus는 클라이언트 상태(폼이 제출 중인지 여부)를 관리하기 때문에 서버에서 실행될 필요가 없다. 따라서 "use client";를 사용하여 이 컴포넌트가 클라이언트 측에서만 실행되도록 함으로써, 불필요한 서버 처리를 방지하고 클라이언트에서의 사용자 경험을 최적화할 수 있다.  

<br>  

## useFormState  
  
Server Action에서 오류가 발생해서 사용자에게 알릴 때 useFormState hook 사용  
결과를 알고 싶은 action을 인자로 넘겨줘야 한다.  
1. client component로 바꿔주고  
client component 내부에서 use server를 선언할 수 없기 때문에   
2. use server를 위한 actions.ts 라는 새로운 파일을 만들어 줌  
  
 ✔︎ useFormState를 쓸 때 실행하고자 하는 action을 전달하는 것 뿐만 아니라  
초기값도 필수적으로 제공해야 한다.  
  
action은 formData와 함께 호출되는데 처음에는 초기값 state와 함께 호출되고 다음부터는 이전 action에서 return된 state와 함께 호출된다.  
  
![6]({{ site.baseurl }}/assets/img/20240620_1.png)  
  
state (상태):  
* state는 현재 폼의 상태를 나타내며, handleForm 함수로부터 반환된 상태를 포함한다. 이 상태에는 폼의 데이터 또는 에러 메시지 등이 포함될 수 있다. 예를 들어, state.errors는 폼 검증 후 발생한 에러 리스트를 저장할 수 있다.  
action (액션):  
* action은 폼의 이벤트를 처리하기 위한 함수이다. 이 예에서는 <form> 태그의 action 속성으로 사용되고 있다. 폼이 제출되면, 이 action 함수가 호출되어 handleForm 함수를 실행하고 폼 데이터를 처리한다.  