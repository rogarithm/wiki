---
layout  : wiki
title   : 
summary : 
date    : 2022-08-23 01:07:30 +0900
updated : 2022-08-23 01:14:33 +0900
tags    : 
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# 
 
`mkdir collection && cd collection` 대신에 `mkdir collection && cd $_`와 같이 쓸 수 있다.

man bash에 관련된 설명을 볼 수 있다.
dd
Parameters - special parameters - _
At shell startup, set to the absolute pathname used to invoke the shell or shell script being executed as passed in  the  envi-
ronment  or  argument  list.  Subsequently, expands to the last argument to the previous command, after expansion.  Also set to
the full pathname used to invoke each command executed and placed in the environment exported to that command.   When  checking
mail, this parameter holds the name of the mail file currently being checked.

_는 마지막 명령의 인자를 의미한다.

\*  `mkdir collection && cd $_` 이 오류를 낸다면 `mkdir collection && cd "$_"`와 같이 quoting해주면 된다.

 참고 자료: https://unix.stackexchange.com/questions/125385/combined-mkdir-and-cd
 
