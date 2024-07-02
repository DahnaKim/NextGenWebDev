---  
layout: post  
title:  "Social Authentication"  
categories: [ Clone Coding ]  
image: assets/img/20240701_05.png  
tags: [featured]  
---  
  
# Social Authentication  
  
## Github Authentication  
- 많은 곳들이 OAuth 표준 방식을 많이 사용함 (카카오, 네이버, Facebook, Instagram, Apple login, Twitter(X) Login...)  
  
1. GitHub계정에서 OAuth Application등록  
(https://github.com/settings/applications/new)  
  
2. Generate a new client secret 버튼  
  
3. key 복사 후 .env파일에 `GITHUB_CLIENT_SECRET`에 붙여넣기  
  
4. Update application 버튼 클릭  
  
5. middleware.ts에 route등록  
  
6. 도큐먼트 꼭 확인  
  
![1]({{ site.baseurl }}/assets/img/20240701_01.png)  
  
**params 객체를 URL 쿼리 문자열 형식으로 변환하는 이유**  
1. HTTP GET 요청의 특성:  
OAuth 인증 요청은 일반적으로 HTTP GET 요청을 통해 이루어진다. GET 요청에서는 필요한 데이터를 URL에 쿼리 문자열의 형태로 포함시켜 서버로 전달한다.  
2. 쿼리 파라미터 형식:  
URL 쿼리 문자열은 ?로 시작하고, 각 파라미터는 &로 구분된다. 각각의 파라미터는 key=value 형태로 표현된다. 예를 들어, client_id=myClientId&scope=read:user,user:email&allow_signup=true와 같은 형식  
이러한 형식으로 데이터를 전달해야 서버가 이를 올바르게 해석하고 처리할 수 있다.  
3. OAuth 인증 프로세스:  
OAuth 인증 과정에서 클라이언트 애플리케이션은 사용자에게 GitHub 로그인 페이지를 제공해야 한다. 이 페이지로 리다이렉트할 때 필요한 정보(클라이언트 ID, 요청 권한 범위 등)를 URL 쿼리 문자열로 전달해야 한다.  
GitHub은 이 쿼리 문자열을 해석하여 사용자를 적절한 인증 페이지로 안내하고, 인증이 완료되면 클라이언트 애플리케이션에 필요한 인증 토큰을 반환한다.  
4. URLSearchParams 객체의 사용:  
URLSearchParams 객체를 사용하면 params 객체를 URL 쿼리 문자열로 쉽게 변환할 수 있다. 이는 파라미터들을 일일이 문자열로 변환하는 번거로움을 덜어준다.  
URLSearchParams 객체는 파라미터들을 자동으로 인코딩하여 URL로 전달하기에 적합한 형식으로 만들어준다. 예를 들어, 공백이나 특수 문자가 포함된 값들은 자동으로 인코딩된다.  
따라서 params 객체를 URL 쿼리 문자열 형식으로 변환하는 것은 OAuth 인증 요청을 위해 필요한 데이터를 올바르게 전달하기 위한 필수적인 과정이다. 이를 통해 클라이언트 애플리케이션은 사용자를 GitHub 인증 페이지로 적절히 리다이렉트할 수 있다.  

<br>
  
## Access Token  
[도큐먼트 참고](https://docs.github.com/en/apps/oauth-apps/building-oauth-apps/authorizing-oauth-apps)  
  
GitHub OAuth 인증 과정에서 사용자가 GitHub에서 인증한 후 돌아올 때 액세스 토큰을 교환하는 과정을 처리  
  
![2]({{ site.baseurl }}/assets/img/20240701_02.png)  

<br>
  
## Github API  
  
![3]({{ site.baseurl }}/assets/img/20240701_03.png)  
  
- path : app/github/complete/route.ts  
  
`cache: "no-cache"`   
1. 캐시된 응답 확인:  
cache: "no-cache"는 브라우저가 HTTP 캐시를 확인하도록 하지만, 항상 서버로부터 최신 리소스를 가져오도록 강제한다.  
즉, 브라우저는 먼저 캐시를 확인하고, 만약 캐시된 응답이 유효하다면 이를 사용하기 전에 서버에 요청을 보내 최신 리소스가 있는지 확인한다.  
2. 조건부 요청:  
브라우저는 서버로 조건부 요청을 보낸다. 조건부 요청은 If-Modified-Since 또는 If-None-Match헤더를 포함하여, 서버가 리소스가 변경되었는지 검사하게 한다.  
서버가 리소스가 변경되지 않았다고 응답하면 (304 Not Modified), 브라우저는 캐시된 리소스를 사용한다.  
서버가 리소스가 변경되었다고 응답하면 (200 OK), 브라우저는 새로운 리소스를 받아서 캐시를 업데이트한다.  
3. 최신 데이터 보장:  
이 설정은 항상 최신 데이터를 가져오도록 보장한다. 캐시된 응답이 있을 경우에도 서버에 최신 리소스가 있는지 확인한다.  
**사용 사례**  
- 동적 콘텐츠 : 서버의 데이터가 자주 변경되는 경우, 예를 들어, 사용자의 프로필 정보나 최신 뉴스 기사 등을 가져올 때 최신 데이터를 보장할 수 있다.  
- 보안 및 민감한 데이터 : 민감한 데이터가 포함된 응답을 캐시하지 않도록 하여 보안성을 높인다.  
- 실시간 데이터 : 항상 최신 상태를 유지해야 하는 데이터, 예를 들어 주식 가격이나 날씨 정보 등을 가져올 때 사용된다.  
  
- NextJS는 우리가 실행하는 fetch request들을 캐싱한다. 메모리에 저장하지 않기 위해서 사용. 프로세스를 진행하는 user마다 달라야하기 때문  
  
  
`id + ""` : 변수를 문자열로 변환  

<br>
  
## Code Challenge  
  
**기존 코드**  
  
```  
import db from "@/lib/db";  
import getSession from "@/lib/session";  
import { redirect } from "next/navigation";  
import { NextRequest } from "next/server";  
  
export async function GET(request: NextRequest) {  
  const code = request.nextUrl.searchParams.get("code");  
  if (!code) {  
    return new Response(null, {  
      status: 400,  
    });  
  }  
  const accessTokenParams = new URLSearchParams({  
    client_id: process.env.GITHUB_CLIENT_ID!,  
    client_secret: process.env.GITHUB_CLIENT_SECRET!,  
    code,  
  }).toString();  
  const accessTokenURL = `https://github.com/login/oauth/access_token?${accessTokenParams}`;  
  const accessTokenResponse = await fetch(accessTokenURL, {  
    method: "POST",  
    headers: {  
      Accept: "application/json",  
    },  
  });  
  const { error, access_token } = await accessTokenResponse.json();  
  if (error) {  
    return new Response(null, {  
      status: 400,  
    });  
  }  
  const userProfileResponse = await fetch("https://api.github.com/user", {  
    headers: {  
      Authorization: `Bearer ${access_token}`,  
    },  
    cache: "no-cache",  
  });  
  const { id, avatar_url, login } = await userProfileResponse.json();  
  const user = await db.user.findUnique({  
    where: {  
      github_id: id + "",  
    },  
    select: {  
      id: true,  
    },  
  });  
  if (user) {  
    const session = await getSession();  
    session.id = user.id;  
    await session.save();  
    return redirect("/profile");  
  }  
  const newUser = await db.user.create({  
    data: {  
      username: login,  
      github_id: id + "",  
      avatar: avatar_url,  
    },  
    select: {  
      id: true,  
    },  
  });  
  const session = await getSession();  
  session.id = newUser.id;  
  await session.save();  
  return redirect("/profile");  
}  
```  

<br>
  
### Code Challenge  
1. 재사용 가능한 login function 만들기  
2. GitHub를 통해 새로운 사용자를 생성할 때, 데이터베이스에서 동일한 username을 가진 사용자가 이미 존재하는지 확인하고, 존재하는 경우 에러를 반환  
3. GitHub API를 통해 이메일 정보를 가져오기  
4. fetch request 각각을 분리시키기, typescript와 type을 이용해서 각 function의 response의 값이 무엇인지 알려주기  
  
![5]({{ site.baseurl }}/assets/img/20240701_05.png)  
  
**getAccessToken 동작 방식**  
1. 함수 정의 및 매개변수:  
getAccessToken 함수는 code라는 문자열 매개변수를 받는다. 이 code는 GitHub의 OAuth 프로세스 중 사용자가 인증을 완료한 후 리디렉션 URL에 포함된 인증 코드이다.  
  
2. URLSearchParams 객체 생성:  
URLSearchParams는 URL 쿼리 문자열을 쉽게 만들고 조작할 수 있게 해주는 웹 API이다. new URLSearchParams({...})를 통해 GitHub에 요청할 때 필요한 매개변수들을 설정한다. (여기서는 client_id, client_secret, code를 설정)  
client_id: GitHub OAuth 애플리케이션의 클라이언트 ID  
client_secret: GitHub OAuth 애플리케이션의 클라이언트 시크릿  
code: 사용자가 GitHub 인증을 통해 받은 인증 코드  
3. 쿼리 문자열 생성:  
URLSearchParams 객체의 toString() 메서드를 호출하여 매개변수들을 쿼리 문자열로 변환한다.  
이 쿼리 문자열은 이후 HTTP 요청의 URL에 포함된다.  
4. 액세스 토큰 요청 URL 생성:  
GitHub의 액세스 토큰 엔드포인트 URL (https://github.com/login/oauth/access_token)과 쿼리 문자열을 결합하여 최종 요청 URL (accessTokenURL)을 생성한다.  
5. fetch 함수 호출:  
fetch 함수는 네트워크 요청을 보내기 위해 사용된다.  
accessTokenURL을 첫 번째 인자로, 요청의 설정을 담은 객체를 두 번째 인자로 전달한다.  
method: "POST": HTTP POST 메서드를 사용하여 요청을 보낸다.  
headers: { Accept: "application/json" }: 서버가 JSON 형식의 응답을 반환하도록 요청한다.  
6. 응답 처리:  
fetch 함수는 네트워크 요청을 보내고 응답을 기다린다. 이 응답은 Response 객체로 반환된다.  
accessTokenResponse.json()을 호출하여 응답 본문을 JSON 형식으로 파싱한다. 이 부분은 await키워드를 사용하여 비동기적으로 처리된다.  
7. 액세스 토큰 반환:  
파싱된 JSON 데이터는 액세스 토큰 응답 (AccessTokenResponse) 객체로 반환된다. 이 객체는 GitHub로부터 받은 액세스 토큰(access_token)과 오류(error) 정보를 포함한다.  
  
**실행 흐름**  
- 사용자가 애플리케이션에서 GitHub 로그인을 클릭  
- 사용자가 GitHub에서 인증을 완료하고 애플리케이션의 리디렉션 URL로 돌아옴  
- 리디렉션 URL에 포함된 code 매개변수를 이 함수에 전달  
- getAccessToken 함수가 호출되어 GitHub로부터 액세스 토큰을 요청  
- GitHub가 액세스 토큰을 반환하면, 이를 JSON 형식으로 파싱하여 반환  
- 반환된 액세스 토큰을 사용하여 GitHub API에 접근할 수 있게 됨  
  
**나머지 코드**  
  
```  
async function getGithubProfile(access_token: string): Promise<GithubProfile> {  
  const userProfileResponse = await fetch("https://api.github.com/user", {  
    headers: {  
      Authorization: `Bearer ${access_token}`,  
    },  
    cache: "no-cache",  
  });  
  return userProfileResponse.json();  
}  
  
async function getGithubEmails(access_token: string): Promise<GithubEmail[]> {  
  const emailResponse = await fetch("https://api.github.com/user/emails", {  
    headers: {  
      Authorization: `Bearer ${access_token}`,  
    },  
    cache: "no-cache",  
  });  
  return emailResponse.json();  
}  
  
// Function to check if user already exists by GitHub ID, email or username  
async function findExistingUser(id: string, email: string, username: string) {  
  const user = await db.user.findFirst({  
    where: {  
      OR: [  
        { github_id: id },  
        { email: email },  
        { username: username },  
      ],  
    },  
    select: {  
      id: true,  
    },  
  });  
  return user;  
}  
  
export async function GET(request: NextRequest) {  
  const code = request.nextUrl.searchParams.get("code");  
  if (!code) {  
    return new Response(null, {  
      status: 400,  
    });  
  }  
  
  const { error, access_token } = await getAccessToken(code);  
  if (error || !access_token) {  
    return new Response(null, {  
      status: 400,  
    });  
  }  
  
  const { id, avatar_url, login: githubLogin } = await getGithubProfile(access_token);  
  const emails = await getGithubEmails(access_token);  
  const primaryEmail = emails.find((email) => email.primary)?.email;  
  
  if (!primaryEmail) {  
    return new Response(  
      JSON.stringify({  
        error: "Primary email not found.",  
      }),  
      {  
        status: 400,  
      }  
    );  
  }  
  
  const existingUser = await findExistingUser(id + "", primaryEmail, githubLogin);  
  
  if (existingUser) {  
    return new Response(  
      JSON.stringify({  
        error: "A user with the same GitHub ID, email, or username already exists.",  
      }),  
      {  
        status: 400,  
      }  
    );  
  }  
  
  const newUser = await db.user.create({  
    data: {  
      username: githubLogin,  
      github_id: id + "",  
      avatar: avatar_url,  
      email: primaryEmail,  
    },  
    select: {  
      id: true,  
    },  
  });  
  
  return login(newUser.id);  
}  
  
```  
  
![4]({{ site.baseurl }}/assets/img/20240701_04.png)  
  
  