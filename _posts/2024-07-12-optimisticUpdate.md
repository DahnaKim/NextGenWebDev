---  
layout: post  
title:  "Optimistic Updates"  
categories: [ Clone Coding ]  
image: assets/img/20240712_01.png  
tags: [featured]  
---  
  
# Optimistic Updates  
Optimistic Updates는 사용자가 인터페이스에서 어떤 작업을 수행했을 때, 서버의 실제 응답을 기다리지 않고 먼저 사용자 인터페이스를 업데이트하는 기법이다. 이 방법을 사용하면 사용자 경험이 훨씬 더 빠르고 원활하게 느껴진다. 서버와의 통신이 완료되기를 기다리는 동안 생길 수 있는 지연 시간을 최소화할 수 있다.  
낙관적 업데이트의 일반적인 흐름은 다음과 같다:  
1. 사용자 액션: 사용자가 어떤 작업을 수행한다. 예를 들어, 버튼을 클릭해서 데이터를 업데이트하거나 삭제하는 경우.  
2. 즉시 UI 업데이트: 서버의 응답을 기다리지 않고 즉시 UI를 업데이트한다. 예를 들어, 사용자가 항목을 삭제했을 때 화면에서 해당 항목을 바로 제거한다.  
3. 서버 요청: 동시에 서버에 요청을 보낸다. 이 요청은 실제 데이터베이스나 서버 상태를 업데이트하는 작업임  
4. 서버 응답 처리: 서버에서 응답이 오면 성공 또는 실패 여부에 따라 추가 작업을 수행한다.  
    * 성공: 이미 UI가 업데이트된 상태이므로 별도의 추가 작업이 필요하지 않을 수 있다.  
    * 실패: 에러 처리가 필요하며, 실패한 경우 UI를 원래 상태로 되돌리거나 사용자에게 오류 메시지를 표시한다.  
단점:  
* 데이터 불일치 가능성: 서버 요청이 실패할 경우, UI와 실제 데이터 상태가 일치하지 않을 수 있다.  
* 추가 코드 필요: 실패 시 UI를 복구하거나 오류를 처리하는 추가 코드가 필요하다.  

<br>

## Introduction  
**변경파일**  
- prisma/schema.prisma  

<br>
  
## See Posts  
**변경파일**  
- app/(tabs)/life/loading.tsx  
- app/(tabs)/life/page.tsx  

<br>
  
## Likes and Dislikes  
**변경파일**  
- app/posts/[id]/page.tsx  

<br>
  
## Cache Tags  
**변경파일**  
- app/posts/[id]/page.tsx  

<br>
  
## useOptimistic  
- like같은 경우 유저의 UI를 업데이트하기 위해 백엔드에 like가 잘 등록됐는지 확인하고 기다릴 필요가 없음  
optimistic response는 마치 백엔드에서 mutation이 성공한 것처럼 UI를 수정하는 것  
**변경파일**  
- app/posts/[id]/actions.ts  
- app/posts/[id]/page.tsx  
- components/like-button.tsx  
  
  
  