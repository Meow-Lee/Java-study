---
description: About Networking
---

# Networking

### 1. 네트워킹

* 두 대 이상의 컴퓨터를 케이블로 연결하여 네트워크를 구성하는 것

#### 클라이언트 / 서버

* 서버 -> 서비스를 제공하는 컴퓨터
* 클라이언트 -> 서비스를 사용하는 컴퓨터
* 서버가 제공하는 서비스 종류에 따라, 파일 서버, 메일 서버, 어플리케이션 서버 등으로 나눌 수 있음

#### IP 주소

* 컴퓨터(호스트)를 구별하는데 사용되는 고유한 값으로, 인터넷에 연결된 모든 컴퓨터는 IP 주소를 가짐

#### URL(Uniform Resource Locator)

* 인터넷에 존재하는 여러 서버들이 제공하는 자원에 접근할 수 있는 주소
* '프로토콜://호스트명:포트번호/경로명/파일명?쿼리스트링#참조' 의 형태

> http://www.codechobo.com:80/sample/hello.html?referer=codechobo#index1 과 같다
>
> * 프로토콜 -> 자원에 접근하기 위해 서버와 통신하는데 사용되는 통신 규약
> * 호스트명 -> 자원을 제공하는 서버의 이름
> * 포트번호 -> 통신에 사용되는 서버의 포트번호
> * 경로명 -> 접근하려는 자원이 저장된 서버상의 위치
> * 파일명 -> 접근하려는 자원의 이름
> * 쿼리 -> URL에서 '?' 이후의 부분
> * 참조 -> URL에서 '#' 이후의 부분

```java
// URL 객체 생성
URL url = new URL("http://~ ");
URL url = new URL("www.~", "/sample/hello.html");
URL url = new URL("http", "www~", 80, "/sample~");
```

#### URLConnection

* 어플리케이션과 URL 간의 통신 연결을 나타내는 클래스의 최상위 클래스로 추상 클래스
* 상속받아 구현한 클래스로는 HttpURLConnection 과 JarURLConnection

### 2. 소켓 프로그래밍

