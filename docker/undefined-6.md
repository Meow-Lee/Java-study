---
description: 그림과 실습으로 배우는 도커 & 쿠버네티스
---

# 도커 컴포즈

## <도커 컴포즈>

## 도커 컴포즈란?

* 명령어에 익숙해져도 여러 개의 컨테이너로 구성된 시스템을 실행하려면 인자나 옵션도 많고 볼륨이나 네트워크도 설정해야하기 때문에 불편함
* 따라서, 시스템 구축과 관련된 명령어를 하나의 텍스트 파일(Compose file)에 기재해 명령어 한번에 시스템 전체를 실행하고 종료와 폐기까지 한번에 하도록 도와주는 도구임
* 즉, 도커 컴포즈를 사용하면 여러 개의 명령어를 하나의 정의 파일로 합쳐 실행할 수 있음

### 도커 컴포즈의 구조

* 도커 컴포즈는 시스템 구축에 필요한 설정을 .yml 포맷으로 기재한 정의 파일을 이용해 전체 시스템을 일괄 실행 또는 종료, 삭제 할 수 있는 도구
* 정의 파일에는 컨테이너나 볼륨을 어떠한 설정으로 만들지에 대한 항목이 기재되어 있으나, 도커 명령어는 아님(비슷함)
* up 커맨드는 docekr run 커맨드와 비슷한데, 정의 파일에 기재된 내용대로 이미지를 내려받고 컨테이너를 생성 및 실행
* down 커맨드는 컨테이너와 네트워크를 정지 및 삭제, 볼륨과 이미지는 삭제하지 않음
* 종료만 하고 싶다면 stop 커맨드 사용

### 도커 컴포즈와 Dockerfile 스크립트의 차이점

* 도커 컴포즈는 docker run 명령어를 여러 개 모아놓은 것과 같음
* 컨테이너와 주변 환경을 생성하고, 네트워크와 볼륨까지 설정할 수 있음
* Dockerfile 스크립트는 이미지를 만들기 위한 것으로 네트워크나 볼륨은 만들 수 없음
* 즉, 간단히 말해서 만드는 대상이 다르다고 할 수 있음
* 착각하면 안되는 점 : 쿠버네티스는 도커 컨테이너를 관리하는 도구인 만큼 여러 개의 컨테이너를 다루는 것과 관계가 깊은데, 도커 컴포즈와 혼동하면 안됨
* 쿠버네티스는 컨테이너를 관리하고, 도커 컴포즈는 컨테이너 생성 및 삭제만 하는 것으로 관리 기능은 없음

## <도커 컴포즈 설치 및 사용법>

* 도커 엔진과는 별개의 소프트웨어이지만, 윈도우나 mac에서는 이미 설치되어 있음(도커 데스크톱과 함께 설치됨)

## 도커 컴포즈 사용법

* 도커 컴포즈를 사용하려면 Dockerfile 스크립트로 이미지를 빌드할 때처럼 호스트 컴퓨터에 폴더를 만들고 이 폴더에 정의 파일을 배치
* 정의 파일의 이름은 docker-compose.yml 이라는 이름을 사용해야함
* 파일은 호스트 컴퓨터에 배치되지만 명령어는 똑같이 도커 엔진에 전달되며, 만들어진 컨테이너도 동일하게 도커 엔진 위에서 동작함
* 즉, 일일이 입력하던 명령어를 도커 컴포즈가 대신 입력해주는 역할을 하는 구조
* 정의 파일은 한 폴더에 하나만 있어야함
* 그래서 여러 개의 정의파일을 사용하려면 그 개수만큼 폴더를 만들어야 함
* 참고로, 도커 컴포즈에서 컨테이너가 모인 것을 서비스라고 부름

## <도커 컴포즈 파일을 작성하는 법>

## 도커 컴포즈 정의 파일의 내용 살펴보기

```docker
version: “3”
services:
 mysql000ex11:
  image: mysql:5.7
  networks:
   - wordpress000net1
  volumes:
   - mysql000vol11:/var/lib/mysql
  restart: always
  command: mysqld --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
  --default-authentiaction-plugin=mysql_native_password // mysql 8.0부터 넣어줘야함
  environment:
   MYSQL_ROOT_PASSWORD: myrootpass
   MYSQL_DATABASE: wordpress000db
   MYSQL_USER: wordpress000kun
   MYSQL_PASSWORD: kunpass
 wordpress000ex12:
  depends_on:
   - mysql000ex11
  image: wordpress
  networks:
   - wordpress000net1
  volumes:
   - wordpress000vol12:/var/www/html
  ports:
   - 8085:80
  restart: always
  environment:
   WORDPRESS_DB_HOST: mysql000ex11
   WORDPRESS_DB_NAME: wordpress000db
   WORDPRESS_DB_USER: wordpress000kun
   WORDPRESS_DB_PASSWORD: wkunpass
networks:
 wordpress000net1:
volumes:
 mysql000vol11:
 wordpress000vol12:
```

## 컴포즈 파일(정의 파일)을 작성하는 방법

* YAML 형식이고, 확장자는 .yml이며 텍스트 에디터로 작성하면 됨
* \-f를 사용해서 파일 이름을 지정하면 다른 이름을 사용해도 되지만 그렇지 않다면 정해진 이름을 사용해야함

### 컴포즈 파일 작성 방법

* 맨 앞에 컴포즈 버전을 적고, 그 뒤로 services, networks, volumes를 차례로 작성
* 도커 컴포즈와 쿠버네티스에서는 컨테이너의 집합체를 서비스라고 부름
* 작성 요령은 주 항목 -> 이름 추가 -> 설정과 같은 순서로 생각하면 쉬움
* YAML 형식에서는 공백에 따라 의미가 달라지므로 탭은 의미가 없고 공백 두개로 맨 처음 들여쓰기를 했다면 그 뒤로도 공백 두 개가 한단이 되도록 해야함
* depends\_on은 다른 서비스에 대한 의존 관계를 나타냄
* 컨테이너를 생성하는 순서나 연동 여부를 정의
* penguin컨테이너 정의에 depends\_on: -namgeuk이라면 namgeuk 컨테이너를  생성 후에 penguin컨테이너를 만들게 됨

## <도커 컴포즈 실행>

## 도커 컴포즈 커맨드

* 도커 엔진은 docker 명령을 사용했고, 도커 컴포즈는 docker-compose 명령을 사용함
* 주로 up, down, stop 을 사용

### 컨테이너와 주변 환경을 생성하는 docker-compose up 커맨드

* 컴포즈 파일의 경로는 -f 옵션을 사용해 지정함
* docker-compose -f \[정의파일경로] up \[옵션]

### 컨테이너와 네트워크를 삭제하는 docker-compose down 커맨드

* 컴포즈 파일의 내용을 따라 컨테이너와 네트워크를 종료 및 삭제
* docker-compose -f \[정의파일경로] down \[옵션]

### 컨테이너를 종료하는 docker-compose stop 커맨드

* 컨테이너 종료
* docker-compose -f \[정의파일경로] stop \[옵션]
