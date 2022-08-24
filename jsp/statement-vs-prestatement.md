---
layout  : wiki
title   : 
summary : 
date    : 2022-08-24 21:43:09 +0900
updated : 2022-08-24 21:43:14 +0900
tags    : 
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# statement vs. prestatement

## sql 쿼즈 실행 단계
1. 쿼리 문법이 틀린 곳은 없는지, 쿼리에 적힌 테이블이나 칼럼이 DB에 있는지 확인한다.
2. 컴파일한다.
3. 쿼리를 실행하기 위한 여러 가지 방법 중 가장 나은 방법을 고른다.
4. 전단계에서 고른 방법을 저장한다.
5. 쿼리를 실행한다.

## 왜 PreStatement가 성능면에서 더 뛰어난가?
- Statement는 쿼리를 실행할 때마다 위 다섯 단계를 거친다.
- 반면 PreStatement는 처음 실행할 때에만 첫 세 단계를 거쳐 가장 나은 방법을 저장한다.
- 다음으로 쿼리를 실행하기 전 사용자가 입력한 값을 PreStatement 안 ?와 교체한다.
- 이후 PreStatement에서 같은 쿼리를 실행하면 첫 세단계는 거치지 않는다.

## 왜 PreStatement가 보안면에서 더 뛰어난가?
- 이미 컴파일된 PreStatement는 다시 컴파일되지 않는다.
- 외부에서 SQL injection하려면 쿼리의 원래 논리를 수정해야 하는데, PreStatement는 이미 컴파일되어 있어서 논리를 수정할 수 없다.

## 참고 자료
https://velog.io/@jsj3282/Statement보다-Preparedstatement을-사용해야-하는-이유성능-보안-측면
