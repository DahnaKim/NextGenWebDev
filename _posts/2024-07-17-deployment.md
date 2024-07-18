---  
layout: post  
title:  "Deployment"  
categories: [ Clone Coding ]  
image: assets/img/20240717_03.png  
tags: [featured]  
---  
  
# Deployment  
  
## Introduction  
- Vercel에 배포  
- PostgreSQL 데이터베이스를 포함하는 serverless 스토리지 서비스를 사용할 예정  
- 무료 배포 가능  
- 준비 : Vercel 가입하기, GitHub에 코드 push  
  
<br>

## Deploying  
- 프로젝트에 포함된 데이터베이스는 파일에 포함된 것일 뿐임  
- 앱을 Vercel에 배포할 때마다 새로운 서버를 줌 (이전 서버는 폐기됨)  
- 새 버전을 배포하면 이전 서버를 사용해서 저장했던 모든 클라이언트가 삭제 됨  
- PrismaClient는 node_module안에 존재 함 -> Vercel은 자체 node_module을 가지고 있어서 나의 앱 node_module을 인식 못함  
![1]({{ site.baseurl }}/assets/img/20240717_01.png)  
-> 내 Client를 Vercel의 node_modules 폴더에 넣음  
- `Connect Project`  
- POSTGRES_PRISMA_URL, POSTGRES_URL_NON_POOLING 환경변수 복사본을 갖고 싶은 경우  
`npm i -g vercel`   
`vercel login`  
`vercel link` -> y -> 계정선택 -> y  
`vercel env pull  .env.development.local`   
- Client 생성 + migrations도 적용  
![2]({{ site.baseurl }}/assets/img/20240717_02.png)  

<br>
  
## Serverless Database  
- Vercel에서 스토리지 생성  
- Prisma 탭에서 복사해서 Schema의 db에 교체  
- 기존 migration 삭제  
- Postgresql을 위해 새 migrations를 생성  
- 다운로드한 환경변수 .env에도 넣어줘야 함(URL, directURL변수  
- `npx prisma migrate dev --create-only` : 데이터베이스 연결  
- version control이 있어야 함(.gitignore에 있으면 안됨)  

<br>
  
## Environment Variables  
- Vercel project settings -> environment variables -> 환경변수 추가  
또는  
- 콘솔에서 `vercel env add 환경변수`  
- middleware에서 문제가 있던 변수들을 추가 하기  
✔︎ 내 PC와 vercel에서 같은 COOKIE_PASSWORD를 쓰는 것은 절대 추천하지 않음  
- 배포 후에는 `npx prisma migrate dev`를 쓰지 않는 것을 추천 :   
즉시 DB에 마이그레이션을 시도하기 때문 ->   
끝에 `--create-only`를 붙여 먼저 실행.   
직접 마이그레이션 코드를 보는 것을 추천함(Prisma가 비효율적인 SQL을 작성한 경우 문제가 생길 수도 있다.) ->   
모든 것을 확인 후 `npx prisma migrate deploy`실행  
- 마지막으로 Settings -> Functions -> 함수들이 작동할 위치나 리전을 수정  
사용자나 DB에 최대한 가깝게 위치시키는 것이 좋다.(돈 지불하면 더 빨라짐)  
- 리전 변경하면 다시 배포 해줘야 함(Deployment - Redeploy - Use existing Build Cache 체크 - Redeploy)  

<br>
  
# PlanetScale  
![4]({{ site.baseurl }}/assets/img/20240717_04.png)  
- 팀 규모가 크거나 여러 명이 데이터베이스를 변경할 때 쓰기 좋음  
- 큰 회사들을 위한 옵션 (데이터베이스 변경이 자주 일어나거나 migration을 완전히 통제, 데이터베이스를 더 세부적으로 모니터링하길 원할 때)  
- 로드밸런서를 바로 적용해준다는 장점이 있음(유저가 엄청 많다면 로드밸런서가 request의 종류를 확인해서 해당 request를 Prisma의 primary 데이터베이스 또는 복제본으로 전달)  
- create database를 생성했을 때 생성된 비밀번호를 반드시 .env에 저장  

<br>
  
## Connecting To PlanetScale  
**Connect with 옵션**  
- PlanetScale CLI : 데이터베이스에 직접 접근하고 싶을 때 사용  
- 아니면 그냥 Prisma 선택  
- DATABASE URL을 .env파일에 넣기(별표를 데이터베이스 생성시 선택한 비번으로 바꿔줘야 함)  
- schema.prisma에 제공받은 db코드 넣어주기  
- setting - foreign key 제약조건 활성화(Allow foreign key constraints)  
- `npx prisma db push` : PlanetScale 데이터베이스와 schema.prisma 동기화(only for 개발모드!) 데이터가 손실될 가능성이 항상 있음  

<br>
  
## Safe Migrations  
(production mode)  
- PlanetScale의 강점은 migration과 변경사항에 대한 모든 걸 제어할 수 있게 해줌  
- safe migrations 옆에 있는 설정 클릭  
- Enable safe migrations 활성화 - Save branch settings  
- ex) 모델을 schema.prisma에 추가   
-  Connect with 옵션에서 PlanetScale CLI 선택  
- `brew install planetscale/tap/pscale` : 설치  
- 설치 후 `pscale login`  
- `pscale connect 프로젝트명 main` : 데이터베이스 연결  
- 생성된 local address 를 .env의 DATABASE_URL에 넣어주고 데이터베이스 이름 추가로 넣어줌 (ex : mysql://root@127.0.0.1:3306/프로젝트명)  
- `npx prisma studio`  
- 연결 후 `npm run dev`   
✔︎변경사항이 있다면 절대 종료시키면 안됨. URL이 더 이상 PlanetScale에 연결 되지 않는다.  
-  `pscale branch create 데이터베이스명 브랜치명` : 새 브랜치 생성  
- main은 safe migration이 활성화 돼 있어서 변경사항을 그냥 push할 수 없다.  
- 변경사항 적용 : Deploy branch changes(PlanetScale UI)에 작성  
- 완료되면 다시 메인으로 돌아간 후 branch UI에서 기존 브랜치 삭제하면 됨  