---
description: Docker and Container
---

# Docker와 Container 개념

## Docker

* 컨테이너 기반의 오픈소스 가상화 플랫폼
* 어떤 프로그램을 외부 환경과 격리시켜 구동할 수 있도록 해주는 소프트웨어
* 즉, 어떤 프로그램을 개발할 때 OS와 같이 환경에 구애받지 않고 개발할 수 있도록 해주는 기술

## Container

* OS상에 논리적인 영역(컨테이너)을 구축하고, 애플리케이션이 작동할 때 필요한 요소들을 모아 별도의 서버처럼 동작하는 것
* 쉽게 말해, Window 64bit를 설치할 때 시스템 요구사항을 맞춰야하고, 윈도우를 Mac에 설치할 때 제한사항이 있는데 이러한 것들처럼 제한사항에 구애받지 않고 사용할 수 있도록 하는 기술
* 필수 요소만으로 구성되서 오버헤드가 적음
* 예를 들어, 이전에 사용하던 VM은 말 그대로 운영체제를 설치하는 개념이었지만, Docker는 VM, 즉 OS의 구성요소 몇개만 가져와서 OS처럼 보이게 하여 돌아갈 수 있도록 함

<figure><img src="../.gitbook/assets/Docker 구조.png" alt=""><figcaption><p>Docker Container</p></figcaption></figure>

* OS위에 Docker가 올라가고 애플리케이션들이 컨테이너화되어 올라가있는 형식
