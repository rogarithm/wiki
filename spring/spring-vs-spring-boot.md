---
layout  : wiki
title   : 
summary : 
date    : 2022-08-24 21:42:21 +0900
updated : 2022-08-24 22:00:20 +0900
tags    : 
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# spring vs spring boot

## spring framework
- spring framework를 이용해서 엔터프라이즈 애플리케이션을 보다 쉽게 만들 수 있다.

### DI, IoC
- 의존성 주입(Dependency Injection), 제어 역전(Inversion of Control)
- 객체 간 결합도를 느슨하게 만들어줄 수 있다.
- 단위테스트 만들기가 편해진다
- 코드 재사용성이 높아진다

### 엔터프라이즈 애플리케이션?
- 회사 애플리케이션은 사용자가 많고, 따라서 데이터가 많다.
- 그래서 대규모 데이터 쿼리, 트랜젝션 처리가 필요해진다.
- 이런 처리를 개발자가 직접 만들기 어렵다.
- spring framework에서 모듈을 제공하고, 다른 프레임워크를 통합하고 중복 코드를 제거함으로써 개발자는 비즈니스 로직에만 집중할 수 있도록 도와준다.

## spring-boot
- spring framework는 환경설정이 복잡하다.
- spring boot는 환경설정을 자동화해서 사용자가 더 편하게 spring을 활용할 수 있도록 해준다.
- 최소한의 설정으로 스프링 플랫폼과 서드파티 라이브러리들을 사용할 수 있도록 한다.
- 스프링 프레임워크를 사용하기 쉽게 해주는 도구다.
- 어떻게 도와주나? 자동 설정을 해주고, 의존성 관리가 쉽고, 내장 서버가 있다.

### 자동 설정
- 스프링 기능을 자동 설정해준다.
- starter 의존성으로 간단히 설정할 수 있다.

### 쉬운 의존성 관리
- spriing-boot-starter-* 모듈
- 예를 들어 spring-boot-starter-web은 web을 구축하기 위해 필요한 의존성을 맞는 버전을 찾아가지고 와서 등록해 준다.
- io-dependency-management는 직접 적는 의존성을 관리해준다. 버전명을 적지 않아도 호환성에 맞게 알아서 버전을 관리해준다. 버전을 직접 명시하면 명시한 버전으로 오버라이딩한다.

### 내장 웹 서버
- 애플리케이션 배포 시 복잡함을 피할 수 있게 한다.
- spring을 이용하면 톰캣 등의 WAS 설정을 해주어야 한다.
- spring boot는 톰캣과 같은 내장 서버를 가지고 있다.

## 참고 자료
- spring vs spring boot로 검색
- https://server-engineer.tistory.com/739
- https://www.youtube.com/watch?v=OdpPvdB7qZY&ab_channel=우아한Tech

## 더 읽어볼 내용
- https://ssoco.tistory.com/66 우테코 발표자가 쓴 블로그
- https://jjunii486.tistory.com/84 IoC를 설명한 블로그
