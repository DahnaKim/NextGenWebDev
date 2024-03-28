---
layout: post
title:  "Backend Documentation"
categories: [ WebDev ]
image: assets/img/20240327_1.png
tags: [featured]
---

# Backend Documentation  
  
## 주요기능  
- 초기 설정 및 라이브러리 임포트: Flask, Flask-RESTful, Flask-JWT-Extended 등 다양한 Flask 확장과 기타 필요한 라이브러리를 임포트하고, 애플리케이션과 API, CORS 설정을 초기화  
- 로깅 설정: 애플리케이션 로깅을 위한 기본 설정을 구성. 로그 파일은 logs/application.log에 저장되며, 로그의 크기와 백업 파일의 수에 제한  
- 데이터베이스 연결 및 종료 처리: MySQL 데이터베이스 연결을 설정하고, 애플리케이션 컨텍스트가 종료될 때 데이터베이스 연결을 안전하게 종료  
- 관리자 권한 검사 데코레이터: 특정 엔드포인트에 대한 접근을 관리자로 제한하기 위한 데코레이터 함수  
- 이미지 최적화: 업로드된 이미지 파일의 크기를 줄이고, 필요한 경우 포맷을 변환하여 저장  
- 회원가입 및 로그인 API: 사용자의 회원가입과 로그인 기능을 처리. 회원가입 시 비밀번호는 해시되어 저장됨  
- 사용자 정보 수정 및 삭제 API: 사용자가 자신의 정보를 수정하거나 계정을 삭제할 수 있는 기능. 관리자는 모든 사용자 정보를 수정하거나 삭제할 수 있다.  
- 이미지 파일 업로드, 수정, 삭제 API: 이미지 파일을 서버에 업로드하고, 데이터베이스에 이미지에 대한 메타데이터를 저장. 업로드된 이미지는 필요에 따라 최적화. 또한 이미지의 메타데이터를 수정하거나 이미지를 삭제하는 기능도 포함되어 있음  
- 이미지 목록 및 상세 정보 API: 업로드된 이미지의 목록을 페이지네이션으로 조회하고, 특정 이미지의 상세 정보를 조회할 수 있다.  
검색 API: 제목이나 설명을 기반으로 이미지를 검색하는 기능을 제공  
정적 파일 제공: Flask의 정적 파일 제공 기능을 이용하여 업로드된 이미지 파일을 클라이언트에 제공

<br>
  
## 코드정리  
  
### 초기 설정  
`static` : 서버에 저장되어 있는 그대로 클라이언트(웹 브라우저 등)에 전송되어 사용자의 요청에 따라 변하지 않는 파일들  
`api = Api(app)` : app을 Api 클래스의 생성자에 전달하여, api 객체가 app 객체와 연결. Flask 앱에 RESTful API를 통합  
**CORS(app, resources={r"/api/*": {"origins": "*"}})**  
`CORS(Cross-Origin Resource Sharing)` : 다른 도메인에서 현재 도메인으로의 리소스 요청을 허용하는 메커니즘  
`CORS(app, resources={r"/api/*": {"origins": "*"}})` : 모든 오리진("*"으로 표시됨)에서 app의 /api/* 경로로 시작하는 모든 리소스에 대한 요청을 허용. API를 외부에서도 접근할 수 있게 해준다.  
**csrf = CSRFProtect(app)**  
- CSRF(Cross-Site Request Forgery) 보호 기능  
  
**보안설정 접근 방법**  
1. 개발 초기 단계: CSRF 보호 기능을 비활성화(csrf = CSRFProtect(app)를 주석 처리)하여 백엔드 API 개발에 집중한다. 이 단계에서는 API 기능 구현과 테스트에 초점을 맞춤  
2. 프론트엔드 개발 시작: 프론트엔드 개발이 시작되면, 백엔드와 프론트엔드 간의 통신 방식을 정의하고, 필요한 경우 CORS 설정을 조정  
3. 보안 기능 추가: 프론트엔드와 백엔드 간의 기본적인 통신이 원활하게 이루어지면, CSRF 보호 기능을 포함한 추가 보안 조치를 적용. 이때 프론트엔드는 CSRF 토큰을 적절히 처리하도록 구현해야 함  
4. 보안 테스트: 모든 개발이 완료된 후에는 CSRF 보호를 포함한 애플리케이션의 보안 기능이 올바르게 작동하는지 꼼꼼히 테스트  

<br>
  
### 로깅 설정  
- RotatingFileHandler : 로그 파일의 크기가 일정 크기에 도달하면 새 파일로 전환하는 로그 핸들러. 이를 통해 로그 파일이 지나치게 커지는 것을 방지할 수 있다.  
- logs/application.log : 로그 파일의 경로와 이름을 지정  
maxBytes=10240 : 로그 파일의 최대 크기를 바이트 단위로 설정.   
backupCount=10은 로그 파일이 최대 크기에 도달했을 때 유지할 백업 파일의 수를 지정.   
최대 10개의 백업 파일을 유지하며, 이를 초과하면 가장 오래된 파일부터 삭제한다. 
- logging.Formatter : 로그 메시지의 포맷을 정의.   
로그가 발생한 시간(%(asctime)s),   
로그 레벨(%(levelname)s),   
메시지(%(message)s),   
파일 경로(%(pathname)s),   
줄 번호(%(lineno)d)를 포함  
- file_handler.setLevel(logging.ERROR) : 이 설정은 파일 핸들러에만 적용  
- app.logger.setLevel(logging.ERROR) : Flask 애플리케이션 로거 전체의 로그 레벨을 설정. 이는 애플리케이션 로거에 연결된 모든 핸들러에 적용되며, 처리될 로그 메시지의 최소 레벨을 결정함. 따라서, app.logger.setLevel(logging.ERROR)를 설정하면, 이 레벨 이하의 로그는 애플리케이션 로거를 통해 어디에도 기록되지 않음

<br> 
  
### 데이터베이스 연결 및 종료 처리  
- g 객체 : Flask에서 제공하는 컨텍스트 전역(global) 변수로, 현재 애플리케이션 컨텍스트에 대한 정보를 저장하는 데 사용. 한 요청의 처리 도중에는 전역적으로 접근할 수 있지만, 요청마다 독립적으로 존재한다.  
- if 'db' not in g : g 객체에 db 키가 존재하지 않는 경우를 체크. 즉, 현재 요청에 대한 데이터베이스 연결이 아직 생성되지 않았다면 새로운 연결을 생성 
- @app.teardown_appcontext: 이 데코레이터는 함수를 애플리케이션 컨텍스트가 종료될 때 호출되도록 등록. 함수는 예외 객체를 인자로 받을 수 있으며, 이는 애플리케이션 처리 중 발생한 예외를 참조할 때 사용될 수 있다.  
- db = g.pop('db', None): g 객체에서 db 키에 해당하는 값을 제거하고 그 값을 db 변수에 할당함. 만약 db 키가 존재하지 않는다면 None을 반환. pop 메소드는 해당 키-값 쌍을 g 객체에서 제거하는 역할을 한다.  
- if db is not None : 만약 db 변수에 유효한 데이터베이스 연결 객체가 저장되어 있다면, 즉, None이 아니라면 데이터베이스 연결을 닫음  
- db.close(): 데이터베이스 연결을 안전하게 종료. 이는 데이터베이스에 대한 모든 작업이 완료되었음을 의미하며, 리소스 누수를 방지함

<br>
