---
layout  : wiki
title   : 
summary : 
date    : 2022-08-23 01:26:41 +0900
updated : 2022-08-23 15:02:08 +0900
tags    : 
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# 자바 클래스패스 설정 어떻게 하나?

자바 프로그램을 실행하려면 우선 자바 파일(`.java`)에 코드를 짠다. `javac` 명령으로 이 자바 파일을 컴파일한 클래스 파일(`.class`)을 만든다. 마지막으로 `java` 명령으로 이 클래스 파일을 실행한다.

자바 프로그램을 실행하는 과정에서 사용하는 두 명령(`java`, `javac`)에서 자바 클래스패스 옵션을 설정한다.

단순한 경우는 클래스패스 옵션 설정이 간단하고, 클래스패스 옵션을 사용하지 않더라도 프로그램 실행에 문제가 없다. 하지만 패키지를 쓰거나 짠 코드에서 외부 라이브러리를 쓰는 경우 `javac`와 `java` 명령을 사용하면서 클래스패스 설정을 어떻게 할지 확실히 몰라서 여러 번 당황했다. 왜 안되는지 알아보려고 자료를 조사하고, 실행 환경을 만들어 테스트해보고 나니 전보다 익숙해질 수 있었다. 또 잊어버렸을 때 읽어보기 위해 글로 정리하려고 한다.

참고. 글에 나온 테스트 실행 환경은 Mac OS Mojave의 bash이다.

### Case 1
#### 단순한 자바 파일인 경우 실행하기까지의 과정을 살펴본다.
1. 테스트 환경을 만든다.
- `/java/export` 디렉토리에 `MyClass.java`라는 자바 파일을 만든다. MyClass 클래스는 `Happy Coding!`이라는 문자열을 콘솔에 출력한다.
```
public class MyClass {
  public static void main(String[] args){
        System.out.println("Happy Coding!");
    }
}
```

2. 컴파일해 클래스 파일을 만든다.
- `MyClass.java` 안 프로그램을 실행하려면 먼저 javac 명령으로 클래스파일을 만든다.
```
$ javac MyClass.java
```
- 명령 실행 후 같은 디렉토리에 MyClass.class 파일이 생성된다.

3. 클래스 파일을 실행한다.
- java 명령으로 MyClass 프로그램을 실행한다. 이때 프로그램 이름은 (.class 확장자를 제외한) 클래스의 이름을 넣는다.
```
$ java MyClass
Happy Coding!
```
- 실행에 성공했다.

### Case 2
#### 현재 위치에 클래스 파일이 있을 때 클래스패스 설정을 어떻게 할지 알아본다.
- java 명령을 실행할 때 -classpath 옵션으로 실행하고자 하는 클래스의 위치를 설정해주어야 한다.
- 하지만 '방금 위에서는 아무 옵션도 설정하지 않았는데'라는 의문이 들 수 있다.  그렇다. Case 1에서 클래스 패스를 따로 설정해주지 않았다. 왜냐하면 클래스 패스 옵션을 명시하지 않더라도 기본값으로 설정되기 때문이다.
- 아무 옵션을 주지 않았을 때는 사용자가 명령을 실행하는 시점의 위치를 클래스패스로 설정한다. 즉 Case 1에서 실행했던 클래스패스를 명시하지 않은 `$ java MyClass`는 사실 다음 명령과 같다.
```
$ java -classpath . MyClass
Happy Coding! # 이렇게 해도 프로그램이 실행된다.
```

### Case 3. 현재 위치에 클래스 파일이 없을 때 클래스패스 설정
- 만약 사용자의 현재 위치가 `MyClass.class` 파일이 있는 곳이 아닌 다른 곳이라면, 클래스 패스를 설정하지 않을 경우 오류가 난다. 왜냐하면 java 명령을 실행할 때 클래스 패스를 명시하지 않으면 명령을 실행하는 디렉토리로 클래스 패스가 설정되는데, 그곳엔 실행하려는 클래스 파일이 없기 때문이다.
- 실행하려는 클래스 파일 위치 이외의 곳에서 클래스 파일을 실행하기 위해선 `java` 명령 실행 시 클래스 패스 옵션으로 클래스 파일의 위치를 명시적으로 적어야 한다.
- 실행하고자 하는 클래스 MyClass.class 파일은 /java/export에 있으므로 이 경로를 클래스패스로 설정해준다.
```
$ cd /somewhere/else
$ java -classpath /java/export MyClass
Happy Coding!
```

### Case 4
#### 패키지를 사용하는 자바 파일을 실행할 경우를 알아본다.
- 패키지를 사용하면 조금 복잡해진다.

1. 테스트를 위해 실행 환경을 만든다.
- 패키지를 사용하는 소스를 만든다. 이전까지 썼던 MyClass.java 파일 내용을 조금 바꾸자.
- `MyClass.java` 파일 첫 줄에 패키지 선언부를 추가한다.
```
package com.sesoon;

public class MyClass {
  public static void main(String[] args){
        System.out.println("Happy Coding!");
    }
}
```
- 이제 `MyClass.java` 파일은 `/java/export`에 위치하고 `com.sesoon` 패키지에 포함된다.


