---
layout  : wiki
title   : 
summary : 
date    : 2022-08-23 01:50:42 +0900
updated : 2022-08-24 22:14:25 +0900
tags    : 
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# 자원을 왜 순서대로 열고 그 반대 순서로 닫아야 할까?
여러 종류의 자원을 열면, 다 쓴 자원은 닫아야 한다 

- 멘토링할 때 들었던 이유는 메모리 누수 문제였다. 정확히 어떤 건지는 몰랐다.
- 인터넷으로 찾아보니 왜 반대 순서로 닫아야 하는지는 찾을 수 없었고, 다른 주의점을 찾을 수 있었다. 

## Java 7 이상 버전에서 try-with-resources를 제공한다.
- try-with-resources를 쓰면 자동으로 올바른 순서에 맞춰 자원을 닫아준다.

## try-catch-finally를 쓸 경우 닫아야 하는 자원마다 try-catch로 닫아야 한다.
```
finally {
    if(rs != null) try { rs.close();} catch(SQLException ex) {}
    if(stmt != null) try { stmt.close();} catch(SQLException ex) {}
    if(conn != null) try { conn.close();} catch(SQLException ex) {}
}
```

- 다음과 같이 모든 자원을 try 블록 하나에서 닫는다면, 앞의 자원을 닫다가 예외가 발생하면 그 뒤에 있는 자원이 닫히지 않은 채로 try 블록을 빠져나간다.
```
finally {
	try {
		if(rs != null) rs.close();
		if(stmt != null) stmt.close(); 
		if(conn != null) conn.close();
	} catch(SQLException ex) {}
}
```

바로 위 코드에서 rs 자원을 닫다가 예외가 나면 stmt와 conn이 닫기지 않은 상태로 try 블록이 끝나게 된다. 닫히지 않은 자원은 메모리 누수의 원인이 되며, 메모리 누수가 누적되면 프로그램이 멈추게 될 수 있다.

- 위 두 내용은 자원 닫기와 연관되어 있기는 하지만 왜 역순으로 닫는지를 알려주지는 않는다.

## 연 순서의 역순으로 자원을 닫아야 한다
- 먼저 선언한 자원을 먼저 닫으면 그 뒤에 선언했던 자원이 문제 없이 닫힐지 보증할 수 없다.
- 예를 들어 다음 코드와 같이 (늦게 연 자원) connection을 먼저 닫는다고 하면 (뒤에 연 자원) statement가 닫힐지를 보증할 수 없다.
```
finally {
	 if(conn != null) try { conn.close();} catch(SQLException ex) {}
	 if(rs != null) try { rs.close();} catch(SQLException ex) {}
	 if(stmt != null) try { stmt.close();} catch(SQLException ex) {}
}
```

- 이상적인 환경에서는 굳이 statement와 result set을 닫지 않더라도 connection을 닫는 것으로 충분하다. 하지만 실제로 만드는 소프트웨어에서는 statement가 재사용될 수도 있고, 여러 statement가 connection 하나 위에서 실행될 수도 있다. 어떤 자원이 더 이상 필요하지 않게 되면 그 자원을 바로 닫아야 예상하지 못한 원인을 배제할 수 있다.


## 자원 닫는 순서에 이유가 있는지 고민해봤다.
- 자원 여러 개를 초기화할 때 닫는 순서에 어떤 이유가 있는가?
- 찾아본 자료를 보면 자원 여러 개를 여는 경우, 자원이 서로 연관되어 있기 때문에 닫는 순서가 여는 순서의 역순이 아니면 문제가 생길 수 있는 것 같다. 그러면 자원을 여러 개를 여는 경우 각 자원이 서로 어떻게 연관되는지(왜 이 순서로 초기화하는지) 알면 닫는 순서가 왜 그런지도 알 수 있을 것 같다고 생각했다.
- 예상하기로 어떤 자원 A를 먼저 초기화하는 건 그 다음 자원 B를 초기화하려면 먼저 초기화된 자원 A를 이용해야 하기 때문이 아닐까? 그렇다면 A->B->C 순으로 초기화된 자원을 닫을 때 B를 먼저 닫으면 C가 닫혀지지 않은 채로 남아서 메모리 누수를 일으킬 수 있게 되는 게 아닌가와 같은 생각이 들었다.
- 찾아본 예 중 한 곳(jsp 책 저자인 최범균님 블로그)에서는 jdbc를 사용하는 경우 커넥션을 닫으면 하위 자원도 닫아주지만, 커넥션 풀을 사용하면 커넥션을 닫더라도 커넥션이 없어지는 것이 아니라 커넥션 풀로 돌아가기 때문에 하위 자원을 따로 닫아주지 않으면 그대로 있게 된다고 한다. 그래서 하위 자원이 닫히지 않은 상태로 메모리 누수를 유발한다.
- 더 일반적인 예를 찾고 싶어 java why initialize resultset after connection로 검색해봤지만 쓸만한 결과가 없었다.

## 참고 자료
- https://hogu-programmer.tistory.com/14
- https://blog.benelog.net/1898928.html
- https://stackoverflow.com/a/71704781
- https://javacan.tistory.com/entry/78
