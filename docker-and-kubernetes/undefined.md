# 도커 컨테이너

## 컨테이너 구조

* 컨테이너 레이어와 이미지 레이어로 구성
* 이미지를 컨테이너에 올리면 컨테이너 레이어가 생기고 Read/Write 가능
* 따라서 컨테이너를 구동하면서 발생하는 추가 또는 변경 사항은 컨테이너 레이어에서 이루어짐

<figure><img src="../.gitbook/assets/컨테이너 구조.png" alt=""><figcaption><p>컨테이너는 컨테이너 레이어와 이미지 레이어로 구분</p></figcaption></figure>

* 컨테이너 레이어 -> R/W 모두 가능한 계층으로 최상단 레이어에 추가됨
* 이미지 레이어 -> 읽기 전용 계층으로 다른 컨테이너와 공유할 수 있는 레이어
* 여러개의 컨테이너를 구동하면 컨테이너 레이어는 각기 다른 컨테이너가 공유하지 않는 계층이므로, 이미지 레이어를 공유하고 각각 컨테이너 레이어를 공유
* 동일한 이미지를 공유하믕로서 용량 절약이 가능하고 동일한 퍼포먼스를 낼 수 있음

<figure><img src="../.gitbook/assets/컨테이너 이미지 공유.png" alt=""><figcaption><p>동일한 이미지를 여러 컨테이너가 공유</p></figcaption></figure>

## 커맨드

* docker command -> container, image, volume, network, etc..
* 명령어는 docker로 시작
* docker \[대상] \[커맨드] \[옵션] \[인자]
* 컨테이너 실행 정지 생성

### docker container ...

| 명령어    | 설명                     | 옵션                             |
| ------ | ---------------------- | ------------------------------ |
| start  | 컨테이너 실행                | -i                             |
| stop   | 컨테이너 정지                |                                |
| create | 컨테이너 생성                | --name, -e, -p, -v             |
| run    | 이미지를 내려받고 컨테이너 생성 및 실행 | --name, -e, -p, -v, -d, -i, -t |
| rm     | 컨테이너 삭제                | -f, -v                         |
| exec   | 컨테이너에서 프로그램 실행         | -i, -t                         |
| ls     | 컨테이너 목록 출력             | -a                             |
| cp     | 컨테이너와 호스트 간 파일 복사      |                                |
| commit | 컨테이너를 이미지로 변환          |                                |

### docker image ...

<table data-full-width="false"><thead><tr><th>명령어</th><th>설명</th></tr></thead><tbody><tr><td>pull</td><td>이미지를 내려받기</td></tr><tr><td>rm</td><td>삭제</td></tr><tr><td>ls</td><td>목록 출력</td></tr><tr><td>build</td><td>이미지 생성</td></tr></tbody></table>

### docker 옵션

| 옵션     | 설명           |
| ------ | ------------ |
| --name | 컨테이너 이름      |
| -e     | 환경변수 설정      |
| -p     | 포트번호 설정      |
| -v     | 볼륨 설정        |
| -d     | 백그라운드 실행     |
| -i     | 컨테이너에 터미널 연결 |

