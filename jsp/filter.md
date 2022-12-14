---
layout  : wiki
title   : 
summary : 
date    : 2022-08-23 17:29:47 +0900
updated : 2022-08-24 18:20:39 +0900
tags    : 
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# 필터

## 필터의 정의와 특징
- HTTP 요청과 응답을 변경할 수 있는 재사용 가능한 클래스다. 매번 새로 만들지 않고 한 번 만든 것을 재사용한다.
- 필터는 클라이언트와 서버 간에 자원을 주고받을 때 반복해서 처리하는 일을 한다.
- 필터 여러 개로 필터 체인을 형성할 수 있다.
  + 의문점. 필터 여러 개가 체인을 이룰 때, 각 필터가 요청과 응답을 모두 처리한다면 요청에 가장 가까이 있는 필터는 요청을 첫 번째로 필터링하고 응답을 마지막으로 필터링할 것이다. 두 필터링은 서로 따로 동작해서 단계가 달라도 상관없는 건가?

## 필터 구현
- 필터 구현에 다음 세 타입을 이용한다.
- javax.servlet.Filter
  + init 메서드는 필터를 초기화할 때 호출한다.
  + doFilter 메서드는 필터 기능을 수행한다.
  + destroy 메서드는 필터가 삭제될 때 호출된다.
- javax.servlet.ServletRequestWrapper
  + 요청을 변경할 때 사용한다.
- javax.servlet.ServletResponseWrapper
  + 응답을 변경할 때 사용한다.

## 필터 설정
- web.xml 파일에서 필터 설정하는 방법과 필터 클래스에 @WebFilter 애노테이션을 사용하는 방법이 있다.
- web.xml로 필터 설정하는 방법은 서블릿 설정할 때와 비슷하다.
  + filter 태그는 필터로 사용할 클래스의 완전한 이름과 이 클래스를 참조할 때 쓸 이름을 설정한다.
  + 서블릿과 달리 filter 태그에는 init-param 태그가 있는데, init 메서드 호출 시 전달할 파라미터를 설정한다.
  + filter-mapping 태그에서는 필터 클래스와 url 패턴을 매핑한다.
  + url 패턴 대신 필터를 적용할 서블릿을 설정할 수 있다.
  + 필터 적용 시점을 설정할 수 있다.

## 요청 및 응답 필터링
1. 필터링하고 싶은 요청 정보가 있을 경우 HttpServletRequestWrapper 클래스를 상속받는 클래스를 만든다.
2. 1의 요청 정보를 추출하는 메서드를 재정의해서 필터링을 거친 요청 정보를 돌려주도록 구현한다.
3. 구현한 클래스 인스턴스를 FilterChain의 doFilter 메서드에 넘겨준다.
- 응답 정보도 같은 방식으로 처리한다.

## 참고 자료
https://datamod.tistory.com/39
