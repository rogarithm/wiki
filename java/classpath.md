---
layout  : wiki
title   : 
summary : 
date    : 2022-08-23 01:26:41 +0900
updated : 2022-08-28 20:38:45 +0900
tags    : 
toc     : true
public  : true
parent  : 
latex   : false
---
* TOC
{:toc}

# 자바 클래스패스 설정 어떻게 하나?

자바 프로그램을 실행하려면 우선 자바 파일(`.java`)에 코드를 짠다. 그 다음 `javac` 명령으로 자바 파일을 컴파일해 클래스 파일(`.class`)을 만든다. 마지막으로 `java` 명령으로 클래스 파일을 실행하면 프로그램이 실행된다.

자바 프로그램을 실행하는 과정에서 사용한 두 명령(`java`, `javac`)에는 클래스패스 옵션이 있다. 단순한 경우는 클래스패스 옵션 설정이 간단하고, 클래스패스 옵션을 사용하지 않더라도 프로그램 실행에 문제가 없다. 하지만 패키지를 쓰거나 짠 코드에서 외부 라이브러리를 쓰는 등 더 복잡한 경우 `javac`와 `java` 명령을 사용하면서 클래스패스 설정을 잘 해주지 않으면 프로그램 실행에 애를 먹는 경우가 생긴다. 나 역시 어떻게 할지 확실히 몰라서 여러 번 당황한 경험이 있어 어떻게 해야하는지 알아보았다. 자료를 조사하고, 실행 환경을 만들어 테스트해보고 나니 전보다 익숙해질 수 있었다. 이 내용에 대해 궁금한 분 뿐만 아니라 내도 잊었을 때 읽어보기 위해 글로 정리하려고 한다.

참고로 테스트는 MacOS Mojave의 bash에서 실행했다.

## 단순한 경우의 실행 환경을 만들고 실행해본다.
### 클래스 패스를 설정하지 않는 경우
1. 테스트 환경을 만든다.
- `/java/export` 디렉토리에 `MyClass.java`라는 자바 파일을 만든다.
```
$ mkdir /java/export && cd /java/export
$ touch MyClass.java
$ vim MyClass.java
```
- MyClass.java 파일을 작성한다. MyClass 클래스는 `Happy Coding!`이라는 문자열을 콘솔에 출력한다.
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
```
$ ls /java/export
MyClass.class	MyClass.java
```

3. 클래스 파일을 실행한다.
- java 명령으로 MyClass 프로그램을 실행한다. 이때 프로그램 이름은 (.class 확장자를 제외한) 클래스의 이름을 넣는다.
```
$ java MyClass
Happy Coding!
```

### 현재 위치에 클래스 파일이 없을 때 클래스 패스 설정
- java 명령을 실행할 때 -classpath 옵션으로 실행하고자 하는 클래스의 위치를 설정해주어야 한다.
- 하지만 위에서 실행할 때는 클래스패스 옵션을 설정하지 않았다. 왜냐하면 클래스패스 옵션을 명시하지 않더라도 설정되는 기본값이 있기 때문이다. 기본값으로 설정되는 클래스패스라도 문제 없는 경우라면 클래스패스를 설정하지 않더라도 프로그램을 실행할 수 있다.
- 아무 옵션을 주지 않았을 때는 사용자가 명령을 실행하는 시점의 위치를 클래스패스로 설정한다. 따라서 (사용자의 현재 위치가 `/java/export`라고 했을 때) 위에서 실행했던 클래스패스를 명시하지 않은 `$ java MyClass`는 다음 명령과 같다.
```
$ java -classpath . MyClass
Happy Coding!
```
- 실행하고자 하는 프로그램의 클래스 파일 위치가 클래스패스와 같으면 프로그램을 실행할 수 있다.

## 비교적 복잡한 경우의 실행 환경을 만들고 실행해본다.
### 현재 위치에 클래스 파일이 없을 때 클래스패스 설정
- MyClass 프로그램을 실행하려고 할 때 사용자의 위치가 `MyClass.class` 파일이 있는 곳이 아니라면, 클래스패스를 설정하지 않을 경우 오류가 난다. 왜냐하면 `java` 명령을 실행할 때 클래스패스를 명시하지 않으면 명령을 실행하는 디렉토리(즉 사용자의 위치)로 클래스패스가 설정되는데, 그곳엔 실행하려는 `MyClass.class` 파일이 없기 때문이다.
```
$ cd /somewhere/else
$ java -classpath /java/export MyClass
Error: Could not find or load main class MyClass
Caused by: java.lang.ClassNotFoundException: MyClass
```
- 그러면 어떻게 해야할까? `java` 명령 실행 시 클래스 파일 위치와 사용자의 위치가 다르다면, 클래스 패스 옵션에 클래스 파일의 위치를 명시해야 한다.
- 실행하고자 하는 클래스 MyClass.class 파일은 /java/export에 있으므로 이 경로를 클래스패스로 설정해준다.
```
$ cd /somewhere/else
$ java -classpath /java/export MyClass
Happy Coding!
```

### 패키지를 사용하는 자바 파일의 클래스패스 설정
- 자바 파일 상단에 `package ...`와 같은 패키지 선언이 있는 경우, 이전과 같은 방식으로는 프로그램을 실행할 수 없다.