2. 컴파일해 클래스 파일을 만들고, 실행한다.
- 이전과 같이 컴파일하고 실행하면 클래스를 찾을 수 없다는 오류가 나온다.
```
$ javac MyClass.java
$ java -cp /java/export MyClass
Error: Could not find or load main class MyClass
Caused by: java.lang.NoClassDefFoundError: com/sesoon/MyClass (wrong name: MyClass)
```

3. 문제 분석
#### 왜 오류가 나나?
1) 첫째로 클래스 이름이 잘못되었다. `MyClass.java`를 컴파일한 클래스의 이름은 `MyClass`가 아니라 `com.sesoon.MyClass`이기 때문이다. 자바 파일에서 패키지를 선언했다면, 클래스 이름에 패키지 이름도 포함되어야 한다.
2) 둘째로 디렉토리 구조가 잘못되었다. 패키지를 선언했다면 패키지 하위 디렉토리가 있어야 하고, 그중 가장 하위 디렉토리 안에 클래스 파일이 있어야 한다. 지금 예에서는 `/com/sesoon/MyClass.class`의 구조로 `MyClass` 클래스 파일이 위치해야 이 클래스를 실행할 수 있다.

#### 그러면 어떻게 해야 할까?
- `javac`로 자바 파일을 컴파일하면 클래스 파일은 기본적으로 자바 파일과 같은 경로에 만들어진다. 그래서 이제까지 만들어낸 클래스 파일은 자바 파일과 동일한 디렉토리에 만들어졌다. `d` 옵션을 사용해서  클래스 파일을 만들 디렉토리를 정할 수도 있다.
- `javac -d 만든-클래스-파일을-넣을-경로 자바-파일-이름` 형식으로 사용한다.

```
$ mkdir /java/export/classes
$ javac -d /java/export/classes MyClass.java
```

- `/java/export/classes` 디렉토리 안에 `MyClass` 클래스 파일이 만들어진다.

#### 그런데 패키지 하위 디렉토리는 어떻게 만드나?
- 명령 실행 후 폴더 구조를 확인해보면, 패키지 하위 디렉토리가 `classes` 디렉토리 아래에 만들어진 것을 확인할 수 있다.
```
.
├── MyClass.java
└── classes
    └── com
        └── sesoon
            └── MyClass.class
```
- `javac`의 `-d` 옵션으로 클래스를 만들 위치를 설정하면, 클래스 파일뿐 아니라 패키지 하위 디렉토리도 만든다(해당 디렉토리가 없을 경우).

#### 클래스 파일 실행
- 이제 `MyClass`를 실행할 수 있다.
- 이때 클래스패스는 기본적으로 패키지 하위 디렉토리가 있는 루트 디렉토리를 지정해야 한다. 즉 패키지(`com.sesoon.MyClass`) 가장 앞 요소(여기서는 `com`)를 품는 디렉토리를 말한다. 따라서 여기서는 `/java/export/classes` 디렉토리가 루트 디렉토리가 된다.
- 또한 클래스 이름은 패키지 이름을 모두 포함해야 한다.

```
$ java -cp /java/export/classes com.sesoon.MyClass
Happy Coding!
```

### Case 5
- 만든 자바 파일이 외부 라이브러리에 의존하는 경우를 알아본다.
- 만든 자바 파일이 외부 파일(`.class` 파일, `.jar` 파일 등)에 의존하는 경우가 있다.
- 그럴 경우, `javac` 명령으로 클래스 파일을 생성할 때 (자바 파일을 클래스 패스에 명시하는 것에 더해) 클래스 패스에 의존하는 외부 파일의 경로를 추가해줘야 한다.
- 예를 들어 `MySQLDriverLoader.java`이라는 자바 파일이 `jdbc` 패키지에 포함되고, `servlet-api.jar` 안의 클래스를 사용하고 있다면(의존하고 있다면), 이 자바 파일을 컴파일할 때는 `-d` 옵션으로 패키지 하위 디렉토리를 만들어야 하고, `-cp` 옵션으로 (자바 파일이 위치한 디렉토리 경로와) 의존하고 있는 `.jar` 파일의 경로를 명시해 주어야 한다.
- 이때 의존하고 있는 요소의 경로를 클래스 패스의 앞쪽에 명시해 주어야 한다.

```
> javac -d /usr/local/tomcat8/webapps/chap14/WEB-INF/classes -cp /usr/local/tomcat8/lib/servlet-api.jar:/usr/local/tomcat8/webapps/chap14/WEB-INF/src/ /usr/local/tomcat8/webapps/WEB-INF/src/jdbc/MySQLDriverLoader.java 
> tree /usr/local/tomcat8/webapps/chap14/WEB-INF/classes/
classes/
└── jdbc
    └── MySQLDriverLoader.class
```

### 참고자료
- https://docs.oracle.com/javase/8/docs/technotes/tools/unix/java.html
- https://docs.oracle.com/javase/8/docs/technotes/tools/unix/javac.html
- https://effectivesquid.tistory.com/entry/자바-클래스패스classpath란?category=658328
- https://stackoverflow.com/a/18093929

