---  
layout: post  
title:  "NextJS Extras"  
categories: [ Clone Coding ]  
image: assets/img/20240716_01.png  
tags: [featured]  
---  
  
# Live Streaming  
  
- 16강 참고  
  
  
# NextJS Extras  
  
- 프로젝트에서 다루지 않은 NextJS의 부차적인 기능들을 다룸  
  
## Fonts  
- 대부분 구글 폰트를 사용함  
![1]({{ site.baseurl }}/assets/img/20240716_01.png)    

<br>
  
## Private Folders  
- 폴더 앞에 underscore( _ ) 붙여주면 비공개로 됨  

<br>
  
## Catch All Segments  
- [id] : url의 1개의 파라미터  
- [...name] : multiple parameters 가능  
- [[...name]] : optional catch all segments 어떤 이름을 붙여도 매치 됨  
(사용예시)  
- 블로그 사이트에서 각 포스트에 접근할 때, 카테고리나 태그를 포함하는 URL 구조를 원할 경우  
- 유저 프로필 페이지가 있고, 특정 탭으로 나누어져 있는 경우  

<br>
  
## Logging  
- 외부 API로 작업할 때 유용함  
![2]({{ site.baseurl }}/assets/img/20240716_02.png)    

<br>
  
## Security  
- taintObjectReference : 객체 참조가 예상한 객체를 가리키고 있는지 확인하여, 악의적인 참조 변경을 방지  
- taintUniqueValue : 특정 값의 고유성과 신뢰성을 확인하는 데 사용.  
주로 입력 값의 무결성을 검사하거나, 고유한 식별자가 예상치 못한 값으로 변조되지 않도록 할 때 유용하다.  
- client에서 공유하면 안되는 데이터에 대한 경고를 설정할 수 있음  
- `npm i server-only` : 원하는 파일 전체 보호. client 컴포넌트가 파일의 함수를 inport하려고 하면 error  

<br>
  
## Images  
- 이미지 최적화   
- <Image/>태그의 placeholder="blur" : 네트워크 속도가 느릴 경우 blur처리된 이미지가 먼저 뜨게 처리  
- <Image/>태그의 blurDataURL="base64인코딩 된 이미지"  
- base64란? : 이미지를 텍스트 문자열로 변환  
이진 데이터를 ASCII 텍스트로 변환한다. 6비트씩 데이터를 나누고, 이를 64개의 문자로 매핑함(알파벳 대문자, 소문자, 숫자, +와 /)  
  
![3]({{ site.baseurl }}/assets/img/20240716_03.png)    
  
<br>