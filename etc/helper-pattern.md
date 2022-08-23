---
layout  : wiki
title   : 
summary : 
date    : 2022-08-23 20:46:44 +0900
updated : 2022-08-23 20:46:47 +0900
tags    : 
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# 
## helper pattern

출처: https://wiki.c2.com/?HelperPattern

type interface를 통해 제공하는 동작보다 더 많은 동작이 그 type에 필요할 때 이러한 동작을 보조 클래스나 모듈로 모은다.
객체 지향적 관점에서 잘못된 위치지만, 적어도 최선의 위치다.

### 결과
- type에 대한 동작이 두 곳으로 갈라진다. 반복하는 것보단 낫지만, 완벽하다고 보긴 어렵다.
- 동작이 추가로 필요했던 타입의 코드를 건드릴 필요가 없다.

### 단점
- 동작을 모은 클래스가 빠르게 비대해질 수 있다
- 다형성이 필요한 경우 타입뿐 아니라 helper 클래스도 고려해야 하므로 복잡해지며, helper 클래스를 사용해서 해결하려 한 문제보다 더 큰 문제를 가져오는 것이다.
- helper "pattern"이라고 불리는 이유는 문제에 대한 "가장 덜 냄새나는" 접근 방식이기 때문이다.
