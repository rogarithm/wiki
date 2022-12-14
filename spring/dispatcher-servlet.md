---
layout  : wiki
title   : 
summary : 
date    : 2022-08-29 16:57:04 +0900
updated : 2022-08-29 20:09:48 +0900
tags    : 
toc     : true
public  : true
parent  : spring
latex   : false
---
* TOC
{:toc}

# 

## servlet container
- 서블릿 클래스의 로드, 초기화, 호출, 소멸까지의 주기를 관리
- 소켓을 만든다. 웹 서버와 통신을 통해 클라이언트의 request를 전달받아 response를 전달해야 하기 때문이다.
- 클라이언트로부터 request를 받을 때마다 쓰레드를 생성해 요청을 처리한다
- 어떤 요청이 들어오면 그 요청에 맞는 servlet을 결정하고 실행하는 주체
- Tomcat은 servlet container의 일종

## dispatcher servlet의 정의
- SpringMVC에서 제공하는 클래스
- front controller 역할
- controller로 향하는 모든 웹 요청의 진입점. 요청 처리를 컨트롤러에 위임하고 요청을 처리한 결과 데이터를 client에 전달

## dispatcher servlet 동작 과정
1) 클라이언트는 url로 Request를 보낸다
2) DispatcherServlet은 클라이언트의 Request를 HandlerMapping에 보낸다
3) HandlerMapping은 Request를 처리할 수 있는 (매핑된) Controller를 검색한다
4) 알맞은 Controller가 있다면, HandlerAdapter는 그 Controller로 Request 처리 요청을 보낸다
5) Controller는 요청을 처리하고 Model과 View를 DispatcherServlet으로 반환한다
6) DispatcherServlet은 View 이름을 ViewResolver에게 보낸다. ViewResolver는 View 이름으로 검색해 결과를 알맞게 처리해 보여줄 View를 찾고 그 View를 반환한다
7) DispatcherServlet은 찾아낸 View에 Model을 보내고 화면 표시를 요청한다
8) View는 처리 결과가 포함된 View를 생성해 DispatcherServlet에 보낸다
9) DispatcherServlet은 최종 결과를 클라이언트에게 보낸다

## servlet vs dispatcher servlet

## 참고 자료
https://tecoble.techcourse.co.kr/post/2021-06-25-dispatcherservlet-part-1/
https://tecoble.techcourse.co.kr/post/2021-07-15-dispatcherservlet-part-2/
https://riimy.tistory.com/87
https://taes-k.github.io/2020/02/16/servlet-container-spring-container/
