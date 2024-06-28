---  
layout: post  
title:  "Authentication"  
categories: [ Clone Coding ]   
image: assets/img/20240627_04.png  
tags: [featured]  
---  
  
# Authentication  
  
## Database Validation  
- `async`, `await` : async 함수는 비동기 작업을 처리하는 데 사용된다. async 키워드를 함수 앞에 붙이면 해당 함수는 항상 Promise를 반환한다. 또한, await 키워드를 사용하여 Promise가 해결될 때까지 기다릴 수 있다.  
  
- `npm`과 `npx`의 차이 : npm은 Node.js 패키지를 관리하고 설치, 업데이트, 삭제하는 데 사용  
npx는 npm 버전 5.2.0 이상에 기본적으로 포함된 도구이다. npx의 주요 목적은 패키지를 설치하지 않고도 일회성으로 실행할 수 있도록 함  
  
![1]({{ site.baseurl }}/assets/img/20240627_01.png)  

<br>
  
## Password Hashing  
패스워드 해싱(password hashing)은 사용자의 비밀번호를 안전하게 저장하기 위해 비밀번호를 해시 함수(hash function)를 사용하여 변환하는 과정이다. 해시 함수는 임의 길이의 입력 데이터를 고정 길이의 해시 값(hash value)으로 변환하는 일방향 함수이다. 즉, 해시 값에서 원래의 비밀번호를 역으로 추측하는 것이 매우 어렵다.  
  
**주요 해시 함수**  
- SHA-256: 보편적으로 사용되는 해시 함수. 그러나 패스워드 해싱에는 적합하지 않습니다.  
- bcrypt: 비밀번호 해싱에 많이 사용되는 함수. 솔트와 고유한 비용 인자(cost factor)를 사용하여 보안성을 높입니다.  
- scrypt: 메모리 집약적인 해시 함수로, GPU 기반 공격에 강합니다.  
- Argon2: 암호 해싱 경쟁에서 우승한 알고리즘으로, 현재 가장 강력한 비밀번호 해싱 알고리즘 중 하나입니다.  
  
`npm i bcrypt` : 설치  
`npm i @types/bcrypt` : 패키지에 대한 타입스크립트 definition을 얻을 수 있음  
  
![2]({{ site.baseurl }}/assets/img/20240627_02.png)  

<br>
  
## Iron Session  
사용자 로그인 -> 쿠키를 줌(id) 사용자가 무엇을 하든 브라우저가 자동으로 해당 쿠키를 서버로 보냄   
- `npm i iron-session` 설치 : 사용자가 변경할 수 있기 때문에 그대로 주면 안된다.  
- `getIronSession` : iron session 초기설정 해주기  
  
[비밀번호 생성기](https://1password.com/password-generator/)  
  
![3]({{ site.baseurl }}/assets/img/20240627_03.png)  
느낌표 : 타입스크립트에게 .env 안에 COOKIE_PASSWORD가 무조건 존재한다는 것을 알려주기 위함  

<br>
  
## Email Log In  
![4]({{ site.baseurl }}/assets/img/20240627_04.png)  

<br>
  
## superRefine  
  
![5]({{ site.baseurl }}/assets/img/20240627_05.png)  
  
- `path`, `fatal: true`, `return z.NEVER;` : 데이터베이스를 여러번 호출하지 않고 효율적으로 관리할 수 있음. 뒤에 다른 refine이 있어도 선행된 코드에서 틀리면 실행되지 않는다.  

<br>
  
## Log Out  
![6]({{ site.baseurl }}/assets/img/20240627_06.png)  
  
  