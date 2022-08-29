---
layout  : wiki
title   : 
summary : 
date    : 2022-08-29 16:57:04 +0900
updated : 2022-08-29 17:49:13 +0900
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
- 어떤 요청이 들어오면 그 요청에 맞는 servlet을 결정하고 실행하는 주체
- Tomcat은 servlet container의 일종

## dispatcher servlet의 정의
- 제일 앞에서 서버로 들어오는 모든 요청을 처리하는 front controller
- controller로 향하는 모든 웹 요청의 진입점으로써 웹 요청을 처리하고 결과 데이터를 client에 응답

## dispatcher servlet 동작 과정
클라이언트는 url로 정보를 요청한다
DispatcherServlet은 클라이언트의 요청을 HandlerMapping에 보낸다
HandlerMapping은 요청을 처리할 수 있는(요청을 매핑한) Controller를 검색한다
알맞은 컨트롤러가 있다면, HandlerMapping은 그 Controller로 요청을 보낸다
컨트롤러는 요청을 처리하고 Model과 View를 DispatcherServlet으로 반환한다
DispatcherServlet은 View 이름을 ViewResolver에게 보낸다
ViewResolver는 View 이름으로 결과를 알맞게 처리해 보여줄 View를 찾고 그 View를 반환한다
DispatcherServlet은 찾아낸 View에 Model을 보내고 화면 표시를 요청한다
View는 처리 결과가 포함된 View를 생성해 DispatcherServlet에 보낸다
DispatcherServlet은 최종 결과를 클라이언트에게 보낸다

## servlet vs dispatcher servlet

## 참고 자료
https://tecoble.techcourse.co.kr/post/2021-06-25-dispatcherservlet-part-1/
https://riimy.tistory.com/87
