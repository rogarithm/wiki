---
layout  : wiki
title   : 
summary : 
date    : 2022-08-23 20:37:06 +0900
updated : 2022-08-23 20:37:17 +0900
tags    : 
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# hash map

hash?
wikipedia에서 hash table을 찾아봤다.
hash table = hash map = dictionary
key를 value에 맵핑하는 데이터 구조. index(hash code)를 계산하기 위해 hash function을 쓰고, bucket(slot) 배열로 넣어서 원하는 값을 찾는다. 검색(lookup) 동안, key가 hashed되며 결과 hash는 원하는 값이 어디에 저장되어 있는지를 나타낸다.
Q1. index와 key는 다른가?
Q2. bucket(slot)은 value를 담고 있는 곳인가?


이상적인 해시 함수는 각 키를 유일한 bucket에 할당하지만, 대부분의 해시 테이블 디자인은 해시 함수가 완전하지 않기 때문에 해시 충돌이 일어날 수 있다. 해시 충돌은 해시 함수가 하나 이상의 키에 대해 같은 인덱스를 생성할 때 발생한다.

해시 검색의 시간 복잡도는 테이블에 저장된 요소의 갯수와 독립적이다. 다수의 해시 테이블 설계가 키-값 쌍을 삽입 및 삭제하는 동작이 매 동작마다 일정 상수값을 비용으로 하도록 한다.