1. 테스트를 위해 실행 환경을 만든다.
- 패키지를 사용하는 소스를 만든다. 이전까지 썼던 `MyClass.java` 파일 내용을 조금 바꾸자.
- `MyClass.java` 파일 첫 줄에 패키지 선언부를 추가한다.
```
package com.sesoon;

public class MyClass {
  public static void main(String[] args){
        System.out.println("Happy Coding!");
    }
}
```
- 이제 `MyClass.java` 파일은 이전과 동일하게 `/java/export`에 위치하지만 `com.sesoon` 패키지에 포함된다.


2. 컴파일해 클래스 파일을 만들고, 실행한다.
- 이전과 같이 컴파일하고 실행하면 클래스를 찾을 수 없다는 오류가 나온다.
```
$ cd /java/export
$ javac MyClass.java
$ java -cp /java/export MyClass
Error: Could not find or load main class MyClass
Caused by: java.lang.NoClassDefFoundError: com/sesoon/MyClass (wrong name: MyClass)
```

3. 문제 분석
#### 왜 오류가 나나?
1) 첫째로 클래스 이름이 잘못되었다. 자바 파일에서 패키지를 선언했다면, 클래스 이름에 패키지 이름도 포함하기 때문이다. 예시로 든 `MyClass.java`의 경우, 클래스의 이름은 `MyClass`가 아니라 `com.sesoon.MyClass`가 된다.
2) 둘째로 디렉토리 구조가 잘못되었다. 패키지를 선언했다면 패키지 하위 디렉토리\*가 있어야 하고, 그중 가장 하위 디렉토리 안에 클래스 파일이 있어야 한다. 예시에서는 `com/` 디렉토리와 그 안에 위치한 `sesoon/` 디렉토리가 있어야 하고, 가장 하위 디렉토리인 `sesoon/` 디렉토리 안에 `MyClass.class` 파일이 있어야 프로그램을 실행할 수 있다.
- 클래스패스는 기본적으로 패키지 하위 디렉토리가 있는 루트 디렉토리를 지정해야 한다. 즉 패키지(`com.sesoon.MyClass`) 가장 앞 요소(여기서는 `com`)를 품는 디렉토리를 말한다. 따라서 여기서는 `/java/export` 디렉토리가 루트 디렉토리가 된다.
```
.
└── com
    └── sesoon
        └── MyClass.class
```
- \* 패키지 하위 디렉토리? 패키지 구분자로 패키지 이름을 나눈 각 요소를 이름으로 하는 디렉토리. 예시로 든 MyClass.java에는 com.sesoon 패키지가 선언되어 있다. 패키지 구분자인 '.' 문자로 패키지 이름을 나누면 각각 com, sesoon이 된다.

#### 그러면 어떻게 해야 할까?
- 별다른 옵션 없이 `javac`로 자바 파일을 컴파일하면 클래스 파일은 자바 파일과 같은 경로에 만들어진다. 패키지가 선언된 자바 파일이라면 실행되지 않으므로 패키지 하위 디렉토리를 만들어주면 된다.
- 단순하게 사용자가 직접 만들어주는 방법이 있다.
```
$ cd /java/export
$ mkdir -p classes/com/sesoon
$ mv MyClass.class classes/com/sesoon
```

- 하지만 위 방법은 번거롭다. 패키지 하위 디렉토리를 일일이 만들어야 하고, 그 중 가장 하위 디렉토리로 클래스 파일을 옮겨야 하기 때문이다. 다행히 `javac` 명령은 클래스 파일을 만들 디렉토리를 정하면 그 아래에 패키지 하위 디렉토리를 자동으로 만들어주고(해당 디렉토리가 없을 경우) 그 중 가장 하위 디렉토리에 컴파일한 클래스 파일을 넣어주는 `-d` 옵션을 제공한다.
- `-d` 옵션은 `javac -d 만든-클래스-파일을-넣을-경로 자바-파일-이름` 형식으로 사용하면 된다.
```
$ mkdir /java/export/
$ javac -d /java/export/ MyClass.java
```

- 명령 실행 후 폴더 구조를 확인해보면, 패키지 하위 디렉토리가 만들어졌고, 그 중 가장 하위 디렉토리 안에 클래스 파일이 들어간 것을 확인할 수 있다.
```
.
├── MyClass.java
└── com
    └── sesoon
        └── MyClass.class
```

- 이제 `MyClass`를 실행할 수 있다.
```
$ java -cp /java/export com.sesoon.MyClass
Happy Coding!
```

### 외부 라이브러리를 사용하는 경우 클래스패스 설정
- 만든 자바 파일이 외부 파일(`.class` 파일, `.jar` 파일 등)에 의존하는 경우가 있다.
- 그럴 경우, `javac` 명령으로 클래스 파일을 생성할 때 (자바 파일을 클래스패스에 명시하는 것에 더해) 클래스 패스에 의존하는 외부 파일의 경로를 추가해줘야 한다.
- 예를 들어 `MySQLDriverLoader.java`이라는 자바 파일이 `jdbc` 패키지에 포함되고, `servlet-api.jar` 안의 클래스를 사용하고 있다면(의존하고 있다면), 이 자바 파일을 컴파일할 때는 `-d` 옵션으로 패키지 하위 디렉토리를 만들어야 하고, `-cp` 옵션으로 (자바 파일이 위치한 디렉토리 경로와) 의존하고 있는 `.jar` 파일의 경로를 명시해 주어야 한다.
- 이때 의존하고 있는 요소의 경로를 클래스패스의 앞쪽에 명시해 주어야 한다.

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

