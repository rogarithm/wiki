---
layout  : wiki
title   : 
summary : 
date    : 2022-08-23 01:41:15 +0900
updated : 2022-08-23 01:41:27 +0900
tags    : 
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# 지역 변수와 인스턴스 변수의 차이점

책(자바의 신 75쪽)에서는 지역 변수가 중괄호 안에서 선언된 변수이고, 인스턴스 변수는 클래스 안 메서드 밖에 선언된 변수라고 설명.   
그런데 지역 변수가 선언되는 경계인 중괄호가 어디에 있는 중괄호인지는 따로 설명하고 있지 않음. 확인해보니 뒤에 예로 든 코드에서도 지역 변수를 메서드 안에서만 선언하기는 하지만 명시적으로 설명이 된 곳은 없었음.   
그렇다면 인스턴스 변수는 클래스를 열고 닫는 중괄호 안에 있으므로 지역 변수로도 볼 수 있지 않을까 하는 생각. 두 변수가 서로 다르다면, 둘을 어떻게 구분할 수 있을까 하는 의문.

아래 예시 코드에서 생각해보면 instanceVariable을 지역 변수로도 볼 수 있지 않을까?
```java
public class VariableTypes {
	int instanceVariable;
	static int classVariable;
	public void method(int parameter) {
		int localVariable;
	}
}
```

그래서 Oracle의 [java tutorial](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/variables.html)에 지역 변수를 설명하는 부분을 찾아봄:   
**Local Variables**: Similar to how an object stores its state in fields, **a method will often store its temporary state in local variables**. The syntax for declaring a local variable is similar to declaring a field (for example, int count = 0;). **There is no special keyword designating a variable as local; that determination comes entirely from the location in which the variable is declared — which is between the opening and closing braces of a method**. As such, **local variables are only visible to the methods in which they are declared; they are not accessible from the rest of the class**.

굵게 표시한 부분을 정리해보면:
- 메서드는 지역 변수에 자신의 일시적 상태를 담아둔다.
- 지역 변수를 가리키는 키워드는 따로 없다. 변수가 지역 변수임을 결정하는 건 해당 변수가 메서드를 열고 닫는 중괄호 사이에 선언되어 있는가이다.
- 지역 변수의 유효 범위는 자신이 선언된 메서드 안이며, 해당 클래스의 나머지 부분에서는 접근할 수 없다.

여기서는 지역 변수가 메서드를 열고 닫는 중괄호 사이에 선언된 변수라고 하고 있음. 따라서 지역 변수는 메서드 안에서만 선언할 수 있는 변수이고, 인스턴스 변수는 메서드 밖에, 클래스 안에 선언되는 변수로 분명한 차이가 있다.
