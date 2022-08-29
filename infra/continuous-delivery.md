---
layout  : wiki
title   : 
summary : 
date    : 2022-08-28 21:27:27 +0900
updated : 2022-08-28 22:04:15 +0900
tags    : 
toc     : true
public  : true
parent  : [[infra]]
latex   : false
---
* TOC
{:toc}

# 
## 상황
프로그램이 있다. 개발자는 작은 변화를 프로그램에 가한다. 변화가 적용된 새 버전의 프로그램을 만들려면 integration이 필요하다. 개발자 기기에서 작동할지라도, 다른 개발자, 사용자 기기에서도 마찬가지인지 아직 알 수 없다.

그렇기 때문에 다양한 수준의 테스트를 한다. 컴파일이 되는가, 단위 테스트를 통과하는가, 통합 테스트는? 변화를 적용했을 때 프로그램 성능이 떨어지지는 않는지 성능 테스트도. 개발자가 작업한 것이 좋은 변화를 주는지 확신할 수 있을 때까지. 이러한 것을 배포 파이프라인이라고 한다.

continuous delivery는 이러한 과정이 어떠한 작은 변화에도 적용되어 확신을 가지고 제품을 사용자에게 제공할 수 있는지에 대한 것이다.

continuous deployment는 계속해서 새로운 기능을 추가해야 하는 것이고, continuous delivery는 계속해서 새로운 기능을 추가할 수 있는 것이다. delivery일 경우, 회사의 정책이 매 2주마다 새로운 기능을 추가하는 것이라고 한다면, 매일 하는 것은 아니다. 하지만 만약 회사에서 원한다면 그렇게 할 수 있는 확신을 가지고 있다.

자동화할 요소들은 빌드, 배포, 테스트가 있다.

## 왜 사용하는가?
리스크를 줄이기 위해서. 제품에 적용해야 할 변화가 크면 클 수록, 그 과정에서 문제가 생길 가능성이 커진다. 반면 적용해야 할 변화가 작으면 작을수록, 잘못될 가능성은 작아진다. 문제가 생겼더라도 그 문제가 작다면 원인을 찾기도, 고치기도 수월하다. 최악의 경우에도, 전 버전으로 롤백하면 된다.

개발자가 변경한 것이 실제로 어떤 영향을 주는지를 반영한다. 실제와 완전히 동일하진 않겠지만, 개발자가 자기 자리에서 작동하는 것을 보고 "다 됐어요" 라고 말하는 것 보다는 더 실제와 비슷할 것이다.

사용자 피드백을 얻을 수 있다. 좋은 피드백이든 나쁜 피드백이든, 변경한 기능을 사용자가 사용해보고 피드백을 준다면, 그 피드백을 가지고 제품을 어떤 방향으로 개발해나갈지 결정하는 데 사용할 수 있다.

## 참고 자료
https://www.youtube.com/watch?v=aoMfbgF2D_4

## 더 볼 자료
https://martinfowler.com/delivery.html