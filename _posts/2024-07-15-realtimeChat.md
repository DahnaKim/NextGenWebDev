---  
layout: post  
title:  "Realtime Chat"  
categories: [ Clone Coding ]  
image: assets/img/20240715_01.png  
tags: [featured]  
---  
  
# Realtime Chat  
  
Supabase는 Realtime Chat 애플리케이션에서 실시간 데이터 동기화 및 브로드캐스트 기능을 제공하여 사용자들이 실시간으로 메시지를 주고받을 수 있도록 한다.   
1. 데이터베이스 관리  
Supabase는 PostgreSQL을 기반으로 한 관리형 데이터베이스를 제공한다. 이 데이터베이스는 애플리케이션의 모든 데이터를 저장하고 관리한다. 예를 들어, 채팅 애플리케이션에서는 사용자의 정보, 채팅방 정보, 메시지 등을 저장함.  
2. 실시간 데이터베이스 변경 감지  
Supabase는 실시간으로 데이터베이스 변경 사항을 감지하고 클라이언트에 브로드캐스트한다. 이를 통해 사용자가 새로운 메시지나 업데이트된 데이터를 실시간으로 받을 수 있다.   
3. Realtime 기능  
Supabase의 Realtime 기능은 클라이언트가 특정 채널을 구독(subscribe)하고, 해당 채널에 발생하는 이벤트를 실시간으로 수신할 수 있도록 한다. 채널은 주로 특정 주제나 엔티티(예: 채팅방)에 대한 실시간 이벤트 스트림을 나타낸다. 클라이언트는 특정 이벤트(예: 새로운 메시지)가 발생하면 이를 실시간으로 수신하고 UI를 업데이트할 수 있다.  
4. 인증 및 보안  
Supabase는 인증(authentication) 및 권한 부여(authorization) 기능도 제공한다. 이를 통해 사용자는 안전하게 로그인하고, 자신의 데이터에만 접근할 수 있다. 채팅 애플리케이션에서는 사용자가 로그인하여 자신이 속한 채팅방에만 접근하고 메시지를 주고받을 수 있다.  
5. API 제공  
Supabase는 RESTful API와 GraphQL API를 통해 클라이언트가 데이터베이스와 상호 작용할 수 있도록 한다. 이를 통해 클라이언트는 데이터를 쿼리하고, 삽입하고, 업데이트하고, 삭제할 수 있다. 예를 들어, 새로운 메시지를 데이터베이스에 저장하거나, 특정 채팅방의 메시지를 불러오는 등의 작업을 API를 통해 수행할 수 있다.  

<br>
  
## Models  
**변경파일**  
- app/(tabs)/home/page.tsx  
- app/products/[id]/page.tsx  
- components/list-product.tsx  
- prisma/schema.prisma  
- prisma/migrations/20240715214431_chatroom_and_message/migration.sql  

<br>
  
## Chat Room  
**변경파일**  
- app/chats/[id]/page.tsx  
- app/products/[id]/page.tsx  

<br>
  
## Messages  
**변경파일**  
- app/chats/[id]/page.tsx  
- components/chat-messages-list.tsx  

<br>
  
## Realtime Channel  
- 프로젝트 생성, SUPABASE_PUBLIC_KEY, SUPABASE_URL가져오기  
`npm install @supabase/supabase-js` 설치  
  
**변경파일**  
- app/chats/[id]/page.tsx  
- components/chat-messages-list.tsx  

<br>
  
## Supabase Broadcast  
**변경파일**  
- components/chat-messages-list.tsx  

<br>
  
## Realtime Messages  
**변경파일**  
- app/chats/[id]/page.tsx  
- components/chat-messages-list.tsx  

<br>
  
## Code Challenge  
**변경파일**  
- app/chats/actions.ts  
- components/chat-messages-list.tsx  
  
   