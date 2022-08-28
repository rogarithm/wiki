---
layout  : wiki
title   : 
summary : 
date    : 2022-08-28 16:37:41 +0900
updated : 2022-08-28 16:52:20 +0900
tags    : 
toc     : true
public  : true
parent  : spring
latex   : false
---
* TOC
{:toc}

# 

사이트 기능을 사용하려면 보통 로그인을 해야한다. 만약 사용자가 로그인한 다음 기능 페이지의 url을 복사해서 다음에 로그인하지 않은 채로 url로 바로 들어가면 인증되지 않은 상태이므로 문제가 생긴다.

그래서 클라이언트가 기능을 요청했을 때 서버에서 기능을 요청한 사용자의 세션을 체크하고, 올바른 세션인 경우 기능을 승인하지만, 올바르지 않을 경우는 로그인 페이지로 되돌려야한다.

구현 방법으로 Controller마다 기능 로직을 실행하기 전에 세션을 확인해주는 방법이 있다. 만약 기능이 1000가지라면 세션 확인도 1000번이므로 중복이 일어난다.

세션 확인 로직을 따로 빼서 Controller에서는 이 로직을 호출하도록 할 수도 있다. 하지만 여전히 모든 Controller에서 호출을 하니 중복이다.

이런 로직을 컨트롤러가 처리하기 전에 가로채서 처리하는 방법이 있다. filter와 interceptor가 그것이다.

https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile24.uf.tistory.com%2Fimage%2F2564124C588F496C01B966

차이점이 있다
- filter는 dispatcher servlet 앞에서 처리하고, interceptor는 dispatcher servlet에서 handler(controller)로 가기 전에 정보를 처리한다.
- filter는 J2EE 표준 스펙에 정의된 기능이고, interceptor는 spring framework에서 제공하는 기능이다.

## 참고 자료
- https://www.leafcats.com/39

## 더 알아볼 내용
- https://supawer0728.github.io/2018/04/04/spring-filter-interceptor/
