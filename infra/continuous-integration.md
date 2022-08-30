---
layout  : wiki
title   : 
summary : 
date    : 2022-08-28 21:47:34 +0900
updated : 2022-08-30 14:23:58 +0900
tags    : 
toc     : true
public  : true
parent  : [[infra]]
latex   : false
---
* TOC
{:toc}

# 

## CI의 정의
Continuous Integration. 새로운 코드 변경 사항이 정기적으로 빌드 및 테스트되어 공유 저장소에 통합되는 것을 뜻한다.

애플리케이션 사용자의 요구사항이 생기면 소스를 추가하거나 변경해서 요구사항을 들어준다. 소스를 변경하면 변경 사항을 커밋해서 애플리케이션 소스 저장소에 있는 소스의 버전 업데이트를 한다. 이때 변경된 소스가 기존 소스에 병합되었을 때 충돌을 일으키지는 않는지, 문제 없이 빌드되는지, 테스트를 통과하는지를 확인해야 한다. CI는 이러한 작업을 자동화해서 여러 사람이 기존 애플리케이션 소스를 자주 변경하더라도 애플리케이션 소스에 빠르게 변경 사항을 반영할 수 있도록 하는 작업이다.


## CI 체크항목
- 하루에 적어도 한 번씩 메인 브랜치에 커밋하는가?
- 테스트 모음이 있는가? 제품에 적용하기 전에 테스트를 하는가? 테스트를 통과했다면 문제 없이 통합될 수 있을 거라는 확신을 주는가?
- 제품에 적용하기까지의 파이프라인에 문제가 생기면 10-20분 안에 고칠 수 있는가?

## 참고 자료
https://www.youtube.com/watch?v=aoMfbgF2D_4
https://artist-developer.tistory.com/24
https://www.youtube.com/watch?v=sIPU_VkrguI

## 더 볼 자료
https://martinfowler.com/delivery.html
