---
layout: post
title:  "Interpreter Settings"
categories: [ WebDev ]
image: assets/img/20240229_1.png
tags: [featured]
---
  
1. PyCharm 환경 설정  
setting - 프로젝트명 - Interpreter  
  
⭐️ 설치 리스트  
- Flask : 웹프레임워크. 설치하면 Jinja2템플릿 엔진도 같이 설치됨  
Python 코드와 함께 동적 웹 페이지를 쉽게 생성할 수 있음  
- MySQL : 데이터베이스  
- WTForms : 사용자 입력처리  
- Flask-Login : 사용자 인증, 권한 부여  
- Flask-RESTful : REST API 구축   
- Flask-Security : 사용자 인증과 권한 부여를 쉽게 관리할 수 있게 해줌.  
계정 생성, 로그인, 로그아웃, 비밀번호 재설정 등의 기능  
사용자 역할 관리를 통해 특정 페이지나 기능에 대한 접근을 제어할 수 있다.  
- Flask-Talisman : HTTPS를 강제하고 보안 관련 HTTP 헤더를 쉽게 추가할 수 있게 해줌. XSS(Cross-Site Scripting), 클릭재킹 등의 공격으로부터 웹사이트를 보호  
  
![1]({{ site.baseurl }}/assets/img/20240229_1.png)  

<br>

🛠️ **MySQL 설치 오류 해결**  
Apple Silicon(M1,2,3칩)을 사용하는 Mac은 기본적으로 ARM아키텍처를 사용함  
Homebrew설치 위치와 실행 중인 터미널의 아키텍처가 일치해야 함  
~~~  
`arch -arm64 brew install mysql`   
⇨ ARM 아키텍처를 사용해서 Homebrew명령 실행  
~~~  
  
💡**ARM(Advanced RISC Machine)아키텍처** :   
RISC(Reduced Instruction Set Computing) 기반의 프로세서 아키텍처를 의미  
저전력, 고효율 설계에 중점을 두고 있으며, 주로 모바일 기기, 임베디드 시스템, 그리고 최근에는 개인용 컴퓨터와 서버 등의 분야에서도 널리 사용되고 있다.  
x86과 비교 : Intel과 AMD에 의해 개발   
전통적으로 데스크톱 컴퓨터, 랩톱, 서버 등에서 사용되어 왔다. x86 아키텍처는 복잡한 명령어 집합(CISC)을 기반으로 하며, 고성능을 중시하는 설계가 특징임   
반면, ARM은 RISC 기반으로, 간결한 명령어 집합과 저전력 설계에 중점을 둔다.  
  
2. 간단한 Flask 애플리케이션 생성  
![2]({{ site.baseurl }}/assets/img/20240229_2.png)  
`__name__` : 현재 실행 중인 스크립트. 실행되면 `__main__`으로 설정됨  
`if __name__ == '__main__'` : 스크립트가 모듈로 임포트되어 다른 곳에서 사용될 때는 Flask 애플리케이션을 자동으로 실행하지 않도록 하기 위함  
`app.run(debug=True, port=5000)`   
app.run() : Flask 애플리케이션을 시작  
debug=True : 매개변수로 디버그 모드를 활성화   
애플리케이션에 변경이 있을 때 서버가 자동으로 재시작되고, 오류 발생 시 디버그 정보가 웹 페이지에 표시됨.  
개발 과정에서 문제를 더 쉽게 발견하고 해결할 수 있게 도와준다.  
기본적으로 app.run()은 localhost의 5000번 포트에서 애플리케이션을 실행.

<br>
